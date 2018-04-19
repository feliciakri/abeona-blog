---
title: "How Work Gets Done in Abeona"
date: 2018-04-19T01:07:12+09:00
author: Octavio Putra
draft: false
tags: ["Octavio", "Development", "Git", "Sprint 2"]
---

It's been 11 weeks since we started this project. Its been through lots of ups and downs throughout the process. With lots of people working on a single project, a good workflow is a must in order to maintain a high rate of productivity. Which is why today I want to talk about how work gets done in Abeona, particulary on Git and the Git flow.

## Git

Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency. Version control systems are used by developers to maintain and monitor the changes that are made into the software. They are also sometimes called repositories, because it stores large amount of source code, for software or for web pages, that is kept, either publicly or privately. There are numerous kinds of Git out there. We've been given a Gitlab repository as our main repository. Gitlab is nice because it's got a really good integration system. Connecting issue management, version control, code review, CI, CD, and monitoring into a single, easy-to-install application, Gitlab ensues that a team can work quickly and efficiently.

## Git Flow Branches

There are 5 main branches that we use in our repository:

#### Master
The master branch is the main branch that stores the source code for production environment software. Production environment software is where the real-time staging of the program are executed to perform day-to-day basis. This is the environment that is released to the market. Which is why this important branch can only be merged to after the sprint review.

#### Sit_uat
The sit_uat branch (System Integration Testing & User Acceptance Testing) is a branch used for the development environment. This branch is used to store the program in a somewhat complete state, combining the branches that are being worked on the current sprint, and also acts as the main testbed for integrating said branches. This branch will be presented to the product owner during the sprint review.

#### User Story Branches
The user story branches is a branch used for individual user stories. Their names follow the format usXX_storyname. These are the branches where daily work are submitted. When the user story deemed complete, it is then merged to sit_uat branch after receiving approval from at least 2 team members.

#### Coba_coba
The coba_coba branch is a branch where developers can try things out, as the name suggests. It is primarily used for integration testing and environment setup. It can also be merged into sit_uat if received enough approval.

#### Hotfix & Coldfix
The hotfix branch is the branch used to fix bugs in the master branch, while the coldfix branch is used to fix bugs in the sit_uat brnach. The coldfix branch is also the rollback branch when a user story is rejected by the product owner.

## Software Architecture

In a world where the user interface logic tends to change more often than the business logic, the desktop and Web developers needed a way of separating user interface functionality. The MVC pattern was their solution.

- Model — the data layer, responsible for managing the business logic and handling network or database API.
- View — the UI layer — a visualization of the data from the Model.
- Controller — the logic layer, gets notified of the user’s behavior and updates the Model as needed.

You can see how these elements interact in the diagram below

![MVC Diagram](/img/mvc.png)

The above architecture means that both the View and the Controller depends on the Model. This ensures that either has control of the data. This also means that the model can be separated and tested individually. There are also several variants to MVC depending on the activity of the Model:

#### Passive Model

Passive model architecture is where the model can only be altered by the controller. The controller modifies the model based on the users actions. The view meanwhile only needs to grab the data again from the model when the controller requires it

#### Active Model

Active model architecture is where the model can be altered by both the view and controller. In order to do this, an observer pattern is needed in order to notify the view and other classes about the changes. The view then implements the observer interface and registers as an observer to the model.

#### MVC in Android

Nowadays, the community has found an ubiquitous way to use MVC in android. The Activities, Fragments and Views should be the Views in the MVC world. The Controllers should be separate classes that don’t extend or use any Android class, and so are the Models. To connect the Controller to the View, the Controller needs a reference to the View. The easiest way of doing this is to have a BaseView interface that the Activity/Fragment/View would extend. So, the Controller would have a reference to the BaseView.

The MVC pattern highly supports the separation of different concerns in a software. This in turn increases the testability of the software and easier to extend.
