FXYT
====

FXYT is a tiny canvas colouring language that consists of 36 simple
stack-based commands.  The input code is evaluated for each cell of a
256x256 graphical canvas.  The colour of each cell is determined by
the result of the evaluation.  Here is an extremely simple FXYT code:

```
XY^
```

The result looks like this:

[![Screenshot of XOR pattern][IMG1]][DEMO1]

See some more demos here:

- [Demo 1](https://susam.net/fxyt.html#XYxTN1srN255pTN1sqD)
- [Demo 2](https://susam.net/fxyt.html#XYaTN1srN255pTN1sqN0)
- [Demo 3](https://susam.net/fxyt.html#XYoTN1srN255pTN1sqDN0S)
- [Demo 4](https://susam.net/fxyt.html#XYpTN1srN255pTN1sqD)
- [Demo 5](https://susam.net/fxyt.html#XYN256sTdrD)
- [Community Demos][demo.html]

Visit https://susam.net/fxyt.html to write your own code!

This project is inspired by Martin Kleppe's [Tixy][TIXY] project.
While Tixy supports JavaScript expressions to determine the size and
colour of circles in a 16x16 grid, FXYT comes with its own tiny,
stack-based language that is written in postfix notation.  Further,
FXYT provides a 256x256 grid of cells each of which can be painted
with an arbitrary colour determined by the result of the evaluation of
the input code.

This project is intentionally minimal in nature.  It is intended to be
a creative code golfing playground that offers the challenge of
crafting interesting visuals with a limited set of commands.

FXYT stands for *function of x, y, and t* and it may be pronounced
/fɒksɪt/ ("foxit").

[IMG1]: https://susam.github.io/blob/img/fxyt/fxyt-0.1.0-xor.png
[DEMO1]: https://susam.net/fxyt.html#XYx
[TIXY]: https://github.com/aemkei/tixy
[DRAW1]: https://susam.net/fxyt.html
[demo.html]: http://susam.github.io/fxyt/demo.html


Contents
--------

* [Coordinates](#coordinates)
* [Data Stack](#data-stack)
* [Loop Control Stack](#loop-control-stack)
* [Colours](#colours)
* [Integers](#integers)
* [Arithmetic](#arithmetic)
* [Division by Zero](#division-by-zero)
* [Comparison](#comparison)
* [Inversion](#inversion)
* [Bitwise Operations](#bitwise-operations)
* [Clip](#clip)
* [Duplicate](#duplicate)
* [Pop](#pop)
* [Swap](#swap)
* [Rotate](#rotate)
* [Loops](#loops)
* [Frame Interval](#frame-interval)
* [Print Data Stack](#print-data-stack)
* [Idioms](#idioms)
* [Common Mistakes](#common-mistakes)
* [Constraints](#constraints)
* [Distributable Links](#distributable-links)
* [FAQ](#faq)
* [License](#license)
* [Support](#support)
* [See Also](#see-also)


Coordinates
-----------

### X

Enter the following code in the input field:

```
X
```

The output consists of a blue gradient spread across the canvas that
gradually changes from black on the left hand side to bright blue on
the right hand side.

The input code is evaluated for each cell in a 256x256 graphical
canvas.  Each cell has a coordinate which we represent here as (x, y)
where x represents the column of the cell and y represents the row of
the cell.  The value of x varies from 0 to 255 as we move from the
leftmost column to the rightmost column.  Similarly, the value of y
varies from 0 to 255 as we move from the bottommost row to the topmost
row.

Thus the cell at the bottom-left corner of the canvas has the
coordinate (0, 0).  Similarly, the cell at the top-right corner of the
canvas has the coordinate (255, 255).

The command `X` pushes the x-coordinate value of the current cell to
the data stack.  When the above piece of code is evaluated for each
cell in the canvas, we get the result 0 for all cells at x = 0, the
result 1 for all cells at x = 1, and so on up to the final column
where the result is 255 for all cells at x = 255.  The result obtained
for each cell determines the colour of each cell.  In particular, in
this example, the result obtained for each cell determines the blue
component of the colour of each cell.  As a result, all cells at x = 0
appear black and all cells at x = 255 appear bright blue.


### Y

Now enter the following code:

```
Y
```

We get a blue gradient again.  However, this time the blue gradient
changes from black to bright blue as we move gradually from the
bottommost row of cells at y = 0 to the topmost row at y = 255.


### T

Finally, enter the following code:

```
T
```

Now we get an output that changes with time where the colour of the
entire canvas changes from black to blue gradually in 256 iterations.

We call any input code that contains at least one occurrence of the
`T` command as time-dependent code.  Such code is evaluated in 256
iterations represented with the time variable t that changes from t =
0 to t = 255.  By default, there is a 100 ms interval between two
iterations.  This interval is known as the frame interval.

Thus we can say that any time-dependent code is evaluated for each
cell at the coordinate (x, y, t) where the x-value and y-value
represent the location of the cell as explained before and t
represents the current iteration.

The command `T` pushes the t-value of the current iteration to the
stack.  When the above piece of code is evaluated, we get the result 0
for all cells at t = 0, the result 1 for all cells at t = 1, and so on
up to the final iteration when the result is 255 for all cells at t
= 255.  As a result, the colour of all the cells change from dark to
bright blue gradually with each iteration.


Data Stack
----------

The runtime environment contains exactly one data stack.  Most FXYT
commands manipulate this data stack.  This data stack can contain at
most 8 integer values.  It is an error to push a value to the stack
when the stack is full.

In the remainder of this document, sometimes we will provide examples
of stack.  Such examples will be written using an array notation where
the elements that appear on the left are at the bottom of the stack
and the elements on the right are at the top of the stack.  For
example, a stack that contains (from bottom to top) the values 10, 20,
30, and 40 will be written as [10, 20, 30, 40].  If we push 50 to this
stack, we will write the resulting stack as [10, 20, 30, 40, 50].


Loop Control Stack
------------------

Apart from the data stack, there is a loop control stack.  Unlike the
data stack, the loop control stack cannot be manipulated directly.
The loop control stack is an internal implementation detail of the two
looping commands `[` and `]` introduced later in this document.  Each
entry in the loop control stack is a pair of integers: loop counter
and loop body position.  This stack can contain at most 8 such
entries.  As a result, while executing nested loops, at most 8 nested
loop bodies can be entered.  It is an error to enter a loop that is 9
levels deep.  To learn more about loops, see the section
[Loops](#loops).


Colours
-------

Commands of FXYT manipulate the data stack.  As mentioned before, the
input code is evaluated for each cell at (x, y) for time-independent
code.  If the input code is time-dependent, then it is evaluated for
each cell at (x, y, t).

At the end of evaluation of the code for each cell, the top 3 values
of the data stack is inspected to determine the red, blue, and green
(RGB) components of the colour of the cell.  The value at the top of
the data stack is used as the blue component, the second value from
the top is the green component, and the third value from the top is
the red component.

Suppose the data stack looks like the following after the evaluation of the
input code for a certain cell: [10, 20, 30, 40, 50].  Then the colour
used to paint that cell in RGB notation is rgb(30, 40, 50).

For example, consider the following code:

```
YYY
```

When this code is evaluated for each cell of the canvas, the
y-coordinate value of the cell is pushed to the data stack three
times.  At the end of each evaluation, the three values at the top of
the data stack determine the RGB components of the colour.  Since they
are all equal, we get a shade of grey for each cell.

If less than three values exist on the data stack at the end of
evaluation, then values for the missing RGB components are set to 0.
For example, consider the following code:

```
YY
```

At the end of evaluation, the data stack contains only two values, so
they decide the values of the blue and green components.  The red
component is set to 0.  Thus each cell gets a shade of cyan.  Finally
consider this code:

```
Y
```

This time we get only one value on the data stack at the end of each
evaluation and that only value determines the blue component of the
colour of each cell.

If the data stack is empty at the end of evaluation, then all three
components of the RGB colour is set to 0 and the corresponding cell is
painted black.  For example, consider the following code:

```
XP
```

The `X` command pushes the x-coordinate value to the data stack but
then the `P` command pops it off the data stack.  Thus the data stack
becomes empty at the end of each evaluation.  As a result, all cells
are painted black.  In fact, when the input code is empty, the
evaluation of the empty code results in an empty stack thus leading to
a black canvas too.

The three values at the top of the data stack must be integers between
0 and 255, inclusive, because they are interpreted as the red, blue,
and green components of the resulting RGB colour.  If any value among
these three values is negative, it is an error.  Similarly, if any of
these values exceed 255, it is an error.

If an error occurs during the evaluation of the code, the error is
displayed in the status panel and the entire canvas is painted red.


Integers
--------

The commands `X`, `Y`, and `T` push integer values to the data stack
that correspond to the x, y, and t coordinates, respectively, of the
current cell.  It is also possible to place arbitrary integers on the
stack by using `N` along with the digit commands introduced in this
section.

The following code pushes the integer 0 to the data stack:

```
N
```

The `N` command stands for *nil* which pushes the nil value (the zero
value) to the data stack.

A digit command like `1`, `2`, etc. multiplies the value at the top of
the data stack with 10 and adds the integer value of the digit to it.
For example, consider the following code:

```
N125
```

There are 4 distinct commands in this code: `N`, `1`, `2`, and `5`.
The evaluation of these commands occur as follows:

 1. `N` pushes the value 0 to the data stack.
 2. `1` multiplies the value 0 at the top of the data stack with 10
    and then adds 1 to it to get 1.  Thus the value at the top of the
    stack is now 1.
 3. `2` multiplies the value 1 at the top of the data stack with 10
    and then adds 2 to it to get 12.
 4. `5` multiplies the value 12 at the top of the data stack with 10
    and then adds 5 to it to get 125.

Thus at the end of the evaluation, the value at the top of the data
stack is 125.

The command sequence consisting of `N` followed by some digits is an
idiom for composing an integer on the data stack.  For example, the
following code places the three integers 147, 112, and 219 to the
stack.

```
N147N112N219
```

The data stack looks like [147, 112, 219] at the end of the evaluation
and each cell of the canvas gets painted with the RGB colour (147,
112, 219) thereby making the entire canvas look medium purple.


Arithmetic
----------

The commands `+`, `-`, `*`, `/`, and `%` perform arithmetic
operations.  Each command pops two values from the data stack,
performs the arithmetic operation, and pushes the result back to the
data stack.

For example, the following code leaves the result 120 on the data
stack.

```
N70N50+
```

The command sequence `N70` leaves the integer 70 on the data stack.
Then the command sequence `N50` puts another integer 50 on the data
stack.  Finally, the command `+` replaces both integers with their
sum.

The commands `+`, `-`, `*`, `/`, and `%` perform the addition,
subtraction, multiplication, division, and modulus operations,
respectively.  These commands are explained with the following
examples:

| Example | Result  | Remarks                                    |
|---------|---------|--------------------------------------------|
| `XY+`   | X + Y   |                                            |
| `XY-`   | X - Y   |                                            |
| `XY*`   | X * Y   |                                            |
| `XY/`   | X / Y   | Fractional part is discarded               |
| `XY%`   | X mod Y | Result lies between 0 and Y - 1, inclusive |

Note that in case of subtraction, the value at the top of the data
stack is subtracted from the second value from the top of the data
stack.  Similarly, in case of division and modulo, the value at the
top of the data stack divides the second value from the top of the
data stack.

Also note that all arithmetic operations result in integers.  Thus any
fractional part is discarded from the result.  For example, the
following code leaves the result 7 on the data stack:

```
N73N10/
```

The following code leaves the result -7 on the data stack:

```
NN73-N10/
```

The modulo command leaves a nonnegative remainder that is less than
the divisor.  For example, the following code leaves the remainder 2
(not -3) on the data stack:

```
NN8-N5%
```


Division by Zero
----------------

The FXYT evaluator has different modes for handling division by zero.
The default mode is mode 0 in which any division by zero is considered
an error.  When such an error occurs, the evaluator halts and the
entire canvas is painted red.

In mode 1, when division by zero occurs while evaluating the code for
a certain cell, the error is ignored and the cell is painted black.

In mode 2, when division by zero occurs while evaluating the code for
a certain cell, the error is ignored and the cell is painted red
instead.

The command `M` can be used to change modes.  Each `M` increments the
mode number.  It is an error to increment the mode number to 3.

The following code demonstrates the default behaviour which leads to
an error because a division by zero occurs at the coordinate (0, 0):

```
XY%
```

The following code demonstrates mode 1 where division by zero error is
ignored and the cells at which such errors occur are painted black.

```
MXY%
```

The following code demonstrates mode 2 where division by zero error is
ignored and the cells at which such errors occur are painted red.

```
MMXY%
```

In practice, `M` (mode 1) may be useful to get a nice output with
division by zero errors ignored.  The command sequence `MM` (mode 2)
on the other hand may be useful to conspicously display all cells
where division by zero error occurred.


Comparison
----------

The commands `=`, `<`, and `>` perform comparison operations.  Each
command pops two values from the data stack, compares them, and pushes
the result of the comparison back to the data stack.  The result of
each comparison command is explained in table below:

| Example | Result                  |
|---------|-------------------------|
| `XY=`   | 1 if X = Y, 0 otherwise |
| `XY<`   | 1 if X < Y, 0 otherwise |
| `XY>`   | 1 if X > Y, 0 otherwise |

For example, the following code leaves the value 1 on the data stack:

```
XX=
```

The following code leaves the value 0 on the data stack:

```
N1N2=
```

The following code draws a blue diagonal along the cells (x, y) where
x = y:

```
XY=N255*
```


Inversion
---------

The command `!` may be used to invert the result of comparison.
Precisely speaking, this command changes the integer at the top of the
stack according to these two simple rules:

- If the integer at the top of the data stack is 0, change it to 1.
- Otherwise change it to 0.

The following code paints all cells blue except the ones on the x = y
diagonal:

```
XY=!N255*
```


Bitwise Operations
------------------

The commands `^`, `&`, and `|` perform bitwise operations on integer
values.  Each command pops two values from the data stack, performs
bitwise operations on them, and pushes the result back to the data
stack.  The result of each command is explained in the table below:

| Example | Result          |
|---------|-----------------|
| `XY^`   | X bitwise-XOR Y |
| `XY&`   | X bitwise-AND Y |
| `XY\|`  | X bitwise-OR Y  |


Clip
----

The command `C` clips the value at the top of the data stack between 0
and 255.  This can be useful for ensuring that a value at the top of
the data stack is always a valid colour value.

Consider the following code:

```
XY+
```

This code produces an error at the cell (1, 255) where this code
evaluates to the result 266.  However, adding the command `C` as
follows resolves the error:

```
XY+C
```

Now whenever the result exceeds 255, the `C` command clips the result
to 255 thus leading to a valid value for the blue component of the
cell's colour.

Similarly, while the code `XY-` produces an error, the code `XY-C`
doees not.  Whenever the result is negative, the command `C` clips the
result to 0.


Duplicate
---------

The command `D` duplicates one item on the data stack.  It pushes a
copy of the value at the top of the data stack to the top.  For
example, consider the following code:

```
N1N2N3D
```

The resulting stack is [1, 2, 3, 3].


Pop
---

The command `P` pops one item off the top of the data stack.  The
popped value is dropped.  For example, consider the following code:

```
N1N2N3P
```

The resulting stack is [1, 2].


Swap
----

The command `S` swaps the two values at the top of the data stack.
For example, consider the following code:

```
N1N2N3S
```

The resulting stack is [1, 3, 2].


Rotate
------

The command `R` rotates the three values at the top of the data stack
such that the third value from the top of the data stack moves to the
top, the value at the top of the data stack moves to the second place
from the top, and the second value from the top of the data stack
moves to the third place from the top.  For example, consider the
following code:

```
N1N2N3N4N5R
```

The resulting stack is [1, 2, 4, 5, 3].


Loops
-----

Looping is supported with the commands `[` and `]`.  Each time `[` is
encountered, the evaluator picks a loop counter from the data stack
and remembers the code position after the `[` as the beginning of the
loop body.  It maintains this information in the loop control stack.
The loop control stack is not directly accessible to the programmer.
It is an internal implementation detail of the commands `[` and `]`.

To elaborate, the command `[` pops off one value from the data stack
and uses this value as the loop counter.  The command that comes after
the `[` is considered to be the beginning of the loop body.  If the
loop counter is a positive integer, the loop body is entered.
Otherwise, a corresponding `]` is found (nested `[` and `]` pairs are
skipped) and the evaluator skips ahead to the command after the
corresponding `]`.

The command `]` decrements the loop counter of the current loop.
After decrementing the loop counter, if its value is still positive,
then the evaluator jumps back to the beginning of the current loop's
body.  Otherwise, the evaluator exits the loop.  While exiting a loop,
the current loop's data is removed from the loop control stack.

Consider the following input code:

```
NN5[N10+]
```

At first, `N` pushes the integer 0 to the data stack.  We will refer
to this place of the data stack where 0 has been pushed as the
*initial place*.  The remainder of the code will increment the value
in this initial place.  The command sequence `N5` places the integer 5
on the stack.  Then `[` pops off 5 from the data stack, uses it as the
loop counter, and begins a loop.  Each iteration of the loop executes
`N10+` which adds the integer 10 to the value in the initial place.
Each time `]` is encountered, the loop counter is decremented by 1 and
if the loop counter is still 0, the evaluator jumps back to the
beginning of the loop body.  As a result, the loop containing `N10+`
is executed 5 times before the loop counter becomes 0 and the loop is
exited.  Thus the value at the initial place is incremented by 50.
When the evaluation completes, the value 50 is left on the data stack.

Here is an example of nested loops that leaves the value 200 on the
data stack.

```
NN5[N10[N4+]]
```


Frame Interval
--------------

The frame interval determines the time interval between two
consecutive iterations of the evaluator for time-dependent code.  The
default frame interval is 100 ms.  The frame interval may be changed
with the command `F`.  This command pops an integer value from the top
of the data stack.  It is an error if the popped value is negative.
If the popped value is nonnegative, it is considered to be a candidate
frame interval expressed in milliseconds.  The frame interval thus
obtained during the evaluation of the cell (0, 0) is set as the frame
interval between the current iteration and next iteration of
evaluation.

For example, the following code sets the frame interval to 1000 ms and
executes a time-dependent code:

```
N1000FXT^
```

Remember that the input code is evaluated for each cell.  It is
possible to write code such that the evaluation of the code for
different cells leads to different frame intervals.  As explained
earlier, it is the frame interval set in the evaluation of cell (0, 0)
that determines the frame interval for the next iteration of
evaluation.  For example, consider the following time-dependent code:

```
XY+N200+FT
```

In the above example, the command `F` pops an integer value 200 while
the code is evaluated at the cell (0, 0).  However it pops an integer
value 710 when the same code is evaluated at the cell (255, 255).  The
frame interval for the next iteration of evaluation is thus 200 ms
(not 710 ms).

The renderer for time-dependent code tries to schedule each iteration
of the evaluation in such a manner that the average frame interval
equals the set frame interval.  However, if the frame interval is set
to a very small value (say, less than 100 ms or so), the renderer may
fail to maintain the set frame interval on an average, especially,
when the time to render the canvas exceeds the chosen frame interval.


Print Data Stack
----------------

The command `W` provides a very minimal debugging facility.  It prints
the current coordinate followed by the data stack to the status panel
and halts evaluation.

Type the following code as the input:

```
XYW
```

The following data should appear in the status panel:

```
(0, 0) -> [0, 0]
```

During the evaluation of the code at coordinate (0, 0), the `W`
command prints the coordinate (0, 0) followed by the data stack [0, 0]
and halts the evaluation.

A more typical requirement may be to find out what the input code
evaluates to at a particular coordinate.  This can be accomplished by
combining comparison commands with looping commands.  Here is an
example that shows the result of evaluating `XY^` at the coordinate
(7, 9):

```
XY^XN7=YN9=&[W]
```

For each cell, first `XY^` is evaluated and the result is left on the
stack.  Then `XN7=` compares if x-coordinate value equals 7 and pushes
the result (either 1 or 0) to the data stack.  Similarly, `YN9=`
compares if the y-coordinate value equals 9 and pushes the result
(either 1 or 0) to the data stack.  Then `&` pops both the comparison
results and combines them with the bitwise AND operation and pushes
the result to the data stack.  Note that this command pushes 1 if both
comparisons earlier evaluated to 1, otherwise 0 is pushed to the
stack.  Thus the following loop body is entered if and only if x = 7
and y = 9.  As soon as the loop body is entered, `W` writes the
current coordinate (7, 9) and the result 14 (the bitwise XOR of 7 and
9 is 14) and halts the evaluation.


Idioms
------

The following idioms may be useful while writing FXYT code:

- To place an arbitrary positive integer on the data stack, write `N`
  followed by the decimal digits of the integer.  For example, `N105`
  places the integer 105 on the data stack.  The code `N105` may be
  informally read as the *number 105*.  See the section
  [Integers](#integers) to learn more about why this works.

- To place an arbitrary negative integer on the data stack, write `NN`
  followed by the digits of the integer followed by `-`.  For example,
  `NN105-` places the integer -105 on the data stack.  The code
  `NN105-` may be informally read as the *negative number 105*.

- To increment an integer at the top of the data stack, write `N1+`.
  Similarly, to decrement an integer at the top of the data stack,
  write `N1-`.

- To check if the top two values on the data stack are unequal, write
  `=!`.  This replaces the two values at the top of the data stack
  with 1 if they are unequal, 0 otherwise.  See section
  [Inversion](#inversion) for details.

- To execute a block of code conditionally, push 1 on the data stack
  if the block should be executed and 0 otherwise, and then write a
  loop with the code block in it.  For example, the code `YN10=[N255]`
  places the integer 255 on the data stack if the y-coordinate value
  equals 10.


Common Mistakes
---------------

- Forgetting to write `N` before entering a new integer is a common
  mistake.  For example, the code `X1+` does not push x-coordinate
  value and increment it by 1.  Instead it is an error.  Remember that
  `1` (a digit command) multiplies the existing number at the top of
  the data stack with 10 and then adds 1 to it.  Thus `X1` effectively
  replaces the integer x on the stack with 10x + 1.  The `+` command
  then fails to add two integers at the top of the data stack because
  the data stack contains only one integer.  The correct input code
  may be something like `XN1+` instead.  Always remember to write `N`
  while forming a new integer on the data stack.


Constraints
-----------

The reference implementation that comes with this project enforces the
following constraints:

- Whenever a command produces an integer result, the result must not
  not exceed 2147483647 and it must not be less than -2147483648.

- The input code must not exceed 1024 bytes in length.

- The number of commands executed to evaluate the colour for a cell
  must not exceed 1000.

If any of these contraints are violated during evaluation, the
evaluation halts with an error.


Distributable Links
-------------------

The reference implementation provides distributable links when the
input code is 64 bytes or less in length.  Note that the
implementation allows code up to a maximum length of 1024 bytes.
However, no distributable link is generated when the code length
exceeds 256 bytes.  Thus code that does not exceed 256 bytes in length
has a special status in the reference implementation.

The distributable link encodes the input code and appends it as a URL
fragment to the address of the current page.  Copy the URL with the
encoded input code embedded in it from the address bar of the web
browser in order to share it with others.  When the recipient of the
URL opens it with their web browser, the implementation reads the code
embedded in the URL, decodes it, and executes it.


FAQ
---

 1. Can you add feature X to this project?

    I have no intention of adding new features to this project.  The
    language defined and implemented by this project is intentionally
    minimal.  Inspired by esoteric programming languages with concise
    instruction sets, this tiny drawing language is meant to serve as
    a challenge for creating interesting visuals using a limited set
    of commands.


License
-------

This is free and open source software.  You can use, copy, modify,
merge, publish, distribute, sublicense, and/or sell copies of it,
under the terms of the MIT License.  See [LICENSE.md][L] for details.

This software is provided "AS IS", WITHOUT WARRANTY OF ANY KIND,
express or implied. See [LICENSE.md][L] for details.

[L]: LICENSE.md


Support
-------

To report bugs or ask questions, [create issues][ISSUES].

[ISSUES]: https://github.com/susam/fxyt/issues


See Also
--------

See [Andromeda Invaders](https://github.com/susam/invaders), a
1980s-arcade-style game written using HTML5, Canvas, and Web Audio.

See [CFRS[]](https://github.com/susam/cfrs), an extremely minimal
turtle graphics language with only 6 simple commands.

See [this demo page][demo.html] for FXYT community demos.


<!--
Release Checklist
-----------------

- Update version in package.json.
- Update version in HTML (1 place).
- Update copyright in HTML (1 place).
- Update copyright in LICENSE.md.
- Disable logging.
- Update CHANGES.md.
- Run: the following commands:

  npm run lint
  git status
  git add -p

  VER=<VER>
  git commit -em "Set version to $VER"
  git tag $VER -m "FXYT $VER"
  git push origin main $VER

  git remote add cb https://codeberg.org/susam/fxyt.git
  git push cb --all
  git push cb --tags
  git push cb main:pages
-->
