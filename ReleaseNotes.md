# Release notes

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
