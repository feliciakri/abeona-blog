---
title: "Benefits of Test Driven Development"
date: 2018-04-05T00:01:26+09:00
draft: false
tags: ["Octavio", "Development", "TDD", "Sprint 2"]
---
Test driven development has became the hot topic to discuss on our team. Our hackers keep tearing their hair out trying to churn out a 100% code coverage tests often to no avail. While testing might seem like an obvious task, its implementation often leaves a lot of people baffled and often leave it out entirely, especially if a high code coverage is demanded. Even so, don't skip out on TDD as it brings along a lot of benefits with it.

## Test Driven Development?

Test Driven Development is the practice of writing a test for a piece of required functionality, before writing any implementation code. This test should fail when first run, and then, you write the code to get it to pass. It doesn't have to be the most perfect code, just so long as the test passes. Once it does, you can then safely refactor your code. This way of coding ensures that the programmer knows what they're going to code before actually coding its implementation.

To do TDD is to discipline yourself. You cannot be tempted to create implementation before creating the tests, it doesn't work that way. This might seem counterproductive, especially on tight deadlines. But by understanding the reasons behind TDD, you can actually see where this system fits your schedule.

## Benefits of TDD

### Acceptance Criteria

Here in PPL course, we have a definition of done regarding how a user story can be merged into master. TDD can act as one of the acceptance criteria. For us, the minimum is 100% code coverage, although it can vary on special cases. By using TDD, You can use it as a means to know what you need to test and then, once you've got that list in the form of test code, you can rest safely in the knowledge that you haven't missed any work.

### Focus

TDD helps you narrow your focus. Everyone has been in that happy state where an error pops up and you do everything in your power to make it go away. Failed tests can also create the same "excitement" that you've always love in coding. It focuses you on the smallest of implementations rather than the application as a whole. This in turn makes you code step by step and not get caught in an endless loop of a feature creep.

### Interfacing & Cleaner Codes

Because of the nature of tests that tests a single piece of functionality, to write a test in TDD means you have to think about the public interface that other code in your application needs to integrate with. From the perspective of the test, you're only writing method calls to test the public methods. This means that code will read well and make more sense. Since your tests are only interfacing with public methods, you have a much better idea of what can be made private, meaning you don't accidentally expose methods that don't need to be public.

### Safe Refactoring

If you pass your test, then that means you don't have to worry about doing anything wrong while refactoring. This gives a security net where the test cases have your back. This could also apply when you work with someone else's code. You don't have to TDD all the way through it, just ones that you're going to implement. This ensures a safer work while refactoring.

### Contain Bugs

A better code coverage in your test means that you'll save time down the line while debugging by showing you which lines are actually churning out that bug. If a bug comes out, write a test to fix that bug. That way, you can also reproduce the said bug which helps in defining the bug.

### Living Documentation

Ever read your own code and realize you don't know how it turned out that way? TDD ensures that you have a readable documentation. If you're unsure of how a class or library works, go and have a read through the tests. With TDD, tests usually get written for different scenarios, one of which is probably how you want to use the class. So you can see the expected inputs a method requires and what you can expect as outcome, all based on the assertions made in the test.
