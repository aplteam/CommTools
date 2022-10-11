# CommTools

Basic communication tools for the session, mainly used for user commands and their APIs.

Comes with two functions

## YesOrNo

Asks a simple question and allows just "Yes" or "No" as answers.

You may specify a default via the optional left argument which when specified
rules what happens when the user just presses <enter>. `default` must be either 1 (yes) or 0 (no).

Example:

```
      1 CommTools.YesOrNo' Are you sure?'
      
 Are you sure? (Y/n)
 ```
 
 ## Select
 
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
