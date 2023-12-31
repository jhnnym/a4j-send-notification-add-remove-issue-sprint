# a4j-send-notification-add-remove-issue-sprint
"Automation for Jira" rule that sends notification when an issue is added or removed from an Open sprint

The solution works in both Jira Data Center and Cloud versions. 

## Premise 
a. This has not been tested with parallel sprints enabled
b. There can be only one sprint change per rule triggered.  
c. The UI and REST API allow only one sprint change per issue update.
d. Tested using Jira 8.20.10 and A4J 9.0.3 with limited scenarios
e. An issue's sprint field can have multiple closed (completed) and (zero or one active or future) sprints.

When sprint has changed the A4J smart value fieldChange will have the following:

"{{fieldChange.from}}"  = the issue's previous sprint IDs 
"{{fieldChange.to}}"    = the issue's current sprint IDs

## High level description of steps in the rule:

1. fromList = list of previous sprints
2. toList = list of current sprints
3. mergedList = fromList union toList
4. removedSprintId = mergedList - toList
5. addedSprintId = mergedList - fromList
6. if a sprint was added then

     a. get the added sprint record via REST API

     b. if the sprint is active (open) then send 'add' notification

7. if a sprint was removed then
     a. get the removed sprint record via REST API
     b. if the sprint is active (open) then send 'removed' notification

We have two IFs to cover the scenario:
  from: [closed-sprint, active-sprint] to [closed-sprint, future-sprint]

We don't know the changed sprint is active or future until we fetch the sprint record. Also, having two IFs will cover the parallel sprint scenario where we can have more than one active sprint at the same time.

## Files

### Jira Data Center version
- dc-a4j-rule-send-notification-when-issue-add-remove-from-open-sprint.json - exported rule
- dc-rule-part-[123]-of-3.png - screenshot of rules

### Jira Cloud version
- cloud-a4j-rule-send-notification-when-issue-add-remove-from-open-sprint.json - exported rule

## Demo videos

### Jira Data Center
[![Jira Data Center Demo](https://img.youtube.com/vi/uAXXAUwJA9c/maxresdefault.jpg)](https://youtu.be/uAXXAUwJA9c?vq=hd1080)

### Jira Cloud
[![Jira Cloud Demo](https://img.youtube.com/vi/DYwTEXxO5oo/maxresdefault.jpg)](https://youtu.be/DYwTEXxO5oo?vq=hd1080)


