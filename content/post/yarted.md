---
title: "Yet Another React Testing Documentation"
date: 2018-03-23T04:04:49+07:00
author: Felicia
tags: ["Felicia", "React", "Sprint 1"]
---

Abeona admin react uses Jest for component testing and enzyme for snapshot testing.
Some of the problems encountered is the admin page is mostly consists of REST API Client and even though there's Jest mocking feature to simulate async API calls, testing API Client is not as simple.
Another problem is Jest and Enzyme uses different ECMAScript. This is overcame with the help of babeljs.
Linting uses ESLint with AirBnb rules, which seems like the most progressive one, fairly easy to maintain and adopted by the industry.

## DRAFT ##
