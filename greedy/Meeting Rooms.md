#revision-1 #revision-2  #revision-3 #revision-4 #revision-5 #CanBeImplementedWithoutRevisit  
## Problem Link:
https://www.scaler.com/academy/mentee-dashboard/class/313436/assignment/problems/168773/?navref=cl_pb_nv_tb
## Understanding:
- Given a list of intervals represents the meeting duration start and end
- Find the minimum number of rooms required to finish the meeting
## Input and Output:
![[Screenshot 2025-12-31 at 3.51.33 PM.png]]

![[Screenshot 2025-12-31 at 3.51.51 PM.png]]
## Problem Constraints:
![[Screenshot 2025-12-31 at 3.52.20 PM.png]]
## Approach:
### Brute Force:
### Optimised:
- Starting time of every meeting is important as we need room at the starting time of every meeting.
- Hence sorting the list based on start time.
- Allocating first meeting in one room.
- Iterate the meetings
	- Check if early finishing room can be reused for the current room
	- If yes reuse it,
	- If no allocate a new room
- Return the total number of rooms allocated.
- **Time Complexity:** O(N log N)
- **Space Complexity:** O(N)
### Reference:
![[WhatsApp Image 2025-12-31 at 4.35.06 PM.jpeg]]
## Code:
```Java
class Schedule{
	int start;
	int end;
	Schedule(int start, int end){
		this.start = start;
		this.end = end;
	}
}

private int findMeetingRoomCount(int[][] meetings){

	List<Schedule> meetingSchedule = findSchedule(meetings);

	meetingSchedule.sort(Comparator.comparing(schedule -> schedule.start));

	PriorityQueue<Integer> scheduleOrg = new PriorityQueue<>();

	for(Schedule schedule : meetingSchedule){
		if(scheduleOrg.isEmpty()) scheduleOrg.add(schedule.end);
		else if(scheduleOrg.start >= scheduleOrg.peek()){
			scheduleOrg.poll();
			scheduleOrg.add(schedule.end);
		}
		else scheduleOrg.add(schedule.end);
	}

	return scheduleOrg.size();
}

private List<Schedule> findSchedule(int[][] meetings){
	List<Schedule> scheduleList = new ArrayList<>();
	for(int[] meeting : meetings){
		scheduleList.add(new Schedule(meeting[0], meeting[1]));
	}
	return scheduleList;
}

```


