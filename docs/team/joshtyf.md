---
layout: page
title: Joshua's Project Portfolio Page
---

## Joshua Tan - Project Portfolio for QuickCache(tP)

### 1. Introduction

This document serves as a project portfolio for QuickCache, and outlines my contributions to the project, including the features that I have implemented.

#### 1.1. About the team

My team of five consists of  five Year 2 Computer Science students, all taking the module CS2103T Software Engineering.

#### 1.2. About the project

This project was developed as part of the module CS2103T Software Engineering. We were tasked to develop a desktop application (Windows/macOS/Linux) with a Command Line Interface (that is, the program operates via text input from the user, called commands). Additionally, we were required to use an existing application, called AddressBook Level 3, as the starting point for building our application.

My team decided to a create an application that helps students manage their flashcards, test themselves, and view their performance over time. To do so, we incorporated the existing people management features of AddressBook and used it as a starting point to build QuickCache.

In total, QuickCache took a total of 10 weeks to complete.

### 2. Summary of Contributions

My primary role within the team was to manage the documentation and my responsibilities included: the formatting and structure of both
the UG and DG; checking for the correctness and accuracy of UML diagrams; ensuring proper use of language to communicate our ideas
clearly to the readers.

In addition, I also helped to develop the application by implementing enhancements.

Given below are my detailed contributions to the project.

#### 2.1 Creation of base classes

I kickstarted the project by creating the `Flashcard` class and other dependencies such as the `Answer`, `Question` and `Tag` classes.
The design architecture was inspired by the original `AddressBook3` design.

#### 2.2 Creation of `Choice` class

  * What it does: Encapsulates a choice for a multiple choice question within a `Choice` class.
  * Justification: Following from 2.1, our team decided that it would be a better option to create choices and to encapsulate it within its
own class instead. This would allow us to be better able to implement our `MultipleChoiceQuestion` class.

#### 2.3 Delete by Tag [enhancement]

  * What it does: Allows the user to delete all flashcards that contain at least one of the specified tags.
  * Justification: This feature enhances the existing delete by index mechanism as the user can now mass delete flashcards.
  * Highlights: This enhancement was slightly challenging to implement as I had to consider whether or not to create a seperate command
  or to merge together with the original delete by index command. After much deliberation, I decided to merge both functionalities together into
  the delete command. As a result, I had to tweak the `DeleteCommand` and `DeleteCommandParser` classes accordingly to ensure proper implementation of
  both features. I also had to design extensive tests to make sure that there wouldn't be any problems from this added enhancement.

#### 2.4 Find command [enhancement]

  * What it does: Allows the user to search for flashcards based on their tags
  * Justification: The user would be using our tagging system to label flashcards according to their modules. Hence
  a search and filter system is needed to find these flashcards.
  * Highlights: Implementing this enhancement was pretty straight forward and the test cases were also pretty easy to consider.

#### 2.5 Improved the GUI

  * What it does: Improves the GUI
  * Justification: The original GUI was plain and we wanted to design something that was a little different from everyone else.
  * Highlights: I worked on making changes to the overall layout of the GUI. In addition, I worked with my teammate Xing Jian
  to pick the colour scheme.

### Code contributions

Click on the following links to view the code that I have contributed:

  * [RepoSense link](https://nus-cs2103-ay2021s1.github.io/tp-dashboard/#breakdown=true&search=joshtyf)
  * [Functional Code](https://github.com/AY2021S1-CS2103T-T13-2/tp/tree/master/src/main/java/quickcache)
  * [Test Code](https://github.com/AY2021S1-CS2103T-T13-2/tp/tree/master/src/test/java/quickcache)

### Other contributions

The following describes the various other contributions that I have made to the project.

* **Project management**:
  * Managed the overall format and structure of the various documentations excluding javadocs.
  * Managed 23 issues on Github.
  * Helped to organise zoom meetings which included reminders and creation of the zoom calls.

* **Enhancements to existing features**:
  * Updated the GUI layout (Pull request [\#214](https://github.com/AY2021S1-CS2103T-T13-2/tp/pull/214))
  * Wrote additional tests for existing features to increase test coverage
  (Pull requests [\#235](https://github.com/AY2021S1-CS2103T-T13-2/tp/pull/235),
   [\#234](https://github.com/AY2021S1-CS2103T-T13-2/tp/pull/234),
   [\#60](https://github.com/AY2021S1-CS2103T-T13-2/tp/pull/60))
  * Refactored code to adhere to DRY principle (Example Pull request: [\#167](https://github.com/AY2021S1-CS2103T-T13-2/tp/pull/167))

* **Documentation**:

  * I was in charge of reviewing every pull request that was related to the documentations
  (Examples: Pull requests [\#160](https://github.com/AY2021S1-CS2103T-T13-2/tp/pull/160),
   [\#229](https://github.com/AY2021S1-CS2103T-T13-2/tp/pull/229),
   [\#159](https://github.com/AY2021S1-CS2103T-T13-2/tp/pull/159) and many more...)
  * User Guide:
    * Added documentation for the features `delete` (Pull request [\#151](https://github.com/AY2021S1-CS2103T-T13-2/tp/pull/151))
    * Planned out the entire format of both the UG
  * Developer Guide:
    * Wrote out the implementation for the `delete` feature (Pull request [\#131](https://github.com/AY2021S1-CS2103T-T13-2/tp/pull/131))
    * Added and edited UML diagrams (Example Pull requests: [\#278](https://github.com/AY2021S1-CS2103T-T13-2/tp/pull/278), [\#287](https://github.com/AY2021S1-CS2103T-T13-2/tp/pull/287)
    * Helped to reformat and rewrite the entire features section of the DG (Pull request [\#245](https://github.com/AY2021S1-CS2103T-T13-2/tp/pull/245))
    * Ensured the accuracy and correctness of all UML diagrams

* **Community**:
  * PRs reviewed (with non-trivial review comments): [\#69](https://github.com/AY2021S1-CS2103T-T13-2/tp/pull/69), [\#37](https://github.com/AY2021S1-CS2103T-T13-2/tp/pull/37))
  * Reported bugs and suggestions for other teams in the class (examples: [1](https://github.com/AY2021S1-CS2103T-T11-2/tp/issues/188), [2](https://github.com/AY2021S1-CS2103T-T11-2/tp/issues/187), [3](https://github.com/AY2021S1-CS2103T-T11-2/tp/issues/186))

* **Tools**:
  * Learned how to use plantuml and taught my teammates accordingly
