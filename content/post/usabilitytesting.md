---
title: "Usability Testing the Abeona App"
date: 2018-05-17T01:26:09+09:00
draft: false
author: Octavio Putra
tags: ["Octavio", "Design", "Usability Testing", "UX", "Sprint 3"]
---

###Usability Testing
Usability testing is a user experience test where volunteers try to use the app through a series of scenarios that mimic real life use. These tests are intended to find out how a user interacts with a digital product and find out design problems that ruins the user's experience.

### Usability Testing for Abeona
The method I use for usability testing Abeona is **Hallway Usability Testing**. This method relies on using the person near you as testers for your app. In my case, I've invited friends from my dorm in order to participate. For Hallway Usability Testing, the recommended number of users in a single interaction is 5 people. Jakob Nielsen has a fantastic article on why having 5 users test each iteration brings most bang for the buck. Anything more than that, it yields only diminishing returns. And based on the formula N(1-(1- L)n) in that article, you could uncover 85% of the issues in your design with just 5 users. But, since there are only 2 other people currently living in my dorm, the remaining 3 people shall be found at a later date. Because the app is not production ready, a clickable mockup is used.

### Testing Scenario
I've managed to gather 2 interviewee for this testing. The first one is Muhammad Zulfikar Arief from FMIPA UI, and Fariz Hussein from FT UI. Both of them have no prior knowledge of the app.

There are 3 main goals that I want users to accomplish:

* Save and Edit Preferences
* Get Recommendations
* Look at nearby POIs

With that in mind, I've devised a scenario according to the main goals.
First of all, some context:

*You are on a vacation to Raja Ampat. You are backpacking with a bunch of friends who don't travel a lot. You also don't have a lot of money, therefore you can only afford to eat at cheap restaurants. Your accommodation and transportation tickets are all set. Its nighttime and you are in your hotel. Your friend just told you of this app to find food.*

From there the tasks are then given to the interviewee:

*User goal: Save their preference and get a recommendation*
*User Task: Ask Abeona for a recommendation.*

This is a fairly simple, but crucial task as it deals with the main flow of the app. When Zulfikar tried it, he said that he had concerns over the bubble onboarding. He tells me that it might be difficult to find something he specifically prefer in mind, especially because food is such a diverse subject. Fariz doesn't really mind the onboarding and said that it was alright. Both of them later navigated through the recommendation and product page without any problems.

*User goal: Edit their preferences*
*User Task: Say that you ate one of things that you chose earlier(in this case, satay) and it was horrible and you are sick of it now. Change the preference so that thing won't show up in your recommendations*

While doing this task, both of them took quite a long time to notice the navbar on the bottom. Other than that they have no problems with navigation. But, they question whether this was necessary as they assume that the recommendation system will give them options anyway.

*User goal: Look at Nearby POIs*
*User Task: Find out what places around you*

When they first heard about this task, they were confused as to the purpose of this task. They think that the recommendation system from before is what I'm talking about. This might be due to poor explanations from my part beforehand. As for navigation, again they took a long time to notice the bottom navbar. Zulfikar in particular have been distracted by the hamburger button that was supposed to be the scrapped nav-drawer. Other than that, there was no problems.

### Fixing the Problems
From the tests conducted, it is concluded that the navbar doesn't have enough visual impact. The navbar then needs to have thicker icons with text captions to highlight its functions. The nav-drawer will also be scrapped as it is distracting and that there are only 3 main navigations in the app, therefore a nav drawer would be an overkill. The search, or nearby places, option will also be added to the home screen scroll menu buttons, as testers usually look there first before looking down to the navbar. A some sort of short tutorial like explanation will also be needed as to emphasize the difference between the services provided inside the app.

### Error handling
I would also like to talk a little about error handling from the UI perspective. While an error occurs, the backend passes an error code. The codes are documented in swagger. As the 8 golden rules suggest, offer simple error handling. Which is why the error codes that are passed are turned into a message that is understandable by humans. That way, the users can take steps to fix the problem. The messages are also shown intuitively where the error happens so that users are focused on what happened and how they can resolve it.
