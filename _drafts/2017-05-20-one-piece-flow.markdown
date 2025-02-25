---
date   2010-09-19 17:00:00
---


General summary of the theory in manufacturing.. then into an example
Sort of like a fable. Multiple parts
- theory
- setting us up to identify the constraint
- identify the constraint - it's QA
- Improve the constraint and look again, is it Dev now?
- Summary - what to do next (e.g. map your system and start improving bit by bit)


Use sample Kanban boards to demonstrate each stage

----------------------------------

Systems thinking - Aiming for one piece flow

This isn't anything revolutionary, people have been telling us about continuous integration for a long time - release small features often etc.
This post talks about this in more detail. Why are we aiming for this? Where did it come from? What are the common issues with this approach?

manufacturing
what is a constraint
stacking things up at the constraint - why is this bad?
 - don't find errors quickly enough, too many things are broken by the time we find out - find the error early and stop the process
 - masks true flow, each stage before the constraint feels like they are moving quickly, but the output of the system is still low

Equivalent in software
- We build a sprint's worth of work, move it to QA where it is tested by the QA team, then a couple of days later we come back with a bug. By this point, there are already 20 items in the queue for QA in the next sprint, and you've forgotten what you ate for breakfast yesterday, so you have no chance of remembering the bug fix.

Once piece flow

Toyota aim for 'one piece flow' (check and elaborate)
The idea is that work flows through the system from start to finish in one go, it isn't stuck anywhere at a constraint.
Of course, it's sensible to have some buffer - we don't want QA sat waiting for you to push a bug fix, for example. The size of that buffer is dependant on your organisation. Perhaps each team (dev, qa, analyists) will be allowed to work on 5 items at the same time.

WIP limit
What is it? (explain)
If you have a Kanban board, this is the purpose of your WIP limit , it is an artificial mechanism to prevent you from queuing work up at the constraint.
Let's assume that we're trying to move towards this model, so we set a WIP limit of 5 items for our analyist, dev and QA teams.
Each team cannot pass an item to the next team until the receiving team has room in this WIP limit.

** HOW TO SET WIP LIMITS? **

Within a few days, we notice that the dev team are often idle waiting for space in the QA team's queue before they can push their feature and start something else. Remember, if they have 5 items in their queue, and there is no room in QA's queue to pass something on, they cannot start something else.
This sounds counter-productive, but it's actually very helpful.. it's identified the current constraint in the system.
Perhaps there aren't enough people in QA. Maybe there are too many manual tests and not enough automation. Either way, we need to work on the bottleneck as a priority.
> Any improvements made not at the bottleneck are wasted (source for this)

The temptation is to up the WIP limit of QA, to allow Dev to keep producing things. This feels like the right thing to do, as it keeps the dev team busy where they were previously idle, but it's only masking the problem.
It doesn't matter how many things the Dev team produce, the output of QA is only 5 items per-day on average. That means the total output of the system is only 5 items per day. The project won't be delivered any quicker, all we're doing is overloading the QA team.
We're picking on QA here, but this could equally be developers waiting for things to hit their queue because they don't have specs, which in turn could be because the client isn't reviewing them early enough. We need to model the whole system to find the constraint.

Now we identify why the constraint exists.
Are we doing too much manual testing and not enough automation? Do we need more QA people?

Constraints will move over time. In our example it's QA. Perhaps we improve our automated testing,so that the QA team are writing automated tests, and only doing exploritory manual testing. Now, QA isn't our backlog anymore. 

QA are sat around wondering what to do with their time, whilst the Dev work queue is constantly full. We have a new constraint - the Dev team.
Perhaps it's too time consuming to get work from Dev to a place where QA can test it. Perhaps we just need more developers?


The core premise of this is that we need to let go of the thinking that everybody needs to be busy all of the time. In theory, in a perfect system everybody will be working in tune, and work will flow through the system in such a way that it arrives at the next 'station' at exactly the right time.. but this isn't the real world.

You _will_ have a constraint, the aim is to put the processes in place to identify it so we can optimise it. This isn't a process that finishes, this is continuous improvement with a purpose.
Instead of saying 'hey, it would be nice if we could run UI tests automatically', we make decisions using data. If testing isn't the bottleneck, improving it won't make any real difference to the output of the system. It might feel productive, but it isn't helping.

We need tools to be able to visualise this. In this post, we're using 'kanban boards', but any method that helps identify the constraint, the important thing is that we have objective data to make that decision.