---
:layout: page
title: User Guide
---

# Welcome to QuickCache User Guide

This User Guide is co-written by Josiah, Joshua, Gilbert, Francis and Xingjian. We are computer science students from National University of Singapore, and members of AY2021S1-CS2103T-T13-2.

--------------------------------------------------------------------------------------------------------------------

## Table of Contents

<link rel="stylesheet" href="UserGuide.css">

1. [Introduction](#intro)
2. [Using this guide](#using-this-guide)
3. [Quick start](#quick-start)
4. [Features](#features)
    1. [Viewing help](#help)
    2. [Creating a new flashcard](#creating-a-new-flashcard)
        1. [Open ended question](#oeq)
        2. [Multiple choice question](#mcq)
    3. [Opening a flashcard](#opening-a-flashcard)
    4. [Editing a flashcard](#editing-a-flashcard)
    5. [Listing all flashcards](#listing-all-flashcards)
    6. [Finding flashcards](#finding-flashcards)
        1. [Finding by question](#finding-by-question)
        2. [Finding by tags](#finding-by-tags)
        3. [Finding by tags and question](#finding-by-tags-and-question)
    7. [Deleting flashcards](#deleting-flashcards)
        1. [Deleting by index](#deleting-by-index)
        2. [Deleting by tags](#deleting-by-tags)
        3. [Clearing all entries](#clear)
    8. [Testing a flashcard](#testing-flashcard)
        1. [Containing an open-ended question](#testing-oeq)
        2. [Containing a multiple choice question](#testing-mcq)
    9. [Displaying statistics](#displaying-statistics)
        1. [Statistics by index](#stats-by-index)
        2. [Statistics by tags](#stats-by-tags)
    10. [Clearing a flashcard's statistics](#clearing-a-flashcards-statistics)
    11. [Sharing flashcards](#sharing-flashcards)
        1. [Exporting a set of flashcards](#export)
        2. [Importing a set of flashcards](#import)
    12. [Exiting the program](#exit)
    13. [Saving the data](#save)
5. [FAQ](#faq)
6. [Command summary](#command-summary)

--------------------------------------------------------------------------------------------------------------------

<a name="intro"></a>
## Introduction to QuickCache

**QuickCache** is a desktop app for managing flashcards, optimized for use via a Command Line Interface (CLI) while still having the benefits of a Graphical User Interface (GUI).

If you are a student who can type fast and loves organizing your study materials, **QuickCache** can get your flashcard management tasks done faster than any traditional GUI appplication.

The **QuickCache User Guide** helps you get started with using **QuickCache**. This user guide aims to walk you through the robust features that **QuickCache** has to offer and take you from zero to hero. Don't like reading? A summary is available at the end for your reference.


--------------------------------------------------------------------------------------------------------------------

<a name="using-this-guide"></a>
## Using this guide

Before diving into **QuickCache** and getting to know its features, lets get familiar with the symbols used in this user guide.

:information_source: This symbol represents important information

:exclamation: This symbol represents warnings

:bulb: This symbol represents additional information


--------------------------------------------------------------------------------------------------------------------

<a name="quick-start"></a>
## Quick start
Here is a quick start on how you can start using our app in your own computer.

1. Ensure you have Java `11` or above installed in your Computer.

1. Download the latest `quickcache.jar` from [here](https://github.com/AY2021S1-CS2103T-T13-2/tp/releases).

1. Copy the file to the folder you want to use as the _home folder_ for QuickCache.

1. Double-click the file to start the app. A GUI similar to the one below should appear in a few seconds. Note how the app contains some sample data.<br>

   ![Ui](images/Ui.png)

1. Type the command in the command box and press Enter to execute it. e.g. typing **`help`** and pressing Enter will open the help window.<br>
   Some example commands you can try:

   * **`add`**`q/Plants give out ___ when they photosynthesise? ans/Oxygen t/Biology` :  Adds an open ended question `Plants give out ___ when they photosynthesise?` with answer `Oxygen` and tagged to `Biology`.

   * **`addmcq`**`q/Plants give out ___ when they photosynthesise? ans/1 c/Oxygen c/Carbon c/Carbon dioxide` :  Adds a multiple choice question `Plants give out ___ when they photosynthesise?` with 3 options `Oxygen`, `Carbon`, `Carbon dioxide` and with answer `Oxygen`.

   * **`open`**`1` : Opens the 1st question shown in the current list.

   * **`edit`**`1 ans/Edited answer` : Edit the answer of the first flashcard on the list to become `Edited answer`.

   * **`list`** : Lists all flashcards.

   * **`find`** `find t/MCQ q/All mammals q/survive t/Biology` : Finds all Flashcards tagged to the tag `MCQ` and `Biology` and has keywords `All`, `mammals` and `survive` in question.

   * **`delete`**`3` : Deletes the 3rd flashcard shown in the current list.

   * **`clear`** : Deletes all flashcards.

   * **`test`**`1 ans/Example answer` : Tests the 1st question shown in the current list with `Example answer` as the answer.

   * **`stats`**`1` : Show stats of the 1st question shown in the current list.

   * **`clearstats`**`1` : Clears the statistics of the 1st flashcard shown in the current list.

   * **`export`**`science-questions.json` : Exports current list of flashcard to a file named `science-questions.json` in the `export` folder.

   * **`import`**`science-questions.json` : Import all flashcards from a file named `science-questions.json` in the `import` folder.

   * **`exit`** : Exits the app.

1. Refer to the [Features](#features) below for details of each command.

--------------------------------------------------------------------------------------------------------------------

<a name="features"></a>
## Features

<div markdown="block" class="alert alert-info">

**:information_source: Notes about the command format:**<br>

* Words in `UPPER_CASE` are the parameters to be supplied by the user.<br>
  e.g. in `add ans/ANSWER`, `ANSWER` is a parameter which can be used as `add ans/Oxygen`.

* Parameters can be in any order.<br>
  e.g. if the command specifies `q/QUESTION ans/ANSWER`, `q/ANSWER ans/QUESTION` is also acceptable.

* Items in square brackets are optional.
  e.g. q/QUESTION [t/TAG] can be used as q/What is my name? t/personal or as q/What is my name? .

* Items with ... after them can be used multiple times.
  e.g. [c/CHOICE]... can be used as c/Choice1, c/Choice1 c/Choice2 etc.

</div>

<a name="help"></a>
### Viewing help : `help`

If you want to see a step-by-step guide for all QuickCache commands, you can get the URL to the QuickCache user guide by using the help command.

Format: `help`

![HelpMessage](images/helpMessage.png)

<a name="creating-a-new-flashcard"></a>
### Creating a new flashcard
You can add a new flashcard to the list.

<div markdown="block" class="alert alert-danger">
:exclamation: You cannot have multiple question and answer prefixes.
</div>

<a name="oeq"></a>
#### Creating a flashcard with open ended question: `add q/QUESTION ans/ANSWER [t/TAG]... [d/DIFFICULTY]`
You can create a flashcard that contains an open ended question which will be added to the list.

1. You can use the add command to add a flashcard with an open ended question which will be added to the list.
For example, you can enter`add q/Sample Question ans/Sample Answer`

    ![addOpenEnded](images/addOpenEnded.png)

2. You have successfully created a flashcard with an open ended question and it will be saved immediately in the list.

    ![addOpenEnded2](images/addOpenEnded2.png)

<div markdown="block" class="alert alert-danger">
:exclamation: You cannot add a flashcard with empty question and empty answer. Additionally, <code>DIFFICULTY</code>
can only be set to <code>LOW</code>, <code>MEDIUM</code>, <code>HIGH</code> or <code>UNSPECIFIED</code>.
</div>

<a name="mcq"></a>
#### Creating a flashcard with multiple choice question: `addmcq q/QUESTION ans/ANSWER c/CHOICE... [t/TAG]... [d/DIFFICULTY]`
You can create a flashcard that contains a multiple choice question which will be added to the list.

1. You can use the addmcq command to add a flashcard with a multiple choice question which will be added to the list.
For example, you can enter `addmcq q/Sample Question ans/1  c/Sample Choice c/Sample Choice 2`

    ![addMCQ](images/addMCQ.png)

2. You have successfully created a flashcard with a multiple choice question and it will be saved immediately in the list.

    ![addMCQ2](images/addMCQ2.png)

<div markdown="block" class="alert alert-danger">

:exclamation:
- You cannot add flashcard with missing question or missing answer or missing choice.
- <code>DIFFICULTY</code> can only be set to <code>LOW</code>, <code>MEDIUM</code>, <code>HIGH</code> or <code>UNSPECIFIED</code>.
</div>

<a name="opening-a-flashcard"></a>
### Opening a flashcard: `open INDEX`

To view a flashcard you can open it by its index.

<div markdown="block" class="alert alert-info">

:information_source: The INDEX refers to the index number shown on the last displayed flashcard list and it <strong>must be a positive integer</strong>.

</div>

1. Use the `list` command to first list all the flashcards. You can also use the `find` command to filter for a list of flashcards.

	![OpenIndexStep1](./images/OpenIndexStep1.png)

2. Using the indices of the displayed list, enter the open command followed by the index of the flashcard you want to open. For example, if you want to open the 1st flashcard in the displayed list, you can enter `open 1`.

	![OpenIndexStep2](./images/OpenIndexStep2.png)

3. Press enter and QuickCache will open the flashcard specified by the index.

	![OpenIndexStep3](./images/OpenIndexStep3.png)


You have successfully opened a flashcard!

<a name="editing-a-flashcard"></a>
### Editing a flashcard: `edit INDEX [q/QUESTION] [ans/ANSWER] [c/CHOICE]... [t/TAG]... [d/DIFFICULTY]`

You can edit a flashcard that you have created previously.

<div markdown="block" class="alert alert-danger">
:exclamation: At least one field of the flashcard must be changed!
</div>

1. Use the `list` command to first list all the flashcards. You can also use the `find` command to filter for a list of flashcards.

    ![edit](images/Edit.png)

2. Using the indices of the displayed list, enter the edit command followed by the index of the flashcard you want to edit.
For example, if you want to edit the 3rd flashcard in the displayed list, you can enter `edit 3 q/Updated Question`.

    ![edit2](images/Edit2.png)

3. Press enter and QuickCache will edit the flashcard specified by the index.

    ![edit3](images/Edit3.png)

You have successfully edited a flashcard.

<div markdown="block" class="alert alert-info">

:information_source:<br>
- When editing tags, the existing tags of the flashcard will be removed i.e adding of tags is not cumulative.
- You can remove all the flashcard’s tags by typing <code>t/</code> without specifying any tags after it.
- You can remove the flashcard’s difficulty by typing d/ without specifying any difficulty after it. Or you can manually set it to <code>d/UNSPECIFIED</code>.

</div>

<a name="listing-all-flashcards"></a>
### Listing all flashcards : `list`

If you want to see all the flashcards you have created, use the `list` command.

<a name="finding-flashcards"></a>
### Finding flashcards

To find flashcards, you can find them by specifying keywords within their questions and/or specifying their tags.

<div markdown="block" class="alert alert-info">

:information_source: You must provide at least one search parameter.

</div>

<div markdown="block" class="alert alert-danger">

:exclamation: Take note that only flashcards which fulfill **all** the specified search parameters will be displayed. Flashcards that fulfill only some search parameters will not be diplayed.

</div>

<a name="finding-by-question"></a>
#### Finding by question: `find q/KEYWORD...`

<div markdown="block" class="alert alert-info">

:information_source:<br>
- Words in spaced keywords will be treated as individual keywords. Example, the keyword `what is` will be treated as two keywords: `what` and `is`.<br>
- Keywords do not need to match exact word. Example, the keyword `Wha` will pick up questions containing `What` as a word.

</div>

1. Use the `list` command to first list all the flashcards.

	![FindQuestionStep1](./images/FindQuestionStep1.png)

2. Enter the find command followed by `q/` and the keyword in the question of the flashcards you want to find. For example, if you want to find flashcards with the keyword `CS2103` in their question in the displayed list, you can enter `find q/CS2103`.

	![FindQuestionStep2](./images/FindQuestionStep2.png)

3. Press enter and QuickCache will display all the flashcards with the keyword `CS2103` in their question.

	![FindQuestionStep3](./images/FindQuestionStep3.png)

<a name="finding-by-tags"></a>
#### Finding by tags: `find t/TAG1...`

<div markdown="block" class="alert alert-info">

:information_source: Tags are case-sensitive.

</div>

1. Use the `list` command to first list all the flashcards.

	![FindTagsStep1](./images/FindTagsStep1.png)

2. Enter the find command followed by `t/` and the tag of the flashcards you want to find. For example, if you want to find flashcards with the tag `Biology` in the displayed list, you can enter `find t/Biology`.

	![FindTagsStep2](./images/FindTagsStep2.png)

3. Press enter and QuickCache will display all the flashcards with the tag `Biology`.

	![FindTagsStep3](./images/FindTagsStep3.png)

<a name="finding-by-tags-and-question"></a>
#### Finding by tags and question: `find t/TAG... q/KEYWORD...`

<div markdown="block" class="alert alert-info">

:information_source:<br>
- Words in spaced keywords will be treated as individual keywords. Example, the keyword `what is` will be treated as two keywords: `what` and `is`.<br>
- Keywords do not need to match exact word. Example, the keyword `Wha` will pick up questions containing `What` as a word.<br>
- Tags are case-sensitive.

</div>

<div markdown="block" class="alert alert-danger">
:exclamation: Take note that finding flashcards based on both tags and question will only display flashcards that have both the specified tags **and** keywords. It will not display flashcards that only have either the specified tags or the specified keywords.

</div>

1. Use the `list` command to first list all the flashcards.

	![FindQuestionAndTagsStep1](./images/FindQuestionAndTagsStep1.png)

2. Enter the find command followed by `t/` and the tag of the flashcards you want to find. Proceed to include `q/` and the keyword in the question of the flashcards you want to find. For example, if you want to find flashcards with the tag `General` and the keyword `University` in the question, you can enter `find t/General q/University`.

	![FindQuestionAndTagsStep2](./images/FindQuestionAndTagsStep2.png)

3. Press enter and QuickCache will display all the flashcards with the tag `General ` and the keyword `University` in their question.

	![FindQuestionAndTagsStep3](./images/FindQuestionAndTagsStep3.png)

<a name="deleting-flashcards"></a>
### Deleting flashcards

To delete a flashcard, you can either delete it by its index or based off its tags.

<div markdown="block" class="alert alert-danger">

:exclamation: You can only delete based on index or based on tags but not both!

</div>

<a name="deleting-by-index"></a>
#### Deleting by index : `delete INDEX`

You can delete a flashcard based on the index shown in the last displayed list.


<div markdown="block" class="alert alert-info">

:information_source: The INDEX refers to the index number shown on the last displayed flashcard list and it <strong>must be a positive integer</strong>.

</div>

1. Use the `list` command to first list all the flashcards. You can also use the `find` command to filter for a list of flashcards.

	![DeleteIndexStep1](./images/DeleteIndexStep1.png)

2. Using the indices of the displayed list, enter the delete command followed by the index of the flashcard you want to delete. For example, if you want to delete the 3rd flashcard in the displayed list, you can enter `delete 3`.

	![DeleteIndexStep2](./images/DeleteIndexStep2.png)

3. Press enter and QuickCache will delete the flashcard specified by the index.

	![DeleteIndexStep3](./images/DeleteIndexStep3.png)

You have successfully deleted a flashcard!

<a name="deleting-by-tags"></a>
#### Deleting by tags : `delete t/TAG...`

You can delete a group of flashcards based on a specified tag. All flashcards that have this specified tag will be deleted.

<div markdown="block" class="alert alert-info">

:bulb: You can specify more than one tag to be used when deleting flashcards. But only flashcards with **all** of these specified tags will be deleted.

</div>

1. Use the `list` command to first list all the flashcards. You can also use the `find` command to filter for a list of flashcards.

	![DeleteIndexStep1](./images/DeleteTagStep1.png)

2. In the user input box, enter in the delete command together with the tags that you want to use as the criteria. For example, if you want to delete all flashcards with the tag `MCQ`, type `delete t/MCQ`.

	![DeleteTagStep2](./images/DeleteTagStep2.png)

3. Press enter and QuickCache will delete the flashcards accordingly.

	![DeleteTagStep3](./images/DeleteTagStep3.png)

You have successfully deleted all the flashcards with the specified tag(s)!

<a name="clear"></a>
#### Clearing all entries : `clear`

Suppose you are in a situation where you want to get rid of all the flashcards in QuickCache. You might find it troublesome to delete each flashcard one-by-one. Instead, you can utilize the clear command to clear all flashcards on QuickCache directly.

<a name="testing-flashcard"></a>
### Testing a flashcard

<a name="testing-oeq"></a>
#### Containing an open-ended question : `test INDEX ans/ANSWER`

You can test yourself with a flashcard containing open-ended question by specifying an answer.

1. Use the `list` command to first list all the flashcards. You can also use the `find` command to filter for a list of flashcards.

    ![TestStep1](./images/TestStep1.png)

2. Using the indices of the displayed list, enter the `test` command followed by the index of the flashcard you want to test and what you think the answer to the question is. For example, if you want to test the first flashcard in the displayed list with the answer `Singapore`, you can enter `test 1 ans/Singapore`.

    <div markdown="block" class="alert alert-info">
    :bulb: Answer is <b>case insensitive</b>.
    </div>
    <div markdown="block" class="alert alert-danger">
    :exclamation: The index <b>must be a positive integer</b> 1, 2, 3, ...
    </div>

    ![TestOpenStep2](./images/TestOpenStep2.png)

3. Press enter and QuickCache will tell you whether you got the question right.

    ![TestOpenStep3](./images/TestOpenStep3.png)

You have successfully tested yourself on an open-ended question!

<a name="testing-mcq"></a>
#### Containing a multiple choice question : `test INDEX o/OPTION`

You can also test yourself a flashcard containing a multiple choice question by specifying an option.

1. Use the `list` command to first list all the flashcards. You can also use the `find` command to filter for a list of flashcards.

    ![TestStep1](./images/TestStep1.png)

2. Using the indices of the displayed list, enter the `open` command followed by the index of the flashcard you want to see the options of. For example, if you want to see the options from the second flashcard in the displayed list, you can enter `open 2`.

    ![TestMCQStep2](./images/TestMCQStep2.png)

3. Using the indices of the previous displayed list, enter the `test` command followed by the index of the flashcard you want to test and what you think the answer to the question is. For example, if you want to test the second flashcard in the displayed list with the 1st option, you can enter `test 2 o/1`.

    <div markdown="block" class="alert alert-danger">
    :exclamation: The index and option <b>must both be a positive integer</b> 1, 2, 3, ...
    </div>

    ![TestMCQStep3](./images/TestMCQStep3.png)

4. Press enter and QuickCache will tell you whether you got the question right.

    ![TestMCQStep4](./images/TestMCQStep4.png)

You have successfully tested yourself on a multiple choice question!

<a name="displaying-statistics"></a>
### Displaying statistics

<div markdown="block" class="alert alert-danger">
:exclamation: You can only show statistics based on index or based on tags but not both!
</div>

Statistics include:

* The number of times and the percentage the user answers all flashcards containing the specified tag(s) correctly.
* The number of times and the percentage the user answers all flashcards containing the specified tag(s) incorrectly.

<a name="stats-by-index"></a>
#### Statistics by index: `stats INDEX`

You can display the statistics of a specified flashcard in a Pie Chart based on the last displayed list.

1. Use the `list` command to first list all the flashcards. You can also use the `find` command to filter for a list of flashcards.

	![StatsIndexStep1](./images/StatsIndexStep1.png)

2. Using the indices of the displayed list, enter the stats command followed by the index of the flashcard you want to view the statistics of. For example, if you want to view the statistics of the first flashcard in the displayed list, you can enter `stats 1`.

    <div markdown="block" class="alert alert-info">
    :information_source: The INDEX refers to the the index number shown on the last displayed flashcard list and it <strong>must be a positive integer</strong>.
    </div>

	![StatsIndexStep2](./images/StatsIndexStep2.png)

3. Press enter and QuickCache will display the statistics of the flashcard specified by the index.

	![StatsIndexStep3](./images/StatsIndexStep3.png)

You have successfully displayed the statistics of a flashcard!

<a name="stats-by-tags"></a>
#### Statistics by tags: `stats t/TAG...`

You can also display the statistics of multiple flashcards in a Pie Chart by specifying tags.

1. In the user input box, enter the stats command together with the tags that you want to use as the criteria. For example, if you want to display statistics for all flashcards with the tag `MCQ`, type `stats t/MCQ`.

    <div markdown="block" class="alert alert-info">
    
    :bulb: You can specify more than one tag to be used when displaying statistics of multiple flashcards. But only flashcards with **all** of these specified tags will be included in the aggregation.
    
    </div>

    ![StatsTagsStep1](./images/StatsTagsStep1.png)

2. Press enter and QuickCache will display the statistics of the flashcards containing the specified tags.

    ![StatsTagsStep2](./images/StatsTagsStep2.png)

You have successfully displayed the statistics of all flashcards containing the specified tags!

<a name="clearing-a-flashcards-statistics"></a>
### Clearing a flashcard's statistics : `clearstats INDEX`

To clear the statistics of a flashcard you can use the clearstats command.

<div markdown="block" class="alert alert-info">

:information_source: The INDEX refers to the the index number shown on the last displayed flashcard list and it <strong>must be a positive integer</strong>.

</div>

1. Use the `list` command to first list all the flashcards. You can also use the `find` command to filter for a list of flashcards.

	![ClearstatsIndexStep1](./images/ClearstatsIndexStep1.png)

	<div markdown="block" class="alert alert-info">
	<Strong>Optional Step:</Strong>
	You can first check the statistics of the flashcard you want to clear the statistics of.

	<img src="./images/ClearstatsIndexStepOptional1.png">

	</div>

2. Using the indices of the displayed list, enter the clearstats command followed by the index of the flashcard you want to clear the statistics of. For example, if you want to clear the statistics of the first flashcard in the displayed list, you can enter `clearstats 1`.

	![ClearstatsIndexStep2](./images/ClearstatsIndexStep2.png)

3. Press enter and QuickCache will clear the statistics of the flashcard specified by the index.

	![ClearstatsIndexStep3](./images/ClearstatsIndexStep3.png)

	<div markdown="block" class="alert alert-info">
	<Strong>Optional Step:</Strong>
	You can check that the statistics of the flashcard has been cleared.

	<img src="./images/ClearstatsIndexStepOptional2.png">

	</div>

You have successfully cleared the statistics of a flashcard!

<a name="sharing-flashcards"></a>
### Sharing flashcards

<a name="export"></a>
#### Exporting a set of flashcards : `export FILE_NAME`

You can export all flashcards from the last displayed list into a file named `FILE_NAME` for backup or sharing with your friends.

1. Use the `list` command to first list all the flashcards. You can also use the `find` command to filter for a list of flashcards.

    ![ExportStep1](./images/ExportStep1.png)

2. In the user input box, enter the `export` command together with the `FILE_NAME` you would like to save the flashcards into. For example, if you would like the file to be named as `josiah-flashcard.json`, you can enter `export josiah-flashcard.json`.

    <div markdown="block" class="alert alert-info">
    :bulb: The filename specified includes the file format extension e.g. file.json
    </div>

    ![ExportStep2](./images/ExportStep2.png)

3. Press enter and the file containing the flashcards will be exported into the `export` folder, located in the same directory as `QuickCache.jar`

    ![ExportStep3a](./images/ExportStep3a.png)

    Here is the file location on Mac OS.

    ![ExportStep3b](./images/ExportStep3b.png)

    ![ExportStep3c](./images/ExportStep3c.png)

    Here is the file location on Windows.

    ![ExportStep3bW](./images/ExportStep3bW.png)

    ![ExportStep3cW](./images/ExportStep3cW.png)


Voila! You have successfully exported your flashcards into a file.

<a name="import"></a>
#### Importing a set of flashcards : `import FILE_NAME`

You can import external flashcards into your local QuickCache as well.

1. Create an `import` folder in the same directory as where `QuickCache.jar` is located. The pictures shown are the equivalents
for both Mac OS and Windows.

    ![ImportStep1](./images/ImportStep1.png)

    ![ImportStep1W](./images/ImportStep1W.png)

2. Place the file that you want to import in the `import` folder.

    ![ImportStep2](./images/ImportStep2.png)

    ![ImportStep2W](./images/ImportStep2W.png)

3. In the user input box, enter the `import` command together with the name of the file you would like to import the flashcards from. For example, if the file to import from is named `joshua-flashcard.json`, you can enter `import joshua-flashcard.json`.

    <div markdown="block" class="alert alert-info">
    :bulb: The filename specified includes the file format extension e.g. file.json and duplicate flashcards will be ignored.
    </div>

    ![ImportStep3](./images/ImportStep3.png)

4. Press enter and the flashcards within the file will be imported in your local QuickCache.

    <div markdown="block" class="alert alert-info">
    :information_source: Flashcards that has previously been imported and has not been modified will be ignored. Flashcards that already exists will not be imported as well.
    </div>

    ![ImportStep4](./images/ImportStep4.png)

Good job! You have successfully imported flashcards from an external file.

<a name="exit"></a>
### Exiting the program : `exit`

You can exit QuickCache by using the exit command.

<a name="save"></a>
### Saving the data

For your convenience, QuickCache data are saved in the hard disk automatically after any command that changes the data. There is no need to save manually.

--------------------------------------------------------------------------------------------------------------------

<a name="faq"></a>
## FAQ

**Q**: Why can't I open QuickCache on my device?<br>
**A**: There are a few potential reasons as to why QuickCache is not opening on your device:

1. You first need to ensure that your device is a **desktop device** as QuickCache is not supported on mobile devices.
2. Next, you need to ensure that you have **Java 11 or above** installed on your device. You can refer to this [guide to check your java version](https://superuser.com/questions/1221096/how-do-i-check-what-version-of-java-i-have-installed).
3. Double clicking QuickCache doesn't work on every desktop device. If you are experiencing problems with this, you can try using terminal or the command prompt to navigate to the folder where `quickcache.jar` is located and then proceed to execute the following command **`java -jar quickcache.jar`**. Note that if you rename `quickcache.jar` you have to tweak the command accordingly.

**Q**: How do I transfer my flashcards to my friend?<br>
**A**: You can first [export](#export) the flashcards that are stored in QuickCache into your desired file format. After exporting, you can proceed to send the file to your friend over your favourite messaging application. Upon receving the file, your friend can then proceed to [import](#import) the flashcards into QuickCache.

**Q**: How can I contribute to QuickCache?<br>
**A**: There are a few ways you can contribute to QuickCache:

* If you are an investor, you can reach out to us [here](mailto:damith@comp.nus.edu.sg).
* If you are a developer, we greatly welcome you to contribute to QuickCache by submitting a pull request [here](https://github.com/AY2021S1-CS2103T-T13-2/tp/pulls). You can view QuickCache's source code at this [link](https://github.com/AY2021S1-CS2103T-T13-2/tp).

**Q**: How do I give suggestions or report for bugs on QuickCache?<br>
**A**: If you have something to suggest to us or experienced any bugs while using QuickCache, we would appreciate it if you post an issue on it [here](https://github.com/AY2021S1-CS2103T-T13-2/tp/issues).

--------------------------------------------------------------------------------------------------------------------

<a name="command-summary"></a>
## Command summary

If you need to quickly look up a command, we have added a table below to summarize all the commands offered by QuickCache.

<table>
    <thead>
        <tr>
            <th>Action</th>
            <th>Command</th>
            <th>Format</th>
            <th>Example</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan=2>
            	<Strong>Creating a flashcard</Strong>
            </td>
            <td rowspan=1>add</td>
            <td rowspan=1>
            	<code>add q/QUESTION ans/ANSWER [t/TAG]... [d/DIFFICULTY]</code>
            </td>
            <td rowspan=1>
            	<code>add q/Sample Question? ans/A</code>
            </td>
        </tr>
        <tr>
            <td rowspan=1>addmcq</td>
            <td rowspan=1>
            	<code>addmcq q/QUESTION ans/ANSWER <br>
            	c/CHOICE... [t/TAG]... [d/DIFFICULTY]</code>
            </td>
            <td rowspan=1>
            	<code>addmcq q/Sample Question? ans/1 c/A c/B c/C</code>
            </td>
        </tr>
        <tr>
            <td rowspan=3>
            	<Strong>Deleting flashcard(s)</Strong></td>
            <td rowspan=2>delete</td>
            <td rowspan=1>
            	<code>delete INDEX</code>
            </td>
            <td rowspan=1>
            	<code>delete 1</code>
            </td>
        </tr>
        <tr>
        	<td rowspan=1>
        		<code>delete t/TAG...</code>
        	</td>
          <td rowspan=1>
            	<code>delete t/Assembly</code>
         	</td>
        </tr>
        <tr>
        	<td rowspan=1>clear</td>
          <td rowspan=1>
            	<code>clear</code>
         	</td>
          <td rowspan=1>
            	<code>clear</code>
         	</td>
        </tr>
        <tr>
            <td rowspan=1>
            	<Strong>Editing a flashcard</Strong></td>
            <td>edit</td>
            <td rowspan=1>
            	<code>edit INDEX [q/QUESTION] [ans/ANSWER] [c/CHOICE]... [t/TAG]... [d/DIFFICULTY]</code>
            </td>
            <td rowspan=1>
            	<code>edit 1 q/Sample Question? ans/2 c/A c/C c/B</code>
            </td>
        </tr>
        <tr>
        	<td rowspan=1>
        		<Strong>Exit QuickCache</Strong>
        	</td>
        	<td>exit</td>
        	<td>
        		<code>exit</code>
        	</td>
        	<td>
        		<code>exit</code>
        	</td>
        </tr>
        <tr>
            <td rowspan=3>
            	<Strong>Finding flashcards</Strong></td>
            <td rowspan=3>find</td>
            <td rowspan=1>
            	<code>find q/KEYWORD...</code>
            </td>
            <td rowspan=1>
            	<code>find q/CS2103T q/What q/is</code>
            </td>
        </tr>
        <tr>
        	<td rowspan=1>
        		<code>find t/TAG...</code>
        	</td>
          <td rowspan=1>
            	<code>find t/Assembly t/MCQ</code>
         	</td>
        </tr>
        <tr>
        	<td rowspan=1>
        		<code>find [t/TAG]... [q/KEYWORD1]...</code>
        	</td>
          <td rowspan=1>
            	<code>find t/Assembly t/MCQ q/CS2100 q/What</code>
         	</td>
        </tr>
        <tr>
        	<td rowspan=3>
        		<Strong>Flashcard Statistics</Strong>
        	</td>
        	<td rowspan=2>stats</td>
        	<td rowspan=1>
        		Display statistics of a single flashcard:<code>stats INDEX </code>
        	</td>
        	<td rowspan=1>
        		<code>stats 1</code>
        	</td>
        </tr>
        <tr>
        	<td rowspan=1>
        		Display statistics based on tags:<code>stats t/TAG...</code>
        	</td>
        	<td rowspan=1>
        		<code>stats t/MCQ</code>
        	</td>
        </tr>
        <tr>
        	<td rowspan=1>clearstats</td>
        	<td rowspan=1>
        		Clear statistics:<code>clearstats INDEX</code>
        	</td>
        	<td rowspan=1>
        		<code>clearstats 1</code>
        	</td>
        </tr>
        <tr>
        	<td rowspan=1>
        		<Strong>Get user guide URL</Strong>
        	</td>
        	<td>help</td>
        	<td>
        		<code>help</code>
        	</td>
        	<td>
        		<code>help</code>
        	</td>
        </tr>
        <tr>
        	<td rowspan=2>
        		<Strong>Import and Export</Strong>
        	</td>
        	<td rowspan=1>import</td>
        	<td rowspan=1>
        		<code>import FILE_NAME</code>
        	</td>
        	<td rowspan=1>
        		<code>import science-questions.json</code>
        	</td>
        </tr>
        <tr>
        	<td rowspan=1>export</td>
        	<td rowspan=1>
        		<code>export FILE_NAME</code>
        	</td>
        	<td rowspan=1>
        		<code>export science-questions.json</code>
        	</td>
        </tr>
        <tr>
            <td rowspan=1>
            	<Strong>Listing flashcards</Strong></td>
            <td>list</td>
            <td rowspan=1>
            	<code>list</code>
            </td>
            <td rowspan=1>
            	<code>list</code>
            </td>
        </tr>
        <tr>
            <td rowspan=1>
            	<Strong>Opening a flashcard</Strong></td>
            <td>open</td>
            <td rowspan=1>
            	<code>open INDEX</code>
            </td>
            <td rowspan=1>
            	<code>open 1</code>
            </td>
        </tr>
        <tr>
        	<td rowspan=2>
        		<Strong>Testing a flashcard</Strong>
        	</td>
        	<td rowspan=2>test</td>
        	<td rowspan=1>
        		Open Ended: <code>test INDEX a/ANSWER</code>
        	</td>
        	<td rowspan=1>
        		<code>test 2 ans/Sample Answer</code>
        	</td>
        </tr>
        <tr>
        	<td rowspan=1>
        		MCQ: <code>test INDEX o/OPTION</code>
        	</td>
        	<td rowspan=1>
        		<code>test 3 o/1</code>
        	</td>
        </tr>
    </tbody>
</table>
