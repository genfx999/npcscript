---
layout: documentation
title: "Instruction reference"
section: "Reference"
order: 6
permalink: /instruction-reference/
description: "Rows run in order unless a control-flow instruction changes the pointer. A waiting row yields to Unity and retries on later ticks. Exit is optional at..."
---

Rows run in order unless a control-flow instruction changes the pointer. A waiting row yields to Unity and retries on later ticks. `Exit` is optional at the physical end of a program.

| Row | Inputs and example | Behavior and common errors |
| --- | --- | --- |
| Call | Function, typed arguments and optional result destination. Example: **Say** with “Hello”. | Calls an attributed method. Choose a declared Local, Global or Member to store a returned value where the editor offers a result field. Missing commands, unresolved arguments and type errors fail the row. A blocking method must return true to advance. |
| If | A variable or condition function, comparison, and a right-hand expression when required. Example: **Response == 1**. | Runs the following branch when true. Insert a matching End If. Choose an appropriate comparison for booleans, numbers and object references. |
| Else | No arguments. | Starts the alternate branch at the same If nesting level. One Else per If is supported. |
| End If | No arguments. | Closes the matching If regardless of whether it has an Else. A mismatched marker is a structural error. |
| While | Same inputs as If. Example: **Local.Count < 3**. | Re-evaluates the condition before each iteration. Change the controlling state or yield to avoid an accidental infinite loop. |
| End While | No arguments. | Jumps back to the matching While to recheck it. |
| Jump | Another row from the jump target picker. | Continues at the row with the saved ID. A missing target fails execution. |
| Exit | No arguments. | Ends the current program or returns to its invoker. |
| Global | Name, such as `SharedCount`. | Declares shared state with numeric zero if absent. Prior state is retained. `Player` is a special read-only lookup. |
| Member | Name, such as `Visits`. | Declares state on this runner with numeric zero if absent. |
| Local | Name, such as `Step`. | Declares state for the current invocation with numeric zero if absent. |
| Local Transform | Variable name and child name. Example: `Speaker` and `Head`. | Searches nested children, including inactive ones. An absent child binds null and reports a warning. |
| Set | Writable variable and value expression. Example: **Local.Step = 0**. | Replaces the value. Declare the destination first. |
| Add | Writable variable and numeric expression. Example: **Local.Step += 1**. | Adds to a numeric value. Use a negative amount to subtract. |
| YesNo | Writable variable and numeric or boolean expression. | Stores the string `Yes` for true or nonzero, and `No` for false or zero. |
| Exec | An expression, such as `GameObject.Find("Lamp").SetActive(true)`. | Runs for side effects. Use exact public instance member names. Missing methods or null receivers fail the row. Use a Call row for commands that need to wait. |
| Prompt | Text and one to nine responses. | Waits for the active prompt's response. Empty responses and missing text are errors. |
| Invoke | Program asset or program variable, optional target, and positional arguments. | Invokes a program on this or a second runner. The waiting option and continue-on-failure option govern the caller's behavior. An in/out argument must reference a declared writable variable. |
| Parameter | Name and Input or In Out mode, at the top of the called program. | Defines a positional input and, for In Out, copies the value back after invocation. Duplicate declarations and argument count mismatches are errors. |
| Context | Speaker, communication type, persona and voice fields. | Supplies localization and recording metadata. Context rows advance without executing a gameplay action. |
| Comment | Note text. | Documents a program and advances immediately. |

## Built-in functions

The [Function reference]({{ '/function-reference/' | relative_url }}) lists each built-in function, its parameters and defaults, and when it waits.
