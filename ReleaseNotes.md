# Release notes

This document covers breaking changes only. For more details on releases call `]Adoc CommTools`


## Version 3.1.0

* `SetOnWait` and `UnsetOnWait` added: `SetOnWait` establishes a function that is called whenever a `CommTools` function is about to wait for input from the user, typically for notifying the user
* Bug fixes
  * `FindAutomationIndex`, and therefore `AddAutomation`, threw a RANK ERROR when `∆Automation` held 
    a one-character ID, such as th  `*` of `Pause`, and the ID in question was an alias
  * `Pause` threw a RANK ERROR when `∆Automation` held a `*` row as well as an alias row
  * `Pause` ignored a `*` row as soon as there were other `Pause` rows: a message that matched none
    of them stopped and waited for the user
  * `YesOrNo` threw a RANK ERROR when `∆Automation` held a one-character ID and the question had an alias

## Version 3.0.1

Bug fix: the new Select-syntax (entering "q" results in ¯1) could not be automated.

## Verssion 3.0.0

Breaking change: `Select` now returns ¯1 if the user enters "q" for quit. ⍬ is now only returned when the user did not enter anything (=did not make a selection).

## Version 2.0.0

With version 2.0.0 two breaking changes were introduced:

### Identifying a message/question/caption has changed

In the past, when message/question/caption was specified for automation and could not be identified, the `CommTools` functions tried to identify it by comparing it with the beginning of the actually used message/question/caption. In other words: using `⍷` rather than `≡`.

This feature has been removed: it potentially identified different calls with the same shortened message/question/caption.

Instead your are advised to use aliases: they don't allow ambiguity and they are easy to use.

### Names of the automation variables

Prior to version 2.0.0, every function had it's own variable for automation. That made things more complex than necessary.

Now there is just one global variable doing the automation: `∆Automation`. It's stil optional, so it only exist in case automation is attempted. It has four columns:

1. Type, which must be one of the major functions offered by `CommTools`: `AskForNumber`, `AskForText`, `Pause`, `Select` or `YesOrNo`
2. The identifier: either exactly the message/question/caption used, or an alias
3. The result to be returned in case of a match
4. Counter (how many times was this line triggered)

Note that there are now two Helpers available for creating/modifying this variable: `AddAutomation` and `ListAutomation`.



