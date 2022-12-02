# Advent of Code 2022

Likely bad code, please don't take.

## [Day 1](https://adventofcode.com/2022/day/1)

I enjoy excel. With the addition of `LET` and `LAMBDA`, excel formula is now the most commonly used functional programming language! I mainly chose to use excel here as it was too late to setup a dev environment.

### [Part 1](https://adventofcode.com/2022/day/1#part1)

A quick excel job, but tables aren't well suited for referencing the previous cell. We cheat by calculating a running sum, resetting on blank values and just taking the max of that. This turns out to be an issue for part 2. 

Some excel thoughts:

- Use `LET` to start off with to keep things tidy
- `OFFSET([@ColumnName], -1, 0)` is handy to avoid referencing cell names

### [Part 2](https://adventofcode.com/2022/day/1#part2)

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
