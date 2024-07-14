# CommTools

Basic communication tools using Quad (`⎕`) and Quote-Quad (`⍞`) for input and output.

This package is particularly useful for test cases and user commands.



## Overview

Comes with these functions and operators: 

* `YesOrNo 'Are you sure?'`
* `'Select the file you want to process' Select 'This.txt' 'That.txt' 'More.txt'`
* `Pause 'Make sure you are connected to the Internet'`
* `AskForNumber 'How many copies would you like to print'`
* `AskForText 'Enter your name'`

These functions and operators have these features in common:

* They can all be interrupted by entering `∘∘∘`
* They can be automated by setting certain variables within the `CommTools` namespace

  This allows you to automate tests that would otherwise require a human to interact with.
* They unify the user experience
* You may force the user to make a decision or enter data
* `Select` allows by default to select just one item, and the user may enter a "q" for "quit", but both can be changed via the left argument
* `AskForText` allows the definition of a default (returned in case the user presses `<return>` without entering any data)

`AskForNumber` and `AskForText` are operators that require a check function as left operand. The check function can be used to check the input and reject or accept it.

## Documentation

You may ask for detailed documentation with

```
      ]ADoc #.CommTools
```

