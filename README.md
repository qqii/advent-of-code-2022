# Advent of Code 2022

## [Day 1](https://adventofcode.com/2022/day/1)

### [Part 1](https://adventofcode.com/2022/day/1#part1)

A quick excel job, but tables aren't well suited for referencing the previous cell. Thoughts:

- Use `LET` to start off with to keep things tidy
- `OFFSET([@ColumnName], -1, 0)` is handy to avoid referencing cell names


