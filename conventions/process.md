# Process

## Prioritisation

Unless specifically raised the normal order of priority is:
* The board of your product team comes before the component board.
* The right-hand of the board comes before the left-hand side of the board.

## I just finished a task, what do I do next
Before starting a brand new task first help old task to be finished.
Following the priority order check:

### You product team board

1) There is any task that you implemented that needs your attention (eg: update with the latest master, update the staging machine).

2) There is any task that needs code review?

### React board

3) repeat 1 for the React board.
4) repeat 2 for the React board.

### Pick a new task

5) Check the task in TODO for your product team board, from top to bottom pick the first unassigned.

If there are **no unassigned** task in TODO in your product team board ask:

* Other developers in your team if you can pick some task assigned to them.
* The product manager of your team if some task on top of the backlog is ready to be worked with.
* The Frontend Tech Lead (this is your last resource).


## Picking up a new task

(aka The three amigos meeting)

When picking up a new task first thing to do is to verify that you have everything to work with that task.

Read the Jira card carefully, if some field is missing or is badly written ask the right person how to fix it. If the card has not been sufficiently discussed in a team meeting involve the people that needs to be involved (no more no less).

* Ask the product manager for a description of the functionality
* Ask the designer for the latest design and all the assets that you may need.
* Verify with QA how this task is going to be tested.
* If the task needs some back-end support to ask a backend developer.

The benefit of this phase is:

* Builds a shared understanding about the intent of an increment of work.
* Identifies misunderstandings and confusion early and allows learning to happen sooner in the delivery of an increment of work.
* Provides a reasonable guard rail for the number of people who should be involved in discussions about any given increment of work.
