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
P0 is reserved for tasks that are not part of a project.
P1 = {T1,T2,...}
```

## B: backlog - upcoming Task in sorted order.
```
This is the input for the "scheduling algorithm" 
There should be a system/ algorithms that determines how to populate the backlog from projects.
//TODO
B = {p1:t2,p2:t7,p3:t3}
```

## D : day - the smallest scheduling interval. Composed of T’s
```
Let’s define the smallest unit of time to be one day. I think it makes sense to plan your day out and not your hours out. When you go to sleep, you kind of “reset” your upcoming day. (might come back and change this later on if needed.)
D1 = {...,p1:t2,...,p2:t7,...,p3:t3,...}  
```

# Test Run
Let’s see if we can plan something using this framework from a high level view.

## Framing this like an leetcodeish problem:


### input:
```json
Backlog = {
    p2:t7 = complete assignment 7 for online course~2hr, 
    p0:t3 = laundry~1hr
    p1:t2 = read chapter 2 of twlight~.5hr
    }
```

### output:
```json
schedule_03/16/19 ={
    "0":"sleep",
    "1":"sleep",
    "2":"sleep",
    "3":"sleep",
    "4":"sleep",
    "5":"sleep",
    "6":"sleep",
    "7":"breakfast, get ready",
    "8":"train ride to work", 
    "9":"work",
    "10":"work",
    "11":"work",
    "12":"lunch",
    "13":"work",
    "14":"work",
    "15":"work",
    "16":"work",
    "17":"train ride home",
    "18":"",
    "19":"p2:t7 = complete assignment 7 for online course",
    "20":"p2:t7 = complete assignment 7 for online course",
    "21":"p0:t3 = laundry",
    "22":"",
    "23":"p1:t2 = read chapter 2 of twlight",
    "24":"sleep"
}

```
### Thought process:
Okay so we have the backlog tasks. what other information do we need?
We need to know the times that are available. Because a person probably sleep lol.
So we need more context it seems, what if we have a profile of the user like as follows:
```
user_profile{
    "user_occupation":"Data Engineer", 
    "occupied_times":{
        "sleep":["0-8", "1-1:30"],
        "meals":["7:15-7:30"],
        "shower":"",
        "travel":"",
        "morning_routine":"7:30-8:00",
        "user_defined":"..."
    }
}
```

So besides the input backlog, we also need a list of blocked off times.
once we can fiugure out what times are available on the calendar, 
we can now place the tasks.


## steps to determine the output process: 
1. we first occupy all the occupied_times , 

```json
schedule_03/16/19 ={
    "0":"sleep",
    "1":"sleep",
    "2":"sleep",
    "3":"sleep",
    "4":"sleep",
    "5":"sleep",
    "6":"sleep",
    "7":"breakfast, get ready",
    "8":"train ride to work", 
    "9":"work",
    "10":"work",
    "11":"work",
    "12":"lunch",
    "13":"work",
    "14":"work",
    "15":"work",
    "16":"work",
    "17":"train ride home",
    "18":"",
    "19":"",
    "20":"",
    "21":"",
    "22":"",
    "23":"",
    "24":"sleep"
}

{"Backlog" : {
    "p2:t7" : "complete assignment 7 for online course~2hr",
    "p0:t3" : "laundry~`hr",
    "p1:t2" : "read chapter 2 of twlight~.5hr"
    }
}

```
Now we need to figure out how to place the tasks. 
We can just go ahead and fill the free hours up with tasks in order.
But that's very robot like. 

- maybe it's better to assign the read book task right before sleep.
- maybe we do the laundry after we shower.
- maybe we should complete the assignment after eating, so we have more enery.
- maybe there's a mood toggle like lazy,productive,super energentic.

We definitely need to tailor the scheduling experience for personal perference.
If we display a bad scheduling, users can:

1. click shuffle to get a new schedule
2. fix the task in the right order that they prefer, so our algorithm can learn their perference.

We see that user perference is definitely important here.
But we can start with just coming up with a way to get tasks from projects and sort it into the backlog.
Then place the task randomly in the empty time slots. 

//TODO
# projects

```json
{
    "projects":[
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



# FEATURE 

- user can shuffle schedule if they dont like it
- user can set their day to be lazy, productive or power to tune the tasks
- when there is empty time, we can recomend tasks to do base on user interest even when backlog is empty.


# SIGNUP 
- user preference / questions for personal profile.
