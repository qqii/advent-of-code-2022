# [Day 1](https://adventofcode.com/2022/day/1)

I enjoy excel. With the addition of `LET` and `LAMBDA` excel formula is now the most commonly used functional programming language! Disciplined used of [structured references via tables](https://support.microsoft.com/en-us/office/using-structured-references-with-excel-tables-f5ed2452-2337-4f71-bed3-c8ae6d2b276e), good variable/table heading naming can go a long way to ensure complex excel files are manageable and understandable. 

I mainly chose to use excel here as it was too late to setup a dev environment.

To new multi line formula (although you have to format it yourself), extend the formula bar and use `ALT+Enter`.

## [Part 1](https://adventofcode.com/2022/day/1#part1)

A quick excel job, but tables aren't well suited for referencing the previous cell. We cheat by calculating a running sum, resetting on blank values and just taking the max of that. This turns out to be an issue for part 2. 

Some excel thoughts:

- Use `LET` to start off with to keep things tidy
- `OFFSET([@ColumnName], -1, 0)` is handy to avoid referencing cell names

## [Part 2](https://adventofcode.com/2022/day/1#part2)

Since I was taking a running sum getting the Nth largest number doesn't work on the example input:

| Input | Running Sum | Top 3
| ----- | ----------- | -----------
| 1000  | 1000        |
| 2000  | 3000        |
| 3000  | 6000        |
|       | 0           |
| 4000  | 4000        |
|       | 0           |
| 5000  | 5000        |
| 6000  | 11000       |
|       | 0           |
| 7000  | 7000        | 3rd Largest
| 8000  | 15000       | 2nd Largest
| 9000  | 24000       | 1st Largest
|       | 0           |
| 10000 | 10000       |

Fixing this is another case of `OFFSET`, and some ugly boundary conditions. On an interesting note, the actual input doesn't have this issue at all!

More excel thoughts:

- If you're using `OFFSET`, excel is not likely not the right tool
- On the other hand using excel forces small steps, and easy debugging by presenting an explosion of the in between steps

## Quick navigation

| Jump to...                 |
| -------------------------- |
| _Previous (no more)_       |
| [Next](../day02/README.md) |
| [Home](../README.md)       |
