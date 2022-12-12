# CommTools

Basic communication tools for the session, mainly used for user commands.

Comes with two functions: `YesOrNo` and `Select`.

## YesOrNo

### Overview

Asks a simple question and allows just "Yes" or "No" as answers.

You may specify a default via the optional left argument which when passed
rules what happens when the user just presses <enter>. `default` must be either 1 (yes) or 0 (no).

Example:

```
      1 CommTools.YesOrNo' Are you sure?'
      
 Are you sure? (Y/n)
 ```

### Interrupting

It is not possible to interrupt `YesOrNo` with either a weak or a strong interrupt. However, by entering `∘∘∘` the user can force `YesOrNo` to run onto a stop vector that is dynamically set.

### Auto-answering

For test cases it is possible to let `YesOrNo` answer any question automatically. For that to work you must specify a global variables inside `CommTools` by the name `YesOrNo_Answers` with two columns:

`[;1]`

: Character vector that must match the question 

`[;2]`

: either "y" or "n" or an empty vector; in case of an empty vector the default (if any) is returned
 
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

By passing one or two additional flags via `⍺` one can allow the user to select more than one options, or force he to select a option being able to quit.

### Interrupting

It is not possible to interrupt `Select` with either a weak or a strong interrupt. However, by entering `∘∘∘` the user can force `Select` to run onto a stop vector that is dynamically set.

### Auto-selection

For test cases it is possible to let `Select` select an option automatically. For that to work you must specify a global variable inside `CommTools` by the name `Select_Choices` with two columns:

`[;1]`

: The caption (left argument passed on to `Select`)

`[;2]`

: either an integer or a vector of integers or an empty vector or a character vector

* One or more integers are interpreted as user input = index into the options presented
* An empty vector is the equivalent of entering `q` (for "quit")
* If it's just an `a` (for all) then all options are selected
* A character vector may match one of the options, in which case that option is selected

  If none of the options matches then the options are restricted to the length of the character vector and the match operation is repeated.