---
title: "Sprint Review and its Lead Up"
date: 2018-04-05T09:30:50+07:00
author: Wibisana Bramawidya
tags: ["Wibisana", "Golang", "Sprint 1", "Sprint Review", "Rant", "Git Flow"]
---

> Mistakes were made. hooo boy, were mistakes made.

## Mistakes

Leading up up last week's sprint review, a lot of mistakes were made. A **WHOLE** lot of them.

#### Mistake no. 1

The **NUMBER 1** mistake I had the most gripe with was the project requirements. Trying to achieve the project's FORCED *Definition of Done* was extremely taxing, taking most of my development time, over 50% of the total development time. The targets we had to achieve were, among others:

- GREEN pipeline (*okay, completely understandable*),
- Separate database seeding for development, staging, and production (*annoying, but I can see why it's necessary*),
- Documentation when needed (*key word, WHEN NEEDED, unlike what GOLINT thinks* :weary:),
- Automatic deployment (*not necessarily the best thing, knowing that some logical errors may be left in*), and
- 100% code coverage (*OKAY WHAT THE ACTUAL !&%!&@%#*).

Dear god those took a long time to finish up. Among those, I failed to finish 2, namely the code coverage and documentation. Towards the sprint review, I realized that there were a bunch of features defined in the sprint goal that were not met. This lead to the finished (*not really, but you know*) but rushed MVP.

#### Mistake no. 2

Secondly, there was also the fact that the Git flow was extremely restrictive, the problem I still have is with how conflict handling is managed. At my internship, we handled merge conflicts by creating a new branch, with a specific naming convention, and branched from the target branch. Then, using `git pull` (*actually using `git merge --no-ff`, but that was too much of a hassle*) to merge the source branch and resolve the resulting conflicts. This, I think, is really beneficial as mishandling a merge can be easily reverted and it keeps that target branch clean.

*Buuut*, since the Git flow **IS** defined in the project requirements, nothing I can do about it, I guess. During the first sprint, I had worked according to the initial definition of the git flow, which I think is horrible. The initial git flow was, that each user story is to be a branch taken from `sit_uat`, then, **ALL** tasks derived from that user story, no matter how many people are working on it, is to be worked on directly on that branch. Since each user story can be broken down to multiple tasks, and the tasks can (and will) be taken up by different people, merge conflicts will constantly come up. The case that comes up time and time again, is when someone pushes a `[RED]` commit, and another wants to push a `[GREEN]` one, the `[GREEN]` commit cannot pass the pipeline since another part of the code is still `[RED]`.

Well, that's all in the past now, since the Git flow has been updated. It is now as how I asked (not directly to the professors) it to be at the beginning of this project, where each task will branch from the user story, leaving each task branch to be worked on by one person and one person only, making the development so much easier. When the task owner finishes their task and fulfills the *Definition of Done*, somewhat, they can then merge their task branch to the user story branch, letting other people working on that user story to use the feature they finished.

## In the End

Sorry this post turned out to be a rant on this project. Since the sprint review just happened, it is still fresh in my mind, along with the complaints and gripes with the project.

Thanks for reading anyway and listening to me ranting a little, *thankfully this is much shorter than my usual posts*. See you when I write again, I guess, じゃあまたね :sunglasses:
