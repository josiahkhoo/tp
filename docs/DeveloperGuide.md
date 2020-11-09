---
layout: page
title: Developer Guide
---
* Table of Contents
{:toc}

--------------------------------------------------------------------------------------------------------------------

## **Setting up, getting started**

Refer to the guide [_Setting up and getting started_](SettingUp.md).

--------------------------------------------------------------------------------------------------------------------

## **Design**

### Architecture

<img src="images/ArchitectureDiagram.png" width="450" />

The ***Architecture Diagram*** given above explains the high-level design of the App. Given below is a quick overview of each component.

**`Main`** has two classes called [`Main`](https://github.com/AY2021S1-CS2103T-T13-2/tp/blob/master/src/main/java/quickcache/Main.java) and [`MainApp`](https://github.com/AY2021S1-CS2103T-T13-2/tp/blob/master/src/main/java/quickcache/MainApp.java). It is responsible for,
* At app launch: Initializes the components in the correct sequence, and connects them up with each other.
* At shut down: Shuts down the components and invokes cleanup methods where necessary.

[**`Commons`**](#common-classes) represents a collection of classes used by multiple other components.

The rest of the App consists of four components.

* [**`UI`**](#ui-component): The UI of the App.
* [**`Logic`**](#logic-component): The command executor.
* [**`Model`**](#model-component): Holds the data of the App in memory.
* [**`Storage`**](#storage-component): Reads data from, and writes data to, the hard disk.

Each of the four components,

* defines its *API* in an `interface` with the same name as the Component.
* exposes its functionality using a concrete `{Component Name} Manager` class (which implements the corresponding API `interface` mentioned in the previous point.)

For example, the `Logic` component (see the class diagram given below) defines its API in the `Logic.java` interface and exposes its functionality using the `LogicManager.java` class which implements the `Logic` interface.

![Class Diagram of the Logic Component](images/LogicClassDiagram.png)

**How the architecture components interact with each other**

The *Sequence Diagram* below shows how the components interact with each other for the scenario where the user issues the command `delete 1`.

<img src="images/ArchitectureSequenceDiagram.png" width="574" />

The sections below give more details of each component.

### UI component

![Structure of the UI Component](images/UiClassDiagram.png)

**API** :
[`Ui.java`](https://github.com/AY2021S1-CS2103T-T13-2/tp/blob/master/src/main/java/quickcache/ui/Ui.java)

The UI consists of a `MainWindow` that is made up of parts e.g.`CommandBox`, `ResultDisplay`, `FlashcardListPanel`, `StatusBarFooter` etc. All these, including the `MainWindow`, inherit from the abstract `UiPart` class.

The `UI` component uses [JavaFx](https://openjfx.io) UI framework. The layout of these UI parts are defined in matching `.fxml` files that are in the `src/main/resources/view` folder. For example, the layout of the [`MainWindow`](https://github.com/AY2021S1-CS2103T-T13-2/tp/blob/master/src/main/java/quickcache/ui/MainWindow.java) is specified in [`MainWindow.fxml`](https://github.com/AY2021S1-CS2103T-T13-2/tp/blob/master/src/main/resources/view/MainWindow.fxml)

The `UI` component,

* Executes user commands using the `Logic` component.
* Listens for changes to `Model` data so that the UI can be updated with the modified data.

### Logic component

![Structure of the Logic Component](images/LogicClassDiagram.png)

**API** :
[`Logic.java`](https://github.com/AY2021S1-CS2103T-T13-2/tp/blob/master/src/main/java/quickcache/logic/Logic.java)

1. `Logic` uses the `QuickCacheParser` class to parse the user command.
1. This results in a `Command` object which is executed by the `LogicManager`.
1. The command execution can affect the `Model` (e.g. adding a new flashcard).
1. The result of the command execution is encapsulated as a `CommandResult` object which is passed back to the `Ui`.
1. In addition, the `CommandResult` object can also instruct the `Ui` to perform certain actions, such as displaying help to the user.

Given below is the Sequence Diagram for interactions within the `Logic` component for the `execute("delete 1")` API call.

![Interactions Inside the Logic Component for the `delete 1` Command](images/DeleteSequenceDiagram.png)

<div markdown="span" class="alert alert-info">:information_source: **Note:** The lifeline for `DeleteCommandParser` should end at the destroy marker (X) but due to a limitation of PlantUML, the lifeline reaches the end of diagram.
</div>

<div style="page-break-after: always;"></div>
### Model component

![Structure of the Model Component](images/ModelClassDiagram.png)

**API** : [`Model.java`](https://github.com/AY2021S1-CS2103T-T13-2/tp/blob/master/src/main/java/quickcache/model/Model.java)

The `Model`,

* stores a `UserPref` object that represents the user’s preferences.
* stores the quick cache flashcard data.
* exposes an unmodifiable `ObservableList<Flashcard>` that can be 'observed' e.g. the UI can be bound to this list so that the UI automatically updates when the data in the list change.
* does not depend on any of the other three components.

<div style="page-break-after: always;"></div>
### Storage component

![Structure of the Storage Component](images/StorageClassDiagram.png)

**API** : [`Storage.java`](https://github.com/AY2021S1-CS2103T-T13-2/tp/blob/master/src/main/java/quickcache/storage/Storage.java)

The `Storage` component,
* can save `UserPref` objects in json format and read it back.
* can save the QuickCache data in json format and read it back.

### Common classes

Classes used by multiple components are in the `quickcache.commons` package.

--------------------------------------------------------------------------------------------------------------------

## **Implementation**

This section describes some noteworthy details on how certain features are implemented.

The features mentioned are:

- [Adding flashcards with open ended questions](#add-flashcard-with-open-ended-question)
- [Adding flashcards with multiple choice questions](#add-flashcard-with-multiple-choice-question)
- [Opening of flashcards](#open-flashcard)
- [Tagging system](#tags)
- [Editing a flashcard](#edit-flashcard)
- [Deleting a flashcard by index](#delete-by-index)
- [Deleting by Tags](#delete-by-tag)
- [Finding flashcards](#find-flashcards)
- [Setting difficulty for flashcards](#difficulty)
- [Displaying statistics of a flashcard](#display-statistics-of-a-flashcard)
- [Displaying statistics of flashcards by tag](#display-statistics-of-flahcards-by-tag)
- [Clearing all flashcards](#clear-all-flashcards)
- [Clearing statistics of a flashcard](#clear-statistics-of-a-flashcard)
- [Testing a flashcard](#test-a-flashcard)
- [Exporting flashcards](#export-flashcards)
- [Importing flashcards](#import-flashcards)

### Add flashcard with open-ended question

#### Implementation
The Add Open Ended Question mechanism is facilitated by `QuickCache`.
The flashcard created is stored internally inside a `UniqueFlashcardList` within the `QuickCache` object.

##### Usage

The following activity diagram summarizes what happens when a user executes add open ended question  command on a specified flashcard:

![AddOpenEndedActivityDiagram](images/AddOpenEndedActivityDiagram.png)

Given below is an example usage scenario and how the mechanism behaves at each step.

Step 1. The user launches the application for the first time. `QuickCache` will be initialized with the initial state.

Step 2. The user executes `add q/question...` command to add a flashcard.
The following sequence diagram shows how the input command gets parsed:

![AddOpenEndedSequenceDiagram](images/AddOpenEndedParserSequenceDiagram.png)

Step 3. This will result in the creation of a flashcard with an open-ended question inside `QuickCache`.

The following sequence diagram shows how the flashcard is added:

![AddOpenEndedSequenceDiagram](images/AddOpenEndedSequenceDiagram.png)

<div markdown="span" class="alert alert-info">:information_source: **Note:** If a command fails its execution, QuickCache will not create the flashcard.
</div>

#### Design considerations

* **Current implementation:** Flashcard is saved upon creation inside the QuickCache.
  * Pros: Easy to implement and CLI-optimized.
  * Cons: May be complicated in the future as there will be too many prefixes along with the `add` command.

### Add flashcard with multiple choice question

#### Implementation

The Add Multiple Choice Question mechanism is facilitated by `QuickCache`.
The flashcard created is stored internally inside a `UniqueFlashcardList` within the `QuickCache` object.

##### Usage

The following activity diagram summarizes what happens when a user executes add multiple choice question  command on a specified flashcard:

![AddMcqActivityDiagram](images/AddMcqActivityDiagram.png)

Given below is an example usage scenario and how the Add Multiple Choice Question mechanism behaves at each step.

Step 1. The user launches the application for the first time. `QuickCache` will be initialized with the initial state.

Step 2. The user executes `addmcq q/question ans/1 c/first c/second` command to add a flashcard.
The following sequence diagram shows how the input is parsed:

![AddMcqSequenceDiagram](images/AddMcqParserSequenceDiagram.png)

Step 3. This will result in the creation of a flashcard with an multiple choice question inside `QuickCache`.

The following sequence diagram shows how flashcard is added:

![AddMcqSequenceDiagram](images/AddMcqSequenceDiagram.png)

<div markdown="span" class="alert alert-info">:information_source: **Note:** If a command fails its execution, QuickCache will not create the flashcard.
</div>

#### Design considerations:

* **Current implementation:** Flashcard is saved upon creation inside the QuickCache.
  * Pros: Easy to implement and CLI-optimized.
  * Cons: May be complicated in the future as there will be too many prefixes along with the `addmcq` command.

### Open flashcard

#### Implementation

The Open flashcard mechanism will allow the user to open a flashcard specified by the given index and display it in the GUI.
The `OpenCommandParser#parse` takes in a single `String` argument called `index`. It then parses the argument and creates an `Index` object to be passed on to
the `OpenCommand` class instance. If no argument is given, then a `CommandException` will be thrown.

During execution, the `OpenCommand` class will pass the `Question` to the GUI for it to display the`Question` of the `Flashcard` to the user.
This is be done by passing the `Question` into a `Feedback` object which is an attribute of the `CommandResult` given to the GUI.

The GUI will change the content of some of its placeholders to display the question and if available, its choices to the user.
The GUI will change the contents of its placeholders accordingly if other commands aside from another `OpenCommand` is called afterwards.

##### Usage

The following activity diagram summarizes what happens when a user executes open command on a specified flashcard:

![OpenFlashcardActivityDiagram](images/OpenFlashcardActivityDiagram.png)

Given below is an example usage scenario and how the Open mechanism behaves at each step.

Step 1. The user launches the application for the first time. `QuickCache` will be initialized with the initial state.

Step 2. The user executes `open 1` command to display the first flashcard in the list on the GUI.

Step 3. This will call `OpenCommandParser#parse` which will then parse the arguments provided. Within the method, `ParserUtil#parseIndex` will be called to convert the user input to index of the first `Flashcard`.

Step 4. The `Index` is then passed to the `OpenCommand` during its construction.
The following sequence diagram briefly shows how the parser operation works:

![OpenParserSequenceDiagram](images/OpenParserSequenceDiagram.png)

Step 5. `OpenCommand#execute` will get the `Flashcard` at the specified `Index` and get its `Question` to be passed to the GUI as part of the `Feedback` attribute within the `CommandResult`.

Step 6. The GUI will then proceed to get the `Question` from `Feedback` and display its choices and question to the user.

The following sequence diagram shows how the Open mechanism works:

![OpenSequenceDiagram](images/OpenSequenceDiagram.png)

#### Design considerations:

* **Current implementation:** Flashcard is taken from the last displayed list
  * Pros: Easy to implement and CLI-optimized.
  * Cons: *nil*

### Tags

#### Implementation

A `Tag` is stored internally in a `Set<Tag>` within a `Flashcard` object.
It is is created and added to the flashcard during the parsing stage of an `add` or `addmcq` command.
Multiple tags can be created for each `Flashcard` as well.

##### Usage

Given below is an example usage scenario and how the Tag mechanism behaves at each step.

Step 1. The user launches the application for the first time. `QuickCache` will be initialized with the initial state.

Step 2. The user executes `add q/question... t/tag` command to add a flashcard with a tag inside QuickCache.

Step 3. During the execution of `AddOpenEndedQuestionCommandParser#parse` method, `ParserUtil#parseTags` will be called
to parse and create `Tag` objects for each unique tag. This will then be stored in a `Set<Tag>` called `tagList`

Step 4. The `Flashcard` will then be constructed containing the `tagList`.

#### Design considerations:

* **Current implementation:** Tag is saved within a `Set<Tag>` within `Flashcard`
  * Pros: Easy to implement and doesn't allow for duplicates.
  * Cons: Might take very long to search for flashcards with a specified tag especially if there are many flashcards.

* **Alternative:** Utilise an additional data structure for each individual tag to store flashcards.
  * Pros: Easy and fast to find flashcards with a specified tag.
  * Cons: Many duplicate copies of flashcards will be created.

### Edit flashcard

#### Implementation

The following activity diagram summarizes what happens when a user executes an edit flashcard command on a specified flashcard:

![EditActivityDiagram](images/EditActivityDiagram.png)

The Edit flashcard mechanism operates by editing the flashcard at a specified index of the last displayed list.
The new information is encapsulated inside a `EditFlashcardDescriptor` and is passed together with the `Index` object
to the `EditCommand`.

During `EditComamnd#execute`, a new `Flashcard` object will be created. For each of its individual content (i.e `Answer`),
if the `EditFlashcardDescriptor` does not have the new information, the old content will be from the original `Flashcard`.

##### Usage

Given below is an example usage scenario and how the Edit mechanism behaves at each step.

Step 1. The user executes `edit 1 ans/answer` command to edit the answer field of the first flashcard.

Step 2. `EditCommandParser#parse` will then parse the arguments provided. In this example, a new `Answer` object will be
created after parsing.

Step 3. The `Answer` object will then be passed to the `EditFlashcardDescriptor` object. The `EditFlashcardDescriptor`
object together with the original `Flashcard` will be passed to the `EditCommand` object. The following sequence diagram
briefly shows how the parser operation works (`Answer` and `EditFlashcardDescriptor` objects not included):

![EditSequenceDiagram](images/EditParserSequenceDiagram.png)

Step 4. The `EditCommand` will then create a new `Flashcard` using information from `EditFlashcardDescriptor`.
In the example, only a new answer is present. All other information will be taken from the original `Flashcard`.

Step 5. `EditCommand#execute` will then replace the old `Flashcard` in the `model` with the new `Flashcard`.

The following sequence diagram shows how the Edit mechanism works:

![EditSequenceDiagram](images/EditSequenceDiagram.png)

<div markdown="span" class="alert alert-info">:information_source: **Note:** If a command fails its execution, it will not be saved in the QuickCache, so the flashcard inside the QuickCache will not be updated.
</div>

#### Design considerations:

* **Current choice:** Create a new flashcard with all the new information to replace the old flashcard.
  * Pros: Maintains immutability.
  * Cons: Difficult to implement as a seperate `EditFlashcardDescriptor` class must be implemented as well.

* **Alternative:** Edit the old flashcard directly.
  * Pros: Much easier to implement.
  * Cons: If the execution is stopped halfway, then the newly updated flashcard will contain wrong information. It will
  be difficult to debug as well.

### Delete by Index

#### Implementation

The Delete By Index mechanism deletes the flashcard at the specified index of the last displayed list.

##### Usage

The following activity diagram briefly summarizes what happens when a user executes the delete command to delete
flashcards by tags:

![DeleteByIndexActivityDiagram](images/DeleteByIndexActivityDiagram.png)

Given below is an example usage scenario and how the Delete By Index mechanism behaves at each step.

Step 1. The user launches the application.

Step 2. The user executes `delete 1` command to delete the first flashcard. This command will be parsed by `DeleteCommandParser`.

Step 3. `DeleteCommandParser#parse` will then create a `DeleteCommand` which has its `isDeleteByTag` field set to `false`.
`DeleteCommandParser#parse` will also create an `Index` object to be passed to the `DeleteCommand` object.

Step 4. `DeleteCommand#execute` will then get a copy of the to-be `Flashcard` from the `model` using the `Index` provided.

Step 5. The `model#deleteFlashcard` will then delete the `Flashcard` by finding for it based on its copy.

<div markdown="span" class="alert alert-info">:information_source: **Note:** If a command fails its execution, it will not be saved in the QuickCache, so the flashcard inside the QuickCache will not be updated.
</div>

The following sequence diagram shows how the Delete By Index mechanism works:

![DeleteSequenceDiagram](images/DeleteSequenceDiagram.png)

#### Design considerations:

* **Current choice:** Delete command works for both index and tags
  * Pros: Convenient for the user as there is only one command.
  * Cons: Extra checks have to be implemented to make sure that the correct deletion mechanism is executed.

* **Alternative:** Create a seperate delete command for deleting by tags
  * Pros: Less overlapping and easier to debug.
  * Cons: Extra work needed to implement the delete command.

### Delete by Tag

#### Implementation

The Delete By Tag mechanism will delete flashcards specified by a given set of tags. Any flashcard containing
all of the specified tags will be deleted.

It works by filtering for the flashcards in the `model` and deleting them one by one.

##### Usage

The following activity diagram briefly summarizes what happens when a user executes the delete command to delete
flashcards by tags:

![DeleteByTagActivityDiagram](images/DeleteByTagActivityDiagram.png)

Given below is an example usage scenario and how the Delete By Tag mechanism behaves at each step.

 Step 1. The user launches the application.

 Step 2. The user executes `delete t/MCQ` command to delete all flashcards with the tag `MCQ`.

 Step 3. This will call `DeleteCommandParser#parse` which will then parse the arguments provided.
 Within `DeleteCommandParser#parse`, `ParserUtil#parseTags` will be called to create a `FlashcardPredicate` using the tags.

 Step 4. A new `DeleteCommand` object will be created with its `isDeleteByTag` field set to `true`. The `FlashcardPredicate`
 will also be passed to the `DeleteCommand` object.
 The following sequence diagram briefly shows how the parser operation works (`FlashcardPredicate` not shown):

 ![DeleteByTagParserSequenceDiagram](images/DeleteByTagParserSequenceDiagram.png)

 Step 5. `DeleteCommand#execute` will filter the `QuickCache` model with the provided predicate and get back the filtered list

 Step 6. It will then iterate through the list and call `QuickCache#deleteFlashcard` on each `Flashcard` in the list.

The following sequence diagram shows how the Delete By Tag mechanism works:

![DeleteByTagSequenceDiagram](images/DeleteByTagSequenceDiagram.png)

#### Design considerations:

* **Current choice:** Delete command works for both index and tags
  * Pros: Convenient for the user as there is only one command.
  * Cons: Extra checks have to be implemented to make sure that the correct deletion mechanism is executed.

* **Alternative:** Create a seperate delete command for deleting by tags
  * Pros: Less overlapping and easier to debug.
  * Cons: Extra work needed to implement the delete command.

### Find flashcards

#### Implementation

The following activity diagram summarizes what happens when a user executes a find flashcard command with both a keyword and tag query:

![FindActivityDiagram](images/FindActivityDiagram.png)

The Find mechanism searches for flashcards based on the specified tag or question keyword or both.
Each filter will result in the creation of a `Predicate<Flashcard>`.
A class called `FlashcardPredicate` will be introduced that collects all `Predicate<Flashcard>` into one class.

##### Usage

Given below is an example usage scenario and how the Find mechanism behaves at each step.

Step 1. The user launches the application.

Step 2. The user executes `find t/Assembly q/What` command to find a `Flashcard` with the tag `Assembly` and the keyword `What` in its `Question`.

Step 3. This will call `FindCommandParser#parse` which will then parse the arguments provided.
Within the method, `ParserUtil#parseTags`and `Parserutil#parseKeywords` will be called to create tags and keywords for the arguments.

Step 4. A new `FlashcardPredicate` will then be created from the `QuestionContainsKeywordsPredicate` and `FlashcardContainsTagPredicate` generated from the given tags and keywords.
It will be used to filter for all the flashcards that have the specified tags. This `FlashcardPredicate` is then passed to the `FindCommand`
The following sequence diagram shows how the parser operation works:

![FindParserSequenceDiagram](images/FindParserSequenceDiagram.png)

 Step 5. `FindCommand#execute` will set the `QuickCache` model's filter with the provided predicate.

 Step 6. The GUI will then proceed to get the filtered list based on the newly set predicate to display to the user.

 Step 7. After execution, `CommandResult` will contain a message indicating that it has listed all `Flashcards` based on the specified restrictions.

 The following sequence diagram shows how the Find mechanism works:

![FindSequenceDiagram](images/FindSequenceDiagram.png)

#### Design considerations

* **Current choice:** Use a `FlashcardPredicate` to filter for the `Flashcard`s.
  * Pros: `FlashcardPredicate` can be extended to be used for other operations.
  * Cons: Multiple predicates will have be created if more search fields are used.

### Difficulty

#### Implementation

The Difficulty mechanism is facilitated by storing a `Difficulty` object inside a `Flashcard` object.

The `Difficulty` class takes difficulty levels from `Difficulties` enums which contains four difficulty levels `LOW`, `MEDIUM`, `HIGH` and `UNSPECIFIED`.

##### Usage

Given below is an example usage scenario and how the Difficulty mechanism behaves at each step.

Step 1. The user launches the application.

Step 2. The user executes `add q/question... d/difficultyLevel` command to add a flashcard with difficulty.
If the user executes `add q/question...` command without `d/` prefix.
The `add` command will cause the addition of a flashcard with a difficulty set to `UNSPECIFIED` inside the QuickCache.

Step 3. The user executes `edit 1 d/difficultyLevel` to edit the difficulty in the first flashcard of the list.
The edit command will change the internal structure of flashcard such that the `Difficulty` is updated.

#### Design considerations:

* **Current choice:** Difficulty is saved upon creation.
  * Pros: Easy to implement.
  * Cons: May be complicated as there will be too many fields in the `add` command.

* **Alternative:** Individual command knows how to add difficulty by
  itself.
  * Pros: Will be less complicated.
  * Cons: There may be too many commands which can be combined to one.


### Test a flashcard

#### Implementation

The Test mechanism is facilitated by `Flashcard`. Specifically, `Statistics` stored within the flashcard. `Flashcard` implements the following methods.
* `Flashcard#getFlashcardAfterTestSuccess()` — Returns a new `Flashcard` object with `Statistics:timesTested` and `Statistics:timesTestedCorrect` incremented by one.
* `Flashcard#getFlashcardAfterTestFailure()` — Returns a new `Flashcard` object with `Statistics:timesTested` incremented by one.

##### Usage

Given below is an example usage scenario and how the Test mechanism behaves at each step.

Step 1. The user launches the application. The `Flashcard` to be tested will be initialized with the initial flashcard state.

![TestState0](images/TestState0.png)

Step 2. The user tests the flashcard with a specified `answer` or `option`.

Step 2a. The user executes `test 1 ans/correct answer` command to test the `Flashcard`. The `test` command calls `Flashcard#getFlashcardAfterTestSuccess()`, and replaces the previously tested flashcard with the newly updated flashcard, changing its state.

![TestState1](images/TestState1.png)

Step 2b. The user executes `test 1 ans/wrong answer` command to test the `Flashcard`. The `test` command calls `Flashcard#getFlashcardAfterTestFailure()`, and replaces the previously tested flashcard with the newly updated flashcard, changing its state.

![TestState2](images/TestState2.png)

The following sequence diagram shows how the Test mechanism works:

![TestSequenceDiagram](images/TestSequenceDiagram.png)

The following sequence diagram shows how the input get parsed:

![TestParserSequenceDiagram](images/TestParserSequenceDiagram.png)

The following activity diagram summarizes what happens when a user executes a test command on a specified flashcard:

![TestActivityDiagram](images/TestActivityDiagram.png)

#### Design considerations:

* **Current choice:** Increments a counter of `timesTestedCorrect` and `timesTested` in `Statistics`.
  * Pros: Easy to implement.
  * Cons: Unable to keep track of useful information such as performance over time.

* **Alternative:** `Statistics` is made up of an `Array` of `test`, including information such as `timestamp`
  * Pros: Retrieval of useful statistics will be possible.
  * Cons: Save file will expand very quickly because each `test` record needs to be logged.

### Display statistics of a flashcard

#### Implementation

The Display mechanism will allow the user to view a Pie Chart of the statistics of the Flashcard specified by the given index and display it in the GUI.

The implementation requires the creation of a `StatsCommandParser` and a `StatsCommand`. The `StatsCommandParser#parse` will take in a single argument for `Index`. After parsing the argument, it will then proceed to create a `StatsCommand` class instance. If no `Index` is given then a `CommandException` will be thrown.

The `StatsCommand` class will have to pass the `Statistics` to the GUI for it to display the `Statistics` of the `Flashcard` to the user. This will be done by passing the `Statistics` into a `Feedback` object which is an attribute of the `CommandResult` given to the GUI.

The GUI will change the content of some of its placeholders to display the `Statistics` as a Pie Chart. The GUI will change the contents of its placeholders accordingly if other commands aside from another `StatsCommand` is called afterwards.

##### Usage

The following activity diagram summarizes what happens when a user executes stats command on a specified flashcard:

![StatsActivityDiagram](images/StatsActivityDiagram.png)

Given below is an example usage scenario and how the Displaystats mechanism behaves at each step.

Step 1. The user launches the application after a few times of using the `TestCommand` feature. The `QuickCache` will be initialized with the existing QuickCache state.

Step 2. The user executes `stats 1` command to display the `Statistics` of the first flashcard in the list on the GUI.

Step 3. This will call `StatsCommandParser#parse` which will then parse the arguments provided. Within the method, `ParserUtil#parseIndex` will be called to convert the user input into the `Index` of the first `Flashcard`.

Step 4. The `index` is then passed to the `StatsCommand`.
The following sequence diagram shows how the parser operation works:

![StatsParserSequenceDiagram](images/StatsParserSequenceDiagram.png)

Step 5. `StatsCommand#execute` will get the `Flashcard` at the specified `Index` and get its `Statistics` to be passed to the GUI as part of the `Feedback` attribute within the `CommandResult`.

Step 6. The GUI will then proceed to get the `Statistics` from `Feedback` and display its data in the form of a Pie Chart to the user.

The following sequence diagram shows how the Displaystats mechanism works:

![StatsSequenceDiagram](images/StatsSequenceDiagram.png)

#### Design Considerations:

* **Current choice:** Passes the `Statistics` object to the GUI in `Feedback` which is an attribute of `CommandResult`.
  * Pros: Provides more abstraction as all of the data the GUI needs to display are in the `Feedback` object.
  * Cons: There is a violation of Demeter's law as GUI interacts with an attribute of `CommandResult`.

* **Alternative:** Do not use the `Feedback` object. Place all the data in the `CommandResult` object directly.
  * Pros: Demeter's law is no longer violated.
  * Cons: There is less abstraction.
  
### Display statistics of flashcards by tag

#### Implementation

The Display stats by tag mechanism will allow the user to view a Pie Chart of the aggregated statistics of flashcards specified by a given set of tags. Any flashcard that contains all the specified tags will have their statistics aggregated.

It works by filtering for the flashcards in the `model` and aggregating their statistics using `StatsCommand:getAggregatedStatistics`.

##### Usage

The following activity diagram summarizes what happens when a user executes stats command with a set of specified tags:

![StatsByTagActivityDiagram](images/StatsByTagActivityDiagram.png)

Given below is an example usage scenario and how the Displaystats mechanism behaves at each step.

Step 1. The user launches the application after a few times of using the `TestCommand` feature. The `QuickCache` will be initialized with the existing QuickCache state.

Step 2. The user executes `stats t/MCQ` command to display the aggregated `Statistics` of all flashcards with the tag `MCQ`.

Step 3. This will call `StatsCommandParser#parse` which will then parse the arguments provided. Within `StatsCommandParser#parse`, `ParserUtil#parseTags` will be called to create a `FlashcardPredicate` using the tags.

Step 4. The `FlashcardPredicate` is then passed to the `StatsCommand`.
The following sequence diagram shows how the parser operation works:

![StatsByTagParserSequenceDiagram](images/StatsByTagParserSequenceDiagram.png)

Step 5. `StatsCommand#execute` will filter the `QuickCache` model with the provided predicate and get back the filtered list.

Step 6. The filtered list will have their statistics aggregated through the `StatsCommand:getAggregatedStatistics`.

Step 7. The aggregated `Statistics` will be passed to the GUI as part of the `Feedback` attribute within the `CommandResult`.

Step 8. The GUI will then proceed to get the `Statistics` from `Feedback` and display its data in the form of a Pie Chart to the user.

The following sequence diagram shows how the Display stats by tag mechanism works:

![StatsByTagSequenceDiagram](images/StatsByTagSequenceDiagram.png)

#### Design Considerations:

* **Current choice:** Passes the `Statistics` object to the GUI in `Feedback` which is an attribute of `CommandResult`.
  * Pros: Provides more abstraction as all of the data the GUI needs to display are in the `Feedback` object.
  * Cons: There is a violation of Demeter's law as GUI interacts with an attribute of `CommandResult`.

* **Alternative:** Do not use the `Feedback` object. Place all the data in the `CommandResult` object directly.
  * Pros: Demeter's law is no longer violated.
  * Cons: There is less abstraction.
  
### Clear all flashcards

#### Implementation

The following activity diagram summarizes what happens when a user executes the clear command in QuickCache.

![ClearActivityDiagram](images/ClearActivityDiagram.png)

The Clear mechanism will allow the user to delete all flashcards in his local QuickCache.

The implementation makes use of the `Model#setQuickCache` and `QuickCache`. A new instance of `QuickCache` without any existing flashcards will replace the users current `QuickCache` in `Model`.

##### Usage

Given below is an example usage scenario and how the Clear mechanism behaves at each step.

Step 1. The user executes `clear` command to clear all flashcards in his local QuickCache.

Step 2. `ClearCommand#execute` will replace the current instance of `QuickCache` with a new empty instance of `QuickCache` through the `Model#setQuickCache` method.

Step 3. After execution, `CommandResult` will contain a message indicating that it has cleared QuickCache.

The following sequence diagram shows how the Clear mechanism works:

![ClearSequenceDiagram](images/ClearSequenceDiagram.png)

#### Design Considerations:

* **Current choice:** Replaces the existing `QuickCache` in model with a new `QuickCache` that is empty.
  * Pros: Easy to implement - minimizes the occurence of bugs.
  * Cons: Waste of resources as a new `QuickCache` instance needs to be created when a user wants to clear QuickCache.

### Clear statistics of a flashcard

#### Implementation

The Clearstats mechanism  will allow the user to reset the statistics of the flashcard specified by the given index.

The implementation requires the creation of a `ClearStatsCommandParser` and a `ClearStatsCommand`. The `ClearStatsCommandParser#parse` will take in a single argument for `Index`. After parsing the argument, it will then proceed to create a `ClearStatsCommand` class instance. If no `Index` is given then a `CommandException` will be thrown.

The `ClearStatsCommand` class will replace the `Flashcard` at the specified `Index` with a copy of the original `Flashcard` that has its `Statistics` reset to zero for all fields.

##### Usage

The following activity diagram summarizes what happens when a user executes clearstats command on a specified flashcard:

![ClearStatsActivityDiagram](images/ClearStatsActivityDiagram.png)

Given below is an example usage scenario and how the Clearstats mechanism behaves at each step.

Step 1. The user launches the application after a few times of using the `TestCommand` feature. The `QuickCache` will be initialized with the existing QuickCache state.

Step 2. The user executes `stats 1` command to display the `Statistics` of the first flashcard in the list on the GUI. The user sees that the `Statistics` has values that are not zero.

Step 3. The user executes `clearstats 1` command to clear the `Statistics` of the first flashcard in the list on the GUI.

Step 4. This will call `ClearStatsCommandParser#parse` which will then parse the arguments provided. Within the method, `ParserUtil#parseIndex` will be called to convert the user input into the `Index` of the first `Flashcard`.

Step 5. The `index` is then passed to the `ClearStatsCommand`.
The following sequence diagram shows how the parser operation works:

![ClearStatsSequenceDiagram](images/ClearStatsParserSequenceDiagram.png)

Step 6. `ClearStatsCommand#execute` will get the `Flashcard` at the specified `Index` and call `ClearStatsCommand#getFlashcardAfterClearStatistics` which will give a copy of the original `Flashcard` with its `Statistics` reset to zero for all fields. The original `Flashcard` will then be replaced by the new `Flashcard` copy.

Step 7. After execution, `CommandResult` will contain a message indicating that it has cleared the `Statistics` of the `Flashcard` on the specified index.

Step 7. The user executes `stats 1` command to display the `Statistics` of the first flashcard in the list on the GUI. The user sees that the `Statistics` is reset.

The following sequence diagram shows how the Clearstats mechanism works:

![ClearStatsSequenceDiagram](images/ClearStatsSequenceDiagram.png)

#### Design Considerations:

* **Current choice:** Replaces the existing `Flashcard` with a new `Flashcard` that has a new `Statistics` with all attributes set at zero.
  * Pros: `Flashcard` and `Statistics` are easier to debug.
  * Cons: Waste of resources as new `Flashcard` and `Statistics` objects need to be created when a user wants to clear its statistics.

* **Alternative:** Edit the `Statistics` of the `Flashcard` directly
  * Pros: No "unnecessary" creation of a new `Flashcard` and `Statistics` object when a user requests to clear its statistics.
  * Cons: `Flashcard` and `Statistics` become difficult to debug.

### Export flashcards

#### Implementation

The Export mechanism is facilitated by `Storage` and `QuickCache`. `Storage` is used to interact with the users local data, and a new `QuickCache` containing the data to be exported is passed to `Storage` to save to local data.

##### Usage

Given below is an example usage scenario and how the Export mechanism behaves at each step.

Step 1. The user inputs the `find t/cs2100` command to find all `Flashcard` containing the tag `cs2100`. The `Model` updates its current filtered flashcard list.

Step 2. The user inputs the `export out.json` command. The following sequence diagram shows how the input command gets parsed:

![ExportParserSequenceDiagram](images/ExportParserSequenceDiagram.png)

Step 3. The parsed `Export` command is executed. The current filtered flashcard list is exported to `out.json`, located in the `/export/` directory.

The following sequence diagram shows how the Export mechanism works as a whole:

![ExportSequenceDiagram](images/ExportSequenceDiagram.png)

The following activity diagram summarizes what happens when a user executes an `Export` command:

![ExportActivityDiagram](images/ExportActivityDiagram.png)

#### Design considerations:

* **Current choice:** Predefined directory of `/export/`
  * Pros: Easy to implement.
  * Cons: The user will have to navigate to his `/export/` folder to retrieve output file.

* **Alternative:** User specifies which directory to save the export file to.
  * Pros: More control over where the export file will end up at.
  * Cons: Difficult to implement.
  * Cons: Command becomes more complicated as the entire path needs to be typed out.

### Import flashcards

#### Implementation

The Import mechanism is similarly facilitated by `Storage` and `QuickCache`. `Storage` is used to interact with the users local data, and a new `QuickCache` containing the data to be imported is read by `Storage` from local data.

##### Usage

Given below is an example usage scenario and how the Import mechanism behaves at each step.

Step 1. The user places the file `in.json` that he wants to import in his `/import/`folder.

Step 2. The user inputs the `import in.json` command. The following sequence diagram shows how the input command gets parsed:

![ImportParserSequenceDiagram](images/ImportParserSequenceDiagram.png)

Step 3. The parsed `Import` command is executed. The flashcards from the file `in.json` is imported into his local `QuickCache`. If a flashcard has been imported before, it will not be imported again. The check for repitive flashcards is carried out using `Model#hasFlashcard` and `Flashcard#equals`.

The following sequence diagram shows how the Import mechanism works as a whole:

![ImportSequenceDiagram](images/ImportSequenceDiagram.png)

The following activity diagram summarizes what happens when a user executes an `Import` command:

![ImportActivityDiagram](images/ImportActivityDiagram.png)

#### Design considerations:

* **Current choice:** Predefined directory of `/import/`
  * Pros: Easy to implement.
  * Cons: The user will have to navigate to his `/import/` folder to a place the input file in it.

* **Alternative:** User specifies which directory to save the export file to.
  * Pros: More control over where the import file can be from e.g. user's download folder.
  * Cons: Difficult to implement.
  * Cons: Command becomes more complicated as the entire path needs to be typed out.


--------------------------------------------------------------------------------------------------------------------

## **Documentation, logging, testing, configuration, dev-ops**

* [Documentation guide](Documentation.md)
* [Testing guide](Testing.md)
* [Logging guide](Logging.md)
* [Configuration guide](Configuration.md)
* [DevOps guide](DevOps.md)

--------------------------------------------------------------------------------------------------------------------

## **Appendix: Requirements**

### Product scope

**Target user profile**:

* Students with many modules who want to memorize points
* Prefer desktop apps over other types
* Prefers typing to mouse interactions
* Is reasonably comfortable using CLI apps
* Wants to monitor his/her progress

**Value proposition**: manage flashcards faster than a typical mouse/GUI driven app with
a test feature and track the progress later.


### User stories

Priorities: High (must have) - `* * *`, Medium (nice to have) - `* *`, Low (unlikely to have) - `*`

| Priority | As a …​                                    | I want to …​                     | So that I can…​                                                |
| -------- | ------------------------------------------ | ------------------------------ | ------------------------------------------------------------------------- |
| `* * *`  | new user                                   | see usage instructions         | refer to instructions when I forget how to use the App                    |
| `* * *`  | new user                                   | have a get started             | see the features available in the app                                     |
| `* * *`  | new user                                   | have a sample data             | explore more about the app                                                |
| `* * *`  | new user                                   | purge all current data         | delete the sample data for exploring the app                              |
| `* * *`  | user                                       | add a new flashcard            |                                                                           |
| `* * *`  | user                                       | delete a flashcard             | remove flashcards that I no longer need                                   |
| `* *`    | user                                       | find a flashcard by name       | locate details of flashcards without having to go through the entire list |
| `* *`    | user                                       | edit a flashcard               |                                                                           |
| `* *`    | user                                       | quiz myself                    | memorize things better                                                    |
| `* *`    | user                                       | organize my flashcard          |                                                                           |
| `* *`    | user                                       | categorize my flashcards       | easily find the flashcard associated with the category                    |
| `* *`    | user                                       | import a set of flashcard      | easily add new category of flashcard                                      |
| `* *`    | user                                       | export a set of flashcard      | easily transfer a category of flashcard                                   |
| `*`      | user who has created many flashcards       | delete a category of flashcard | easily transfer a category of flashcard                                   |
| `*`      | user with many flashcards in the list      | specify the importance         |                                                                           |
| `*`      | user                                       | randomize the question         | test myself better                                                        |
| `*`      | user                                       | get the statistic of quiz      | get a visualisation form of my performance                                |
| `*`      | user                                       | track the progress             | know what I have done when using the app                                  |

### Use cases

For all use cases below, the **System** is the `QuickCache` and the **Actor** is the `user`, unless specified otherwise.

**Use case: UC01 - Explore QuickCache**

**MSS**

1.  User double clicks on QuickCache.jar
2.  QuickCache opens and shows a list of flashcards
3.  User plays around with QuickCache features to familiarize with them
4.  User purges all "get started" data

    Use case ends.

**Extensions**

* 1a. Error message indicating that QuickCache.jar cannot be opened pops up.

  * 1a1. User opens up CLI in the directory containing QuickCache and runs `java -jar QuickCache.jar`.

    Use case resumes at step 2.

* 3a. User wants to know all the available commands present in QuickCache.

  * 3a1. User requests for all the instructions available in QuickCache.

  * 3a2. QuickCache displays all available commands.

    Use case resumes at step 3.


**Use case: UC02 - View statistics of a flashcard**

**Preconditions: User has QuickCache open.**

**MSS**

1.  User requests for statistics of a flashcard
2.  QuickCache displays statistics of the flashcard
3.  User <u>tests himself with the same flashcard (UC05)</u>
4.  User requests for statistics of the flashcard
5.  QuickCache displays updated statistics of the flashcard

    Use case ends.

**Extensions**

* 1a. The given index is invalid.

  * 1a1. QuickCache shows an error message.

    Use case resumes at step 1.


**Use case: UC03 - Delete a flashcard**

**Preconditions: User has QuickCache open.**

**MSS**

1.  User requests to list flashcards
2.  QuickCache shows a list of flashcards
3.  User requests to delete a specific flashcard in the list
4.  QuickCache deletes the flashcard
5.  QuickCache <u>updates flashcard save file (UC09)</u>

    Use case ends.

**Extensions**

* 2a. The list is empty.

  Use case ends.

* 3a. The given index is invalid.

  * 3a1. QuickCache shows an error message.

    Use case resumes at step 2.

**Use case: UC04 - Delete multiple flashcards**

**Preconditions: User has QuickCache open.**

**MSS**

1.  User requests to list flashcards
2.  QuickCache shows a list of flashcards
3.  User requests to delete all flashcards with a specified tag
4.  QuickCache deletes all flashcards that contains the specified tag
5.  QuickCache <u>updates flashcard save file (UC07)</u>
6. QuickCache displays a message indicating that all flashcards with the specified tag has been deleted
7. QuickCache <u>updates flashcard save file (UC09)</u>

    Use case ends.

**Use case: UC05 - Create a flashcard with open ended question**

**Preconditions: User has QuickCache open.**

**MSS**

1.  User requests to add a flashcard
2.  QuickCache adds it to the list
3.  QuickCache <u>updates flashcard save file (UC09)</u>
4.  User requests to list flashcards
5.  QuickCache shows the list of flashcards including the recently added flashcard

    Use case ends.

**Extensions**

* 1a. The question is empty.

  * 1a1. QuickCache shows an error message.

    Use case ends.

* 1b. The answer is empty.

  * 1b1. QuickCache shows an error message.

    Use case ends.
* 1c. User provided more than one question or answer

  * 1c1. QuickCache shows an error message.

    Use case ends.


**Use case: UC06 - Create a flashcard with Multiple choice question**

**Preconditions: User has QuickCache open.**

**MSS**

1.  User requests to add a flashcard
2.  QuickCache adds it to the list
3.  QuickCache <u>updates flashcard save file (UC09)</u>
4.  User requests to list flashcards
5.  QuickCache shows the list of flashcards including the recently added flashcard

    Use case ends.

**Extensions**

* 1a. The question is empty.

  * 1a1. QuickCache shows an error message.

    Use case ends.

* 1b. The answer is empty.

  * 1b1. QuickCache shows an error message.

    Use case ends.

* 1c. The choices are empty.

  * 1c1. QuickCache shows an error message.

    Use case ends.

* 1d. User provided more than one question or answer

  * 1d1. QuickCache shows an error message.

    Use case ends.


**Use case: UC07 - Test a single flashcard**

**MSS:**

1. User requests to list flashcards
2. QuickCache shows a list of flashcards
3. User requests to test a specific flashcard in the list with a specific answer
4. QuickCache displays whether the answer is correct
5. QuickCache updates the statistics for that flashcard

    Use case ends.

**Extensions:**

* 2a. The list is empty.

  Use case ends.

* 3a. The given index is invalid.

  * 3a1. QuickCache shows an error message.

    Use case resumes at step 2.


**System: QuickCache**

**Use case: UC08 - Test a set of flashcards by category**

MSS:

1. User requests to list categories
2. QuickCache shows a list of categories
3. User requests to test a specific category in the list
4. User <ul>tests each flashcard on the list (UC07)</u>
5. QuickCache shows the number of successful questions at the end

    Use case ends.

**Extensions:**

* 2a. The list is empty.

  Use case ends.

* 3a. The given index is invalid.

  * 3a1. QuickCache shows an error message.

    Use case resumes from step 2.

* 3b. The user specifies a time limit.

  * 3b1. QuickCache starts a timer.

    Use case resumes from step 4.

* 3c. The user specifies to randomly sort the questions.

  * 3c1. QuickCache randomizes the flashcards from the category.

    Use case resumes from step 4.

* 4a. The user answers the question.

  * 4a1. QuickCache shows the next question to answer.

  * Step 4a. is repeated until all questions in the category is answered.

    Use case resumes from step 5.

* 4b. The user cancels the test midway.

  Use case resumes from step 5.

* 4c. The user runs out of time midway.

  Use case resumes from step 5.

* 4d. The user closes QuickCache

  Use case ends.



**Use case: UC09 - Update flashcard save file**

**Actor: QuickCache**

MSS:

1. QuickCache accepts update request
2. QuickCache updates save file

    Use case ends.

**Extensions:**

* 2a. No save file.

  * 3a1. QuickCache creates new save file.

    Use case resumes from step 2.

* 2b. Save file corrupted.

  * 2b1. QuickCache shows an error message.

  * 2b2. QuickCache creates new save file.

    Use case resumes from step 2.


**Use case: UC10 - Import flashcard data file**

MSS:

1. User specifies the file name containing the set of flashcards to import from
2. QuickCache imports each flashcard from the file

    Use case ends.

**Extensions:**

* 1a. File not found.

  * 1a1. QuickCache shows an error message.

    Use case ends.

* 1b. Data file corrupted.

  * 1b1. QuickCache shows an error message.

    Use case ends.

* 2a. Duplicate flashcard found.

  * 2a1. QuickCache ignores the duplicate flashcard.

    Use case resumes from step 3.


**Use case: UC11 - Add tags during creation of a flashcard**

MSS:

1. User creates a flashcard and specifies the tags associated with it
2. QuickCache creates the flashcards and adds the tags to it

    Use case ends.

**Extensions**

* 1a. The tags are invalid.

  * 1a1. QuickCache shows an error message.

    Use case ends.

**Use case: UC12 - Edit an existing flashcard**

MSS:

1. User wants to edit an existing flashcard
2. User enters new information pertaining to the flashcard fields he wants to update
4. QuickCache edits the flashcard with the new information
5. QuickCache <u>updates flashcard save file (UC09)</u>

    Use case ends.

**Extensions**

* 1a. The list is empty.

    Use case ends.

* 2a. The given index in invalid.

  * 2a1. QuickCache shows an error message.

    Use case resumes at step 2.

* 2b. User provides no field to edit.

  * 2b1. QuickCache shows an error message.

    Use case resumes at step 2.

* 3b. There is no change in the flashcard or the newly edited flashcard is the same as another flashcard.

  * 3b1. QuickCache shows an error message.

    Use case resumes at step 2.

**Use case: UC13 - Search for flashcards based on tags and/or question**

**MSS**

1. User enters the tags and/or keywords in the question associated with the flashcard he wants to search for
2. QuickCache filters all existing flashcards based on the tag and/or keywords.
3. Quickcache displays all the requested flashcards to the user.

    Use case ends.


**Use case: UC14 - Open a single flashcard**

**MSS**

1. QuickCache opens and shows a list of flashcards
2. User forgets the choices of a flashcard's question
3. User requests to open a specific flashcard in the list
4. QuickCache opens the flashcard and displays the choices

	Use case ends.

**Extensions**

* 4a. The given index is invalid.

  * 4a1. QuickCache shows an error message.

    Use case resumes at step 3.

**Use case: UC15 - Clear statistics of a flashcard**

**MSS**

1.  User requests for statistics of a flashcard
2.  QuickCache displays statistics of the flashcard
3.  User clears the statistics of the flashcard
4.  User requests for statistics of the flashcard
5.  QuickCache displays statistics, that has been reset, of the flashcard

    Use case ends.

**Extensions**

* 1a. The given index is invalid.

  * 1a1. QuickCache shows an error message.

    Use case resumes at step 1.

**Use case: UC16 - Export flashcard data file**

MSS:

1. User requests to list flashcards
2. QuickCache shows a list of flashcards
3. User specifies the file name to export the displayed list of flashcards
4. QuickCache exports displayed the list of flashcards into the specified file

    Use case ends.

**Extensions:**

* 3a. Invalid file name.

  * 3a1. QuickCache shows an error message.

    Use case ends.

**Use case: UC17 - Add difficulty during creation of a flashcard**

MSS:

1. User creates a flashcard and specifies the difficulty level associated with it
2. QuickCache creates the flashcards and adds the difficulty level to it

    Use case ends.

**Extensions**

* 1a. The difficulty level is invalid.

  * 1a1. QuickCache shows an error message.

    Use case ends.

****
### Non-Functional Requirements

1.  Should work on any _mainstream OS_ as long as it has Java `11` or above installed.
2.  Should be able to hold up to 1000 flashcards without a noticeable sluggishness in performance for typical usage.
3.  A user with above average typing speed for regular English text (i.e. not code, not system admin commands) should be able to accomplish most of the tasks faster using commands than using the mouse.

### Glossary

* **Mainstream OS**: Windows, Linux, Unix, OS-X
* **Private contact detail**: A contact detail that is not meant to be shared with others

--------------------------------------------------------------------------------------------------------------------

## **Appendix: Instructions for manual testing**

Given below are instructions to test the app manually.

<div markdown="span" class="alert alert-info">

:information_source: **Note:** These instructions only provide a starting point for testers to work on;
testers are expected to do more *exploratory* testing.

</div>

### 1. Launch and shutdown

1. Initial launch

   1. Download the jar file and copy into an empty folder

   1. Double-click the jar file<br>
       Expected: Shows the GUI with a set of sample flashcards. The window size may not be optimum but can be adjusted.

1. Shutdown after adding flashcards into QuickCache

   1. Type `exit` to exit the application

   1. Re-launch the app by double-clicking the jar file<br>
       Expected: The data of all the flashcards is retained.

### 2. Creating a flashcard

User can create two types of flashcards - containing open end question or multiple choice question.

1. Creating a flashcard with open ended question

   1. Prerequisites: The flashcard you are creating should not already be in QuickCache.
   
   1. Test Case 1: `add q/Test OEQ 1 ans/Test ans 1`<br>
      Expected: The flashcard with the specified parameters will be added to the list of flashcards. The details of the question will be shown in the display window on the side.
   
   1. Test Case 2: `add q/Test OEQ 2 ans/Test ans 2 t/OEQ t/test d/LOW`<br>
      Expected: The flashcard with the specified parameters will be added to the list of flashcards together with its two tags `OEQ`, `test` and difficulty level `LOW` shown. The details of the question will be shown in the display window on the side.

   1. Test Case 3: `add q/Test OEQ 1 ans/Test ans 1`<br>
      Expected: QuickCache responds with an error message indicating that the flashcard already exists (added in Test Case 1). Flashcard is not added.
   
   1. Test Case 4: `add q/Test OEQ 1 ans/Test ans 1 t/Invalid Tag`<br>
      Expected: QuickCache responds with an error message indicating that tag field is invalid. Flashcard is not added.<br>
      <div markdown="block" class="alert alert-info">
      
      :information_source: Note that flashcards containing similar questions and answers but different tags are treated as different flashcards.
      
      </div>
      
   1. Test Case 5: `add q/Test OEQ 1 ans/Test ans 1 d/Invalid Difficulty`<br>
      Expected: QuickCache responds with an error message indicating that difficulty field is invalid. Flashcard is not added.<br>
      <div markdown="block" class="alert alert-info">
      
      :information_source: Note that flashcards containing similar questions and answers but different difficulty are treated as different flashcards.
      
      </div>
   1. Some incorrect `add` commands with missing fields to try: `add`, `add q/ ans/Test ans 1`, `add q/Test OEQ 1 ans/`, `add q/Test OEQ 1 ans/Test ans 1 t/`<br>
      Expected: QuickCache responds with an error message and no flashcard is added.<br>
      <div markdown="block" class="alert alert-info">
      
      :information_source: Empty field for difficulty `d/` is accepted and flashcard difficulty is set to unspecified.
      
      </div>
   1. Some incorrect `add` commands with duplicate prefix to try: `add q/Test OEQ 1 q/Test OEQ 2 ans/Test ans 1`, `add q/Test OEQ 1 ans/Test ans 1 ans/Test ans 2`, `add q/Test OEQ 1 ans/Test ans 1 d/LOW d/HIGH`<br>
      Expected: QuickCache responds with an error message and no flashcard is added.

1. Creating a flashcard with multiple choice question

   1. Prerequisites: The flashcard you are creating should not already be in QuickCache.
   
   1. Test Case 1: `addmcq q/Test MCQ 1 ans/1 c/Choice1 c/Choice2`<br>
      Expected: The flashcard with the specified parameters will be added to the list of flashcards. The details of the question will be shown in the display window on the side.
   
   1. Test Case 2: `addmcq q/Test MCQ 2 ans/1 c/Choice1 c/Choice2 t/MCQ t/test d/LOW`<br>
      Expected: The flashcard with the specified parameters will be added to the list of flashcards together with its two tags `MCQ`, `test` and difficulty level `LOW` shown. The details of the question will be shown in the display window on the side.

   1. Test Case 3: `addmcq q/Test MCQ 1 ans/1 c/Choice1 c/Choice2`<br>
      Expected: QuickCache responds with an error message indicating that the flashcard already exists (added in Test Case 1). Flashcard is not added.
   
   1. Test Case 4: `addmcq q/Test MCQ 1 ans/1 c/Choice1 c/Choice2 t/Invalid Tag`<br>
      Expected: QuickCache responds with an error message indicating that tag field is invalid. Flashcard is not added.<br>
      <div markdown="block" class="alert alert-info">
      
      :information_source: Note that flashcards containing similar questions and answers but different tags are treated as different flashcards.
        
      </div>
      
   1. Test Case 5: `addmcq q/Test MCQ 1 ans/1 c/Choice1 c/Choice2 d/Invalid Difficulty`<br>
      Expected: QuickCache responds with an error message indicating that difficulty field is invalid. Flashcard is not added.<br>
      <div markdown="block" class="alert alert-info">
      
      :information_source: Note that flashcards containing similar questions and answers but different difficulty are treated as different flashcards.
      
      </div>
   1. Test Case 6: `addmcq q/Test MCQ 1 ans/3 c/Choice1 c/Choice2`<br>
      Expected: QuickCache responds with an error message indicating that ans field is invalid. Flashcard is not added.

   1. Some incorrect `addmcq` commands with missing fields to try: `addmcq`, `addmcq q/ ans/1 c/Choice1`, `addmcq q/Test MCQ 1 ans/ c/Choice1`, `addmcq q/Test MCQ 1 ans/1 c/`<br>
      Expected: QuickCache responds with an error message and no flashcard is added.<br>
      <div markdown="block" class="alert alert-info">
      
      :information_source: Empty field for difficulty `d/` is accepted and flashcard difficulty is set to unspecified.
   
      </div>
   1. Some incorrect `addmcq` commands with duplicate prefix to try: `addmcq q/Test MCQ 1 q/Test MCQ 2 ans/1 c/Choice1 c/Choice2`, `addmcq q/Test MCQ 1 ans/1 ans/2 c/Choice1 c/Choice2`<br>
      Expected: QuickCache responds with an error message and no flashcard is added.

### 3. Opening a flashcard

1. Prerequisites: 

   1. There is at least one flashcard stored in QuickCache.
   
   1. List flashcards using the `list` command to see the index of the flashcard.

1. Test Case 1: `open 1`<br>
   Expected: First flashcard is opened. Details of the question and options (for multiple choice questions) will be displayed.

1. Test Case 2: `open 0`<br>
   Expected: No flashcard is opened. Error details shown in display.

1. Other incorrect `open` commands to try: `open`, `open x` (where x is more than the last index in flashcard list), `open Invalid`<br>
   Expected: Error message will appear with instructions on how to use the `open` command.

### 4. Editing a flashcard

1. Prerequisites: 

   1. There is at least one flashcard stored in QuickCache.

   1. List flashcards using the `list` command to see the index of the flashcard.

   1. For some test cases listed bellow to work, user's first three flashcards should be the same as the sample starting flashcards that was provided.

1. Test Case 1: `edit 1 q/Edited question ans/New answer`<br>
   Expected: The flashcard with index 1 is edited with the specified parameters. The details of the question will be shown in the display window on the side.

1. Test Case 2: `edit 1 t/`<br>
   Expected: All tags are removed from the flashcard with index 1. The details of the question will be shown in the display window on the side.

1. Test Case 3: `edit 1 t/General t/OEQ`<br>
   Expected: Specified tags are added to the flashcard with index 1. The details of the question will be shown in the display window on the side.

1. Test Case 4: `edit 1 d/HIGH`<br>
   Expected: Difficulty level changed to `HIGH` for the flashcard with index 1. The details of the question will be shown in the display window on the side.

1. Test Case 5: `edit 1 d/`<br>
   Expected: Difficulty tag is removed from the flashcard with index 1. The details of the question will be shown in the display window on the side.<br>
   <div markdown="block" class="alert alert-info">
   
   :information_source: User can also use `edit 1 d/UNSPECIFIED` to achieve the same result.
   
   </div>
   
1. Test Case 6: `edit 1 c/Choice1`<br>
   Expected: Error message displayed. Choices should not be provided for open ended questions.

1. Test Case 7: `edit 2 c/Choice1 c/Choice2 c/Choice3 c/Choice4`<br>
   Expected: The flashcard with index 1 is edited with the specified parameters. The details of the question will be shown in the display window on the side.

1. Test Case 8: `edit 2 c/Choice1 c/Choice1`<br>
   Expected: Error message displayed. Choices should not be the same.

1. Test Case 9: `edit 2 c/Choice1`<br>
   Expected: Error message displayed. Number of choices smaller than the answer.

1. Test Case 10: `edit 2 ans/100`<br>
   Expected: Error message displayed. Number of choices smaller than the answer.

1. Test Case 11: `edit 0`<br>
   Expected: No flashcard is opened. Error details shown in display.

1. Other incorrect `edit` commands to try: `edit`, `edit x` (where x is more than the last index in flashcard list), `edit Invalid`<br>
   Expected: Error message will appear with instructions on how to use the `edit` command.

### 5. Finding flashcards

Users can fins flashcards both through keywords using the `q/` prefix and through tags using the `t/` prefix.

1. Prerequisites: For some test cases listed bellow to work, user should have the sample starting flashcards that was provided stored in QuickCache.

1. Test Case 1: `find q/Sample`<br>
   Expected: Finds all flashcards containing the keyword `Sample` (not case-sensitive) in its question. Found flashcards will be listed out.

1. Test Case 2: `find q/Sample q/Singapore`<br>
   Expected: Finds all flashcards containing the keyword `Sample` and `Singapore` (not case-sensitive) in its question. Found flashcards will be listed out.

1. Test Case 3: `find t/OEQ`<br>
   Expected: Finds all flashcards containing the tag `OEQ` (case-sensitive). Found flashcards will be listed out.

1. Test Case 4: `find q/Sample t/OEQ t/General`<br>
   Expected: Finds all flashcards containing the tags `OEQ` and `General` (case-sensitive) and keyword `Sample` (not case-sensitive) in its question. Found flashcards will be listed out.

1. Other incorrect `find` commands to try: `find`, `find Something`, `find q/`, `find t/`<br>
   Expected: Error message will appear with instructions on how to use the `find` command.

### 6. Deleting flashcards

There are 2 ways to delete flashcards – by index or by tags.

1. Prerequisites: List all flashcards using the `list` command. There is at least one flashcard stored in QuickCache.

1. Deleting a flashcard by index

   1. Test Case 1: `delete 1`<br>
      Expected: First flashcard is deleted from the list. Details of the deleted flashcard shown in the status message.

   1. Test Case 2: `delete 0`<br>
      Expected: No flashcard is deleted. Error details shown in the display.

1. Deleting flashcards through tags

   1. Test Case 1: `delete t/MCQ`<br>
      Expected: All flashcards with the tag `MCQ` is deleted

   1. Other incorrect `delete` commands to try: `delete`, `delete x` (where x is larger than the list size), `delete 1 t/MCQ` <br>
      Expected: Error message will appear with instructions on how to use the delete command.

1. Clearing all flashcards

   1. Test Case 1: `clear`
      Expected: All flashcards deleted from QuickCache.

### 7. Testing a flashcard

1. Prerequisites: List all flashcards using the `list` command. There is at least one flashcard stored in QuickCache.

1. Testing flashcard containing open ended question

   1. Test Case 1: `test 1 ans/Singapore`<br>
      Expected: Checks if answer provided matches with the answer stored in the open ended question within the first flashcard. Users starting off with the sample questions will expect test to be correct. Flashcard statistics will be updated.<br>
      <div markdown="block" class="alert alert-info">
  
      :information_source: Answers are not case-sensitive.
    
      </div>
   1. Test Case 2: `test 1 ans/`<br>
      Expected: Error message will appear as answer cannot be blank. Flashcard statistics will not be updated.

1. Testing flashcard containing multiple choice question

   1. Test Case 1: `test 2 o/2`<br>
      Expected: Checks if option provided matches with the option stored in the multiple choice question within the second flashcard. Users starting off with the sample questions will expect test to be correct. Flashcard statistics will be updated.<br>
      <div markdown="block" class="alert alert-info">
  
      :information_source: Answers are not case sensitive.
      
      </div>
   1. Test Case 2: `test 1 o/`<br>
      Expected: Error message will appear as option cannot be blank. Flashcard statistics will not be updated.

   1. Other incorrect `test` commands to try: `test`, `test x ans/...` (where x is 0 or larger than the list size)<br>
      Expected: Error message will appear with instructions on how to use the test command.

### 8. Displaying statistics

There are 2 ways to display statistics of flashcards – by index or by tags.

1. Prerequisites: For some test cases listed bellow to work, user should have the sample starting flashcards that was provided stored in QuickCache.

1. Displaying statistics of a flashcard by index

   1. Test Case 1: `stats 1`<br>
      Expected: Display statistics of the first flashcard.

   1. Test Case 2: `stats 0`<br>
      Expected: No flashcard statistics. Error details shown in display.

   1. Other incorrect `stats` commands to try: `stats`, `stats x` (where x is more than the last index in flashcard list), `stats Invalid`<br>
      Expected: Error message will appear with instructions on how to use the `stats` command.

1. Displaying statistics of flashcards through tags

   1. Test Case 1: `stats t/MCQ`<br>
      Expected: Display statistics of all flashcards containing tag `MCQ`.

   1. Test Case 2: `stats t/MCQ t/CS2103`<br>
      Expected: Display statistics of all flashcards containing tags `MCQ` and `CS2103`.

   1. Test Case 3: `stats t/`<br>
      Expected: Error message will appear on display. Tags cannot be empty.

### 9. Clearing statistics in a flashcard

1. Prerequisites: 

   1. There is at least one flashcard stored in QuickCache.
   
   1. List flashcards using the `list` command to see the index of the flashcard.

1. Test Case 1: `clearstats 1`<br>
   Expected: Statistics for the first flashcard is cleared.

1. Test Case 2: `clearstats 0`<br>
   Expected: No flashcard statistics is cleared. Error details shown in display.

1. Other incorrect `clearstats` commands to try: `clearstats`, `clearstats x` (where x is more than the last index in flashcard list), `clearstats Invalid`<br>
   Expected: Error message will appear with instructions on how to use the `clearstats` command.

### 10. Sharing flashcards

User can choose to import or export their data

1. Export data

   1. Prerequisites: Data file present within QuickCache.
   
   1. Test case 1: `export my-flashcard.json`<br>
      Expected: File containing the flashcards will be exported into the export folder, located in the same directory as QuickCache.jar.

1. Import data

   1. Prerequisites: Create an import folder in the same directory as where QuickCache.jar is located. Place the file that you want to import in the import folder. For testing purposes, we will name this file as `my-flashcard.json`.
   
   1. Test case 1: `import my-flashcard.json`<br>
      Expected: Flashcards within the file will be imported in your local QuickCache.<br>
      <div markdown="block" class="alert alert-info">
      
      :information_source: Flashcards that has previously been imported and has not been modified will be ignored. Flashcards that already exists will not be imported as well.

      </div>
      
### 11. Saving data

1. Dealing with missing data file

   1. Users can simulate missing data file by deleting the json file in the data folder in the same directory as where QuickCache.jar.<br>
      Expected: Upon restart, QuickCache will create a new data file in the data folder containing the sample flashcards for the user.

1. Dealing with corrupted data files

   1. Users can simulate corrupted data file by deleting parts of the json file in the data folder in the same directory as where QuickCache.jar.<br>
      Expected: Upon restart, QuickCache will create a new empty data file in the data folder.

## **Appendix: Effort** ##

### Difficulty

The difficulty level for developing QuickCache was high because we chose to morph AB3 into a flashcard manager instead of evolving it into a more powerful contact management app. We also wanted QuickCache to support both Open Ended Questions and Multiple Choice Questions. Other than managing questions, we felt that it was also important for the user to be able to test him/herself within QuickCache, view his/her performance, as well as share flashcards with his/her friends.

### Challenges

We faced a number of challenges in developing QuickCache:
1. The biggest hurdle that we had to overcome was having to work together remotely for the entire project. This was also our first project module in SoC where we had to work together in a large group of five.
2. In addition, we also had to optimize experience of the application for viewing flashcards - some flashcards contained choices and to display statistics of flashcards. It was important that we had to reveal it to the user in a way that is intuitive to them. Since AB3's GUI was designed to only show one possible window (the list of people stored in it), we had to examine the GUI code, understand how it works and made sure the data given from the command result of `OpenCommand`, `TestCommand`, etc. can be displayed.
3. As QuickCache supports different types of questions, we also had to implement additional entity types and made us of the `Question` interface so that our flashcard can support different type of questions.
4. To be able to show statistics, it was important for us to maintain a state on each flashcard, even between sessions. To solve this, we created a `Statistics` class and used it to keep track of the number of times a question is tested and is tested correctly.

### Effort

In terms of effort compared to AB3, we felt that QuickCache took at least the same amount of effort, or twice that of AB3 to develop. You can refer to the table below to see the additions and extensions we made to the commands that AB3 came with:

| Commands | AB3 | QuickCache | Extensions |
| --- | --- | --- | --- |
| add | Y | Y | Add supports both Multiple Choice and Open Ended questions |
| edit | Y | Y | Edit supports both Multiple Choice and Open Ended questions |
| delete | Y | Y | Delete supports both specific flashcard index or bulk deletion by tags |
| list | Y | Y | |
| exit | Y | Y | |
| open | | Y | |
| clear | | Y | |
| test | | Y | |
| stats | | Y | Stats supports both specific flashcard index and aggregation by tags |
| clear stats | | Y | |
| import | | Y | |
| export | | Y | |

We also overhauled the entire `Person` model, and created our own `Flashcard` model, which makes use of the `Question` interface to support multiple entity types.

The following class diagram shows the structure of `Person`:

![BetterModelClassDiagram](images/BetterModelClassDiagram.png)

The following class diagram shows the structure of `Flashcard`:

![FlashcardClassDiagram](images/FlashcardClassDiagram.png)

### Achievements

Overall, we are proud of coming up with a complete product amidst the work from the other modules that we have and our individual commitments. We put in a lot of hard work into our documentation as well to make sure QuickCache is as easy to use for the end-user (the student) as possible. We have also improved the test coverage and achieved a codecov of 74%.
