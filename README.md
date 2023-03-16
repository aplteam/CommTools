# CommTools

Basic communication tools for the session, mainly used by user commands and test cases.

Comes with these functions: 

* `YesOrNo` for asking simple yes/no-questions
* `Select` for letting the user choose from a set of options
* `Pause` for printing a message to the session and asking the user to press `<enter>` to continue

These functions have three features in common:

* They can all be interrupted by entering `∘∘∘`
* They can be automated in some way by setting certain variables within the `CommTools` namespace

  This allows you to automate tests that would otherwise require a human interact

## YesOrNo

### Overview

Asks a simple question and only allows "y" or "y" as answers.
You can specify a default via the optional left argument which, if passed, rules what happens when the user just presses "Enter". `default` must be either 1 (yes) or 0 (no).

Example:

```
      1 CommTools.YesOrNo' Are you sure?'
      
 Are you sure? (Y/n)
 ```

### Interrupting

Due to a long-standing bug (Mantis 14535), it is not possible to interrupt `YesOrNo` with either a weak or a strong interrupt. 
However, by entering `∘∘∘` the user can force `YesOrNo` to run onto a stop vector that is dynamically set.

### Automatic answering

For test cases it is possible to make `YesOrNo` answer any question automatically. For this to work, a global variable named `YesOrNo_Answers` must be specified in `CommTools` with two columns:

`[;1]` Character vector that must match the question 

`[;2]` either "y" or "n" or an empty vector; in case of an empty vector the default (if any) will be returned

To get rid of the global variable `YesOrNo_Answers` (and also `Select_Choices` and `NoPause`, see below) call `CoomTools.Cleanup`.

#### Dynamic captions

Sometimes the question is constructed dynamically. An example is when a path is included that is bound to change.

In this case you may specify an alias. By default an alias is separated by an `@` character.

For example:

```
      ⎕SE.CommTools.YesOrNo_Answers←1 2⍴'foo@' 'y'
      ⎕SE.CommTools.YesOrNo 'foo@Sure?:'
1
```
 
## Select

### Overview
 
 Presents a list of options and allows the user to select one of them (by default).
 
 Example:
 
 ```
       l←'Now' 'Tomorrow' 'Next week' 'Next month' 'Next year'
       'Please select one:' CommTools.Select l
--- Please select one: ---------------------------------------------------
   1. Now        
   2. Tomorrow   
   3. Next week  
   4. Next month 
   5. Next year  
Select one item (q=quit)        
 ```

By passing one or two additional flags via `⍺` you can allow the user to select more than one option, or force her to select a option without the default option to quit.

### Interrupting

Due to a long-standing bug (Mantis 14535), it is not possible to interrupt `Select` with either a weak or a strong interrupt.
However, by entering `∘∘∘` the user can force `Select` to run onto a stop vector that is dynamically set.

### Automatic selection

For test cases, it is possible to have `Select` automatically select an option. For this to work, a global variable named `Select_Choices` must be specified in `CommTools` with two columns:

`[;1]` The caption (left argument passed on to `Select`)

`[;2]` either an integer or a vector of integers or an empty vector or a character vector

* One or more integers are interpreted as user input ←→ index into the options presented
* An empty vector is the equivalent of entering `q` (for "quit")
* If it's just an `a` (for all) then all options are selected
* A character vector may match one of the options, in which case that option is selected

  If none of the options match then the options are restricted to the length of the character vector and the match operation is repeated.

To get rid of the global variable `Select_Choices` (and also `YesOrNo_Answers` and `NoPuase`) call `CoomTools.Cleanup`.

#### Dynamic captions

Sometimes the caption is constructed dynamically. An example is when a path is included that is bound to change.
In this case you may specify an alias. By default an alias is separated by an `@` character.

For example:

```
      ⎕SE.CommTools.Select_Choices←1 2⍴'foo@' 2
      'foo@Please select one option:'⎕SE.CommTools.Select ⍕¨1 2 3
2
```

## Pause

### Overview

Prints a message to the session which may contain `⎕UCS 10` characters (or be a nested vector), adds `(⎕UCS 10),'To continue press <enter>)` and waits for the user to do just that.

If an empty vector is passed as message then `Pause` does nothing at all.

The function returns a 1 in case the message was printed to the session and 0 otherwise.

If a 1 is passed as left argument a line is printed on top of the message to emphasize it.

### Interrupting

Due to a long-standing bug (Mantis 14535), it is not possible to interrupt `Pause` with either a weak or a strong interrupt.
However, by entering `∘∘∘` the user can force `Pause` to run onto a stop vector that is dynamically set.

### Automation

In some scenarios you may want to avoid what `Pause` normally does. This can be achieved by setting the global `NoPause` (which does not exist by default) to one of the following values:

* 1 means that `NoPause` does nothing at all
* A simple character vector that matches a message until either the first `⎕UCS 10` or the end
* A vector of simple character vectors with messages 
* 0 has the same effect as if `NoPause` would not exist

To get rid of the global variable `NoPause` (and also `YesOrNo_Answers` and `Select_Choices`) call `CoomTools.Cleanup`.

#### Dynamic captions

Sometimes the message is constructed dynamically. An example is when a path is included that is bound to change.
In this case you may specify an alias. By default an alias is separated by an `@` character.

For example:

```
      ⎕SE.CommTools.YesOrNo_Answers←1 2⍴'foo@' 'y'
      ⎕SE.CommTools.YesOrNo 'foo@Sure?:'
1
```

## Aliase

By default an alias is separated from the question (`YesOrNo`), the caption (`Select`) or the message (`Pause`) by the `@` character.

There is only one scenario when this would not work: in case you are forced to include the `@` character in your question/caption/message.
In this case you may assign a different character to the global variable `AliasChar`.