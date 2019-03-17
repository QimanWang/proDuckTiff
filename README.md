# Problem Statement:
Failing to plan is planning to fail. However over planning and not doing enough is just as bad.
I know many people have a hard time planning their days and thus not being as productive as they can.
I want to create a framework that can be used as a guideline for planning tasks.
Then implement that framework and create some kind of application that can auto generate your daily tasks.
If one's too lazy to plan their day, they are not productive and thus stuck in a never ending loop of Procrastination.

# Thought Process:
When I’m presented a problem to solve, I like to start from first principles. Let's built this framework from the ground up. Let's start by defining some entities and definitions. 

# Baseline:
There is only so many hours in a day to do stuff. The objective here is to allocate our daily resource (in this case, time and energy)to get the most value out of it. It’s similar to the Computer Science knapsack problem. We want to look at a list of tasks and pick the highest priority/ value tasks. In this case I think priority applies to things that has to be done, like tasks that have a deadline. Value would be applied to things where we have the freedom to choose and thus leaving us to pick the tasks with the highest return in value. 

# Definitions:

There are two things that are interacting here. Work and Time.
Let's define the smallest unit of work to be one Task. 

## T : task - the smallest unit of work. 
```
T1, T2 … are each tasks. Tasks should only take fraction of an hour to a few hours.
```

## P : project - composed of T’s
```
An project , goal, milestone that takes couple of tasks to reach.
P1 = {T1,T2,...}
```

## B: backlog - upcoming Task.**
```
B = {p1:t2,t2:p1,t5:p3}
```

Let’s define the baseline to be one day. I think it makes sense to plan your day out and not your hours out. When you sleep, you kind of “reset” your upcoming day. (might come back and change this later on if needed.)

## D : day - the smallest time interval. Composed of T’s
```
D1 = {T1,T2,T3,...}  Day1 contains task1,task2,task3,etc.
```

When planning we always want to start from the high level. we need to see the bigger picture and the expected outcome.
Let’s see if we can plan something using this framework.



Let's frame this like an leetcodeish problem:
given a list of input task in the form of a backlog. each task can have properties. 
(we can modify the input as we go).

output: schedule for the next n days. let's start with n = 1 for now.

let's define our input 
Backlog = {Wash poppy (my dog) , get AWS certification, Workout. }

#example output 
output:{
    0:do x,
    1:do y,
    ...
    24:
}

# some cases
## 0
input : {} #no task 
output:{
    0:,
    1:,
    ...
    24:
}

### 1
input: tasks:{ "T1- do laundry":"1hr" }, user_profile.NA_times,
hmm, okay so we have 1 task. what other information do we need?
we need to know the times that are available. Because a person probably sleep lol.
so we need more context it seems,
what if we have a profile of the user like as follows:
user_profile{
    "user_type":"employee", 
    "occupied_times":{
        "sleep":["23-7", "1-1:30"],
        "meals":["7:15-7:30"],
        "shower":"",
        "travel":"",
        "other":{
            "get_ready":"7:30-8:00"
        }
    }
}

there is too much cases because people are different and it's hard to take into account.
But there is probably some kind of pattern here.
so input needs a list of blocked off times.

once we can fiugure out what times are available on the calendar, we can now place the tasks.

example : output
output:{
    0:sleep,
    1:sleep,
    2:sleep,
    3:sleep,
    4:sleep,
    5:sleep,
    6:sleep,
    7,breakfast, get ready
    8,laundry
    9,
    10,
    11,
    12,
    13,
    14,
    15,
    16,
    17,
    18,
    19,
    20,
    21,
    22,
    23,
    24:
}

## 2

if we have 2 tasks
input: tasks:{ "T1- do laundry":"1hr", "T2- hand in hw due today":"1hr", "T2-work":"9-5" }, user_profile.NA_times,

tasks can be of different types: 
{
"task_types":{
    "1": "one time",
    "r": "reoccuring"
    }
}
{
"task_values":[
    "required","high","medium","low","meh"
]
}

## should look something ike this
tasks should also have priorities over each other, giving this input, 
```json
{
"input":{
    "tasks":{ 
        "T1": {"what":"do laundry","time":"1hr","priority":"meh"},
        "T2" :{"what":"hand in hw due today","time":"2hr","priority":"high"}, 
        "T3":{"work":"8:30-5","priority":"required"},
    "NA_times/user_profile.NA_times":[[23-7,7-8,20-21]],
}
```
}
work is kind of a reoccuring na_times, it's debatable,
actually work should be it's own task and not reoccuring cuz we need the transit time included.
basically the task time should include the transit times.

## steps to determine the output process 
1.we first occupy all the na_times in this case, 
```json
{
"output":{
    "0":"sleep",
    "6":"sleep",
    "8":"",
    "8:30":"transit to work",
    "9":"",
    ...
    "17":"",
    "18":"",
    "19":"",
    "20":"",
    "21":"",
    "22":"",
    "23":"",
    "24":"",
}
}
```
2. then we sort the tasks by priority.
```json
{
    "T3":{"work":"8:30-5:30","priority":"required"},
    "T2" :{"what":"hand in hw due today","time":"2hr","priority":"high"},
    "T1": {"what":"do laundry","time":"1hr","priority":"meh"}     
}
```
we will put t3, with the specified time first, 
then t2, if no specified time, we can just put it in the next available time.
this seems logical right now hmm.
```json
{
    "output":{
        "0":"sleep",
        "6":"sleep",
        "8":"",
        "8:30":"transit to work",
        "9":"work",
        "17":"work",
        "18":"Hw",
        "19":"hw",
        "20":"dinner, shower",
        "21":"laundry",
        "22":"",
        "23":"sleep",
        "24":"sleep",
    }
}
```
okay so all the tasks are done. we still have 
2 slots still open ,
22:00-23:00, 8:00-8:30

let's go into our shorterm lists,
and add some tasks from there.
let's say we have 3 projects,
```json
{
    "short_term_projects":[
    {"stp1-aws certification":{
        "priority":"high",
        "time_spent_so_far":"34hr",
        "mile_stones/completed_tasks":["ch1","ch2"],
        "task_queue:":["ch3","ch30"],
        "marker":"some market that tells when the next task is"
    }},
    {"stp2-XXXX":{
        "priority":"low",
        "time_spent_so_far":"34hr",
        "mile_stones/completed_tasks":["ch1","ch2"],
        "task_queue:":["ch3","ch30"],
        "marker":"some market that tells when the next task is"
    }},
    {"stp3-yyyyy":{
        "priority":"high",
        "time_spent_so_far":"34hr",
        "mile_stones/completed_tasks":["ch1","ch2"],
        "task_queue:":["ch4","ch30"],
        "marker":"some market that tells when the next task is"
    }}    
    ]} 
}
```
we would sort by the highest prioritys, and get the next task in the task queue,
we then pop the task out and place it.
* maybe have a pop up asking user to pick which project they want to work on today if there are multiple choices.
or randomly display and if the user don't liek it, we can choose the next available task.

or maybe we can shuffle the tasks that are not set in stone, like laundry, homework, etc.


## maybe there are no tasks in short term projects, 
we can go into th long term and pop some tasks out and create short term tasks.



# Components:



