## Challenges:
- Handling large blobs
- Uploading large files:
	- We can directly uploading the data to s3 using pre-signed URL for security by passing the application server.
	- The POST API will create file metadata in DB with status as Uploading.
	- We will get a pre-signed URL as response using that URL we can able to upload the file to s3. once that is done the s3 will send a notification to backend service to update status as uploaded.
- Download file:
	- We can download the file using blog storage URL.
		- Request a pre-signed URL from s3 to download the file from s3.
	- We can store the file in CDN and access it also using CDN URL.
- Share files with other users:
	- We can have a metadata about the users list with whom the file is shared to.
	- But the lookup for files having access for user is slow as the list is not normalised.
	- We can store those information in cache.
	- Sync problem with DB and cache.
	- We can have a separate table in DB with user_file_access mapping in Dynamo DB.
- Sync files with local and remote:
	- Local to remote:
		- For client side syncing
			- We can use Os specific file system like file system watcher and FSEvents 
			- We detect the change and queues the modified file to upload locally.
			- It uses the upload API to do that.
			- Last write wins, No versioning is request.
	- Remote to local:
		- Maintain a SSE connection from each server to the client. 
		- that will send the updates about the changes made the file that is uploaded in the client.
- Handling large files:
	- We can use the chunking concept to chunk the large file and upload it using the s3 pre-signed URL.
	- Resumable uploads: Keep track of which chunks are uploaded successfully and what are not that give the progress.
	- We can do this by saving the progress in DB.
	- How to update the chunk update status in DB, We can use notification service that get triggered by s3 to DB on updating the upload status of each chunk.
	- How to verify the chunk upload 
		- We can verify in the server side verification.
- Upload steps:
	- File gets divided into 5  - 10 MB of chunks and will et a fingerprint for each chunk as well as the whole file which is used to check reusability and duplicates.
	- Client will check if the file with same fingerprint already exists, If yes, then check the uploading status and progress
	- If No client will trigger a request for multipart upload. 
		- Backend will call s3 to get the pre-signed URLs for each part of the chunk.
		- Then backend will return the upload Id with all the pre-signed URLS to uload each chunk of that file.
	- The client will send the file using the pre-signed URL of s3 to s3.
	- Once the upload of each chunk is done, The backend calls s3 to call completeMultipartUpload with all the aprt numbers and ETages. This tells s3 to assemble all the parts into single object
## System Overview:
- A cloud based file storage system that can be used to upload and download files and sync files across devices.
## Functional Requirements:
1. User should be able to
	1. Download file from storage.
	2. Upload file to storage.
	3. Automatically Sync files across devices.
## Non Functional Requirements:
- Scale of the system:
	- file support = 50GB
1. CAP theorem:
	1. By default we are choosing the high availability option for upload, download, sync files.
	2. Eventual consistency is acceptable.
	3. Low latency upload and download respectively (file size vary low possible)
	4. Support large files as 50 GB.
		1. Resume uploads.
	5. High data integrality sync accuracy.

## Core Entities:
~~~
FileMeadata 
File (raw bytes)
Users
~~~
## APIs:
~~~
POST /files
{
	file:
	fileMetadata:
	user; 
}

GET /files/{fileId}

GET /changes?since={timestamp}  
~~~

## High level design:
1. User should be able to upload file
	1. Client
	2. API gateway
	3. File service - on request will upload the file to s3 and then create file metadata in DB with s3 URL
	4. File metadata DB - stores the file metadata about the file that we are uploading.
2. User should be able to download file
	1. File service - on request file service will fetch the file URL from metadata and using the s3 URL client will download file from s3 directly.
3. Sync files
	1. Remote changed files:
		1. The files that are changed in Remote files can be identified using the update timestamp that we have in the file metadata.
		2. On connection check if the update timestamp is changed, If yes download the changed file.
	2. Local changed file
		1. Every client will have client application with them and a local storage.
		2. The local storage will have all the updated file information on sync, it will be uploaded to the s3.
**Reference**:
![[Screenshot 2026-03-07 at 6.55.21 PM.png]]

## Deep dives:
- System should upload files of 50 GB with low latency
	- Here instead of uploading file to server and then uploading to s3 we can directly upload the file to s3 using pre-signed URLS provided by the s3.
	- Here is the flow.
		- For the file that needs to be uploaded
			- Create file metadata
			- get a pre-signed URL from s3 that is valid for 10 to 30 minutes.
			- Send the URL to client in response.
		- The client can use this pre-signed URL to upload files from it.
		- This reduce double uploading.
	- And as we are using REST API, it won't have that capacity to have 50GB of data as request to upload.
	- And other way to reduce latency is to make file uploads in chunks.
	- Each chunk will have a unique id called fingerprint id which can be used to find the chunks of a file uniquely.
	- We can divide a single file into chunks and create file metadata with the chunk informations and store it in DB.
	- We can have status and update Ts in chunks as well to upload and download only the chunks that is updated in sync operations.
- How we can introduce the sync consistency
	- We can give a refresh button in UI to sync the data or we can make periodic polling to sync the data based on the update ts we have with us.

**Reference:**
![[Screenshot 2026-03-07 at 7.42.26 PM.png]]
