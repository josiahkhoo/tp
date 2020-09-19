---
layout: page
title: User Guide
---


QuickCache is a **desktop app for managing flashcards, optimized for use via a Command Line Interface** (CLI) while still having the benefits of a Graphical User Interface (GUI). If you can type fast, QuickCache can get your flashcard management tasks done faster than traditional GUI apps.

* Table of Contents
{:toc}

--------------------------------------------------------------------------------------------------------------------

## Quick start

1. Ensure you have Java `11` or above installed in your Computer.

1. Download the latest `addressbook.jar` from [here](https://github.com/se-edu/addressbook-level3/releases).

1. Copy the file to the folder you want to use as the _home folder_ for your QuickCache.

1. Double-click the file to start the app. The GUI similar to the below should appear in a few seconds. Note how the app contains some sample data.<br>
   ![Ui](images/Ui.png)

1. Type the command in the command box and press Enter to execute it. e.g. typing **`help`** and pressing Enter will open the help window.<br>
   Some example commands you can try:

   * **`list`** : Lists all FlashCards.

   * **`add`**`add n/Plants give out ___ when they photosynthesise? n/Oxygen` :  Adds a question `Plants give out ___ when they photosynthesise?` with answer `Oxygen`.
   
   * **`test`**`1 a/Example answer` : Tests the 1st question shown in the current list with `Example answer` as the answer. 

   * **`delete`**`3` : Deletes the 3rd flashcard shown in the current list.

   * **`clear`** : Deletes all FlashCards.

   * **`exit`** : Exits the app.

1. Refer to the [Features](#features) below for details of each command.

--------------------------------------------------------------------------------------------------------------------

## Features

<div markdown="block" class="alert alert-info">

**:information_source: Notes about the command format:**<br>

* Words in `UPPER_CASE` are the parameters to be supplied by the user.<br>
  e.g. in `add n/ANSWER`, `ANSWER` is a parameter which can be used as `add n/Oxygen`.

* Parameters can be in any order.<br>
  e.g. if the command specifies `n/QUESTION p/ANSWER`, `p/ANSWER n/QUESTION` is also acceptable.

</div>

### Viewing help : `help`

Shows a message explaning how to access the help page.

![help message](images/helpMessage.png)

Format: `help`

### Adding a flashcard: `add`

Adds a new flashcard to the application.

Format: `add n/QUESTION n/ANSWER`

Examples:
* `add n/Plants give out ___ when they photosynthesise? n/Oxygen`

### Testing a flashcard : `test`

Tests a specified flashcard from the list.

Format: `test INDEX a/ANSWER`

* Tests the flashcard at the specified `INDEX`
* The index refers to the index number shown in the displayed flashcard list.
* The index **must be a positive integer**  1, 2, 3, …​
* The `ANSWER` is case-sensitive.

Examples:
* `list` followed by `test 1 a/Example answer` tests the 1st flashcard in the list with `Example answer` as the answer.

### Listing all flashcards : `list`

Shows a list of all flashcards currently created.

Format: `list`

### Deleting a flashcard : `delete`

Deletes the specified flashcard from the list.

Format: `delete INDEX`

* Deletes the flashcard at the specified `INDEX`.
* The index refers to the index number shown in the displayed flashcard list.
* The index **must be a positive integer** 1, 2, 3, …​

Examples:
* `list` followed by `delete 2` deletes the 2nd flashcard in the list.

### Clearing all entries : `clear`

Clears all entries from QuickCache.

Format: `clear`

### Exiting the program : `exit`

Exits the program.

Format: `exit`

### Saving the data

QuickCache data are saved in the hard disk automatically after any command that changes the data. There is no need to save manually.

--------------------------------------------------------------------------------------------------------------------

## FAQ

**Q**: How do I transfer my data to another Computer?<br>
**A**: Install the app in the other computer and overwrite the empty data file it creates with the file that contains the data of your previous QuickCache home folder.

--------------------------------------------------------------------------------------------------------------------

## Command summary

Action | Format, Examples
--------|------------------
**Add** | `add n/QUESTION n/ANSWER` <br> e.g., `add n/Plants give out ___ when they photosynthesise? n/Oxygen`
**Test** | `test INDEX a/ANSWER`<br> e.g., `test 2 a/lorem ipsum`
**Clear** | `clear`
**Delete** | `delete INDEX`<br> e.g., `delete 3`
**List** | `list`
**Help** | `help`
**Exit** | `exit`
