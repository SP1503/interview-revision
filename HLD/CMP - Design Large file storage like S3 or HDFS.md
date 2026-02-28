### System Overview:
- Why large storage when DB is present?
	- We can store large file that requires no updates and can be stored as dump. Pointed updates wont happen. No update = No indexing
	- Hence frequently accessing dump files can be stored in CDN with geographical edge servers. 
	- Simple large file storage is required 
		- To store files that are more than 128MB.
		- Point update is not required.
	- Some examples of files are below
		- videos
		- images
		- log files of 10 years duration
### Functional Requirements:
- A storage for files in terms of 10 TB = 10 * 10 ^ 12 bytes.[Therefore we need sharding]
- The storage is durable. The data should be always available. [Therefore replication is required]
- Upload and download of files should be possible.
- It should e possible that analysis of this large files can be done

### Non Functional Requirements:
- **PACELC**
	- What is the data here ?
		- The data that we have uploaded to the system.
	- What is consistency here?
		- Uploading a data
			- The uploaded version of data should be replicated immediately other wise the client will think that the upload is not happening
		- Editing a data
			- The edited version should be available immediately otherwise misconception will happen.
		- We need immediate consistency.
	- What is availability here ?
		- The system should be able to upload the file whenever the client wants to
	- What is latency here ?
		- The client should be able to upload and download files from the system with high latency as we are dealing with high data.
- The system should be have immediate consistency.
- The upload and download can have pause feature.

### High level design 
- **Can we store 100TB data in a single server**?
	- No, it can be 
		- a SPOF and Bottleneck
		- A single machine cannot handle 100TB data in RAM to do analytics
		- If a file gets corrupt entire 100TB data gets corrupt
	- Hence we need to split the file into chunks
	- We need to maintain metadata about which chunk needs to be present in which machine

 - **How long can be the chunk of the given file** ?
	 - Smaller size like 1KB.
		 - 100 TB / 1 KB = 100 * 10 ^ 9 = 100 Billion chunks
		 - Each metadata is having 200 bytes 
		 - Total metadata for 200bytes * 10 ^ 11 = 20 TB data
		 - 20 TB is huge just to store the metadata
	 - Larger size chunk 100 GB
		 - 100 TB / 100 GB = 100 * 10 ^ 12 / 100 * 10 ^ 9 = 10 ^ 3
		 - 10 ^3 chunks will be present 
		 - metadata overhead will also be small
		 - While uploading, Downloading, processing the chunk data we need to fit 100Gb data in RAM which is not possible solution.
	- Hence need to find a sweet spot between 1KB to 100 GB
		- So that metadata required to chunk it out is also less
		- A single chunk can be easily fit in a RAM.
	- HDFS 1.0 used 64 MB chunk size
	- HDFS 2.0 used 128 MB as the chunk size.
	- For chunk with size 128 MB
		- 100 * 10 ^ 12 /  100 * 10 ^ 6 = 1 Million entries
		- 10 ^ 6 * 200 = 200 MB metadata is okay

### Terminologies in HDFS
- Node naming server:
	- This is a server that knows the chunks of single file stores in which machines.
	- Is we need sharding? No as the data is small a single machine is suffice to store this much of data
	- Is we need replication ?
		- yes, as the data is critical we need replication to have backup of the data
		- We need multiple read replicas(Master - slave architecture is used)
		- We need strong consistency for this metadata as well.
- Data nodes:
	- These are the machines in which a chunk of actual data is stored.
	- Each chunk in itself will be replicated across multiple data nodes present in the hadoop cluster.
	- The distribution is maintained by the name server.
- Replication of data
	- We have two types of data
		- Metadata
		- Actual data (chunks)
	- Chunks:
		- Replicated between multiple machines decided by the name nodes.
		- Hence initial replication is done.
		- How to maintain consistency across multiple replication of chunks:
			- In case of chunks no updates will happen, It is immutable
			- Hence we there will be always consistency
			- If a new version of a file is uploaded then new chunks of the new version will be created and pointed by the old meta data.
			- Old chunks now will be deleted by garbage collector.
			- If file appending is done that is a new chunk that is going to happen, Hence a new chunk will be added with its respective metadata.
	- Metadata:
		- Metadata replication is done using master slave concept.
		- In case of partition, name nodes cannot talk to each other.
		- Hence write will not be allowed for the partition time but we can read the data as metadata is present in the current available name nodes. 

### Rack Aware Algorithm
- In server room 
	- We can see different racks holding a collection of servers that is connected with a single router and single power supply.
	- That single router and single power supply can become single point of failure.
	- Hence if all the replicas of a single chunk is present inside the single rack then when the single rack becomes unavailable all the replicas of that single chunk becomes unavailable.
	- Therefore
		- Name node ensures that at least 1 of each replica is stored in different rack -> rack aware distribution
		- Also ensures that at least one replica for each chunk is stored in different availability zone -> availability zone aware distribution
		- Rack == availability zone

### Upload:
- The user will upload the file from local device to the backend sever
	- This upload also happens using chunks but this chunks are different from the HDFS chunks.
		- To show upload progress
		- If upload gets interrupted then need not to upload from 0.
	- Now the file is stored in HDD of app server.
	- Now the client of HDFS that we have in out app server will send the data from HDD to HDFS server as continuous stream of data.
	- This HDFS server will transform this continuous stream of data into chunks and will send it to the Data nodes using node naming server.
- What if there is a network partition in between the backend app server to the HDFS app server?
	- File upload will fail and the upload will rollback and start from scratch.
- What if the upload to data nodes failed for a particular buffer?
	- Retry mechanism is implemented so that multiple retires will take place.
	- Then we try to update to different data nodes.
- What if the size of the file is ood number ?
	- We can divide and the last chunk can have small value.
	- Example:
		- File size = 200 MB
		- Chunk size = 128 MB
		- last chunk size is 72 MB.
- **Reference**:
![[Screenshot 2026-02-25 at 5.15.29 PM.png]]

Download:
- The download request comes to the app server of HDFS
- The server contacts the node naming server to get all the meta data related to the requested file.
	- Exactly how many chunks, what chunks
	- For each chunk how many replica and what are they
- The app server will start downloading the first chunk
	- Read from any replica 
	- If any replica is down, then choose other replica
- If will store this chunk in the memory buffer
- It will start streaming this data down to the client
- In parallel will download next chunk of data.
- No entire files gets downloaded, instead it will be done by chucks and chucks
- **Reference**:
![[Screenshot 2026-02-25 at 5.34.02 PM.png]]