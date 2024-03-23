Changelog
=========

0.3.0 (UNRELEASED)
------------------

### Fixed

- Ignore embedded code longer than 64 bytes.


0.2.0 (2023-12-24)
------------------

### Changed

- The frame interval set during the evaluation of the code at cell (0,
  0) determines the frame interval of the next iteration of
  evaluation.  The frame interval set during the evaluation of the
  code at other cells is ignored.
- Negative frame interval is considered an error.
- The README now clearly specifies that incrementing mode number to 3
  is an error.


0.1.0 (2023-12-20)
------------------

### Added

- Command `X` to push x-coordinate value to the data stack.
- Command `Y` to push y-coordiante value to the data stack.
- Command `T` to push t-coordinate value to the data stack.
- Command `[` to begin loop.
- Command `]` to close loop.
- Command `+` to add two numbers.
- Command `-` to subtract two numbers.
- Command `*` to multiply two numbers.
- Command `/` to divide two numbers.
- Command `%` to perform modulo operation.
- Command `=` to compare if two numbers are equal.
- Command `<` to compare if one number is less than the other.
- Command `>` to compare if one number is greater than the other.
- Command `!` to invert value.
- Command `^` for bitwise XOR operation.
- Command `&` for bitwise AND operation.
- Command `|` for bitwise OR operation.
- Command `C` to clip value.
- Command `D` to duplicate value.
- Command `P` to pop value.
- Command `S` to swap two values at the top of the data stack.
- Command `R` to rotate three values at the top of the data stack.
- Command `F` to set frame interval.
- Command `M` to change mode.
- Command `W` to print the data stack and halt.
- Command `N` to push 0 to the data stack.
- Commands `0`-`9` to append decimal digit to integer value.
- Text field to type commands.
- Buttons to insert commands.
- Button to clear input field.
- Button to delete one command.
- Button to toggle help.
- Button to pause and resume time.
- Button to move backward in time.
- Button to move forward in time.
- Embed code that does not exceed 64 characters in the URL.
- Decode code from the URL and evaluate.
