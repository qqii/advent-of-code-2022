# Advent of Code 2022

Likely bad code, please don't take.

## [Day 1](https://adventofcode.com/2022/day/1)

I enjoy excel. With the addition of `LET` and `LAMBDA`, excel formula is now the most commonly used functional programming language! I mainly chose to use excel here as it was too late to setup a dev environment.

To new multi line formula (although you have to format it yourself), extend the formula bar and use `ALT+Enter`.

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

## [Day 2](https://adventofcode.com/2022/day/2)

Still haven't setup an IDE yet, so the excel file continues. What's been nice about excel is that it's really forcing me to consider the data structures, although I'm sure array programming languages would do the same. 

### [Part 1](https://adventofcode.com/2022/day/2#part2)

The use of excel really forced me to `map` out the lookups, something that on first instinct I would have used an if statement for. 

`INDEX(<index list>, MATCH(<value>, <match list>, 0))` is a common technique for mapping from `<match list>` to `<index list>`. 
I personally find it much easier to read and understand than `VLOOKUP`, especially when paired with tables ranges. I was a little caught out by `MATCH`, who's default lookup type is less than. In future `XMATCH` is a more powerful alternative (at what cost? performance?) that defaults to exact match.

To keep formulas from getting too unwieldy I've practiced breaking things up into steps, only putting it together (with `LET`) later. This process is made easier  when using tables. At at the cost of verbosity this can be further  adding a `LET(<heading name>, <calculation>, <heading name>)`.
