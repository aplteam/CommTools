# CommTools

Ask the user a question in the APL session, and let your test suite answer it for you.

`CommTools` provides the handful of interactions a user command or a utility needs all the
time: yes/no questions, picking an item from a list, prompting for text or a number, and
stopping for an acknowledgement. They all look and behave alike, they can all be
interrupted, and, most importantly, they can all be **automated**, so code that talks to a
human can still be tested without one.

## What it looks like

```apl
      list←'This.txt' 'That.txt' 'More.txt'
      'Select the file you want to process'CommTools.Select list
--- Select the file you want to process ------------------------------
  1. This.txt
  2. That.txt
  3. More.txt

Select one item (q=quit) :2
2
```

There are no dependencies, and nothing but `⍞` is involved, so this works in any session,
in any Dyalog version from 18.0 onwards, on all platforms.

## The functions

| Call                                        | Asks the user to         | Returns                                        |
|:--------------------------------------------|:-------------------------|:-----------------------------------------------|
| `YesOrNo 'Are you sure?'`                   | answer yes or no         | 1 or 0                                         |
| `'Which file?' Select list`                 | select from a list       | item number(s), `⍬` for none, `¯1` for "quit"  |
| `(CheckFn AskForText) 'Enter your name'`    | enter a character vector | the text entered, or a default                 |
| `(CheckFn AskForNumber) 'How many copies?'` | enter a number           | the number                                     |
| `Pause 'Make sure you are connected'`       | press `<enter>`          | shy 1, or 0 when automation skipped it         |

`AskForText` and `AskForNumber` are operators: the left operand is a check function that
gets what the user entered and returns 1 to accept it or 0 to ask again. Both return an
empty vector when the user just presses `<enter>`; pass a 1 as left argument to insist on
an answer, or give `AskForText` a character vector as left argument to define a default.

Invalid input is rejected and the question repeated, so you never have to check the result
for garbage. Leading spaces in your prompts are removed, and a question mark or a colon is
added when you did not provide one.

## Why use it

* **One look and feel.** Every prompt your application shows is formatted the same way,
  no matter who wrote it.
* **It can be interrupted.** Due to a long-standing Dyalog bug you can issue neither a
  weak nor a strong interrupt while `⍞` waits for input. Entering `∘∘∘` gets you out.
* **It can be automated.** [See below](#automation-in-half-a-minute). This is what
  makes user commands testable.
* **You decide how much you insist.** Force a decision, or offer a default that
  `<enter>` accepts. `Select` takes just one item by default; ask for several, for all, or
  for a particular number of them, say exactly two, by passing `(2 2)`. You can also insist
  on a selection and still let the user quit.
* **It can tell you that it is waiting.** A long-running user command that finally asks a
  question does not have to sit there unnoticed. [See below](#knowing-when-the-session-waits).

## Automation in half a minute

Add an answer to the global `∆Automation` variable, and the function stops asking:

```apl
      CommTools.AddAutomation'YesOrNo' 'DeleteLogFile@' 'y'
      CommTools.ListAutomation''
 Type     ID              Returns  Counter
 ----     --              -------  -------
 YesOrNo  DeleteLogFile@  y              0
      CommTools.YesOrNo'DeleteLogFile@Delete /var/log/app-2026-08-31.log?'
1
```

The question was answered without a human, and the counter in `∆Automation` now says 1, so
a test can prove that the automation really did trigger.

`DeleteLogFile@` is an *alias*: everything up to the `@` identifies the question, and the
alias is removed before the question is shown to a real user. That is what makes prompts
with a dynamic part, such as the filename above, automatable at all.

> [!TIP]
> Give a prompt an alias as soon as you write it. A question you cannot identify is a
> question you cannot automate, and questions tend to grow a dynamic part later on.

When no entry matches, the function simply asks the user, so automation never gets in your
way. `CommTools.Cleanup` removes the variable again.

## Knowing when the session waits

You start a user command that runs for a while, switch to something else, and meanwhile it
has stopped to ask you a question. To be told, hand `SetOnWait` a function: `CommTools`
calls it whenever it is about to wait for input.

```apl
      ∇ r←Notify(type question)
        r←⍬
        ⍝ Pop up a toast, play a sound, call notify-send... but don't wait for it
      ∇
      #.Notify CommTools.SetOnWait 0
```

The function gets the type (`YesOrNo`, `Select`, `Pause`, `AskForText` or `AskForNumber`)
and the question without its alias, and it must return a result, which is ignored. It is
called once per question, and never when automation answers, so test runs stay quiet.

`CommTools.UnsetOnWait` removes the function again; `Cleanup` leaves it alone.

## Installation

Load it into the workspace, into `#` or any namespace you like:

```
      ]Tatin.LoadPackages [tatin]aplteam-CommTools #
```

User commands typically want it in `⎕SE`:

```
      ]Tatin.LoadPackages [tatin]aplteam-CommTools ⎕SE
```

To make it part of a project, install it into the project's dependency folder instead:

```
      ]Tatin.InstallPackages [tatin]aplteam-CommTools /path/to/your/project/packages
```

## Documentation

```
      ]ADoc CommTools
```

or call `CommTools.Help`, which does the same. It covers every function in detail, as well
as automation, aliases, the wildcard syntax and notifications. `CommTools.Public` lists the
public interface, and [ReleaseNotes.md](ReleaseNotes.md) records the breaking changes
between major versions.

## License

MIT, see [LICENSE](LICENSE).
