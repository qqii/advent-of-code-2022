# [Day 5](https://adventofcode.com/2022/day/5)

Using pure excel formula was a mistake. If the goal was to just get the answer, preprocessing the text manually would have made this a lot easier. 

## [Part 1](https://adventofcode.com/2022/day/5#part1)

I gave up with a one cell formula, although it should be doable with `REDUCE`. You can see the result of this attempt in the `InitialState` column and `NextState` column, who's outputs used to take an actual array.

Unfortunately this proved to be incredibly annoying to work with, and when trying to fold the solution back into a single cell I had great difficulty in diagnosing any issues that arose. Thus I have opted to my previous style of using tables, although this was probably not too wise either.

A far more elegant solution would be to relax the constraints of tables and do the output in raw cells. This would also be a much nicer visualisation than my use of `#` to `CONCAT` state together. 

## [Part 2](https://adventofcode.com/2022/day/5#part2)

## Quick navigation

| Jump to...                     |
| ------------------------------ |
| [Previous](../day04/README.md) |
| _Next (come back later...)_    |
| [Home](../README.md)           |
