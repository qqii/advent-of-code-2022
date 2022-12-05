# [Day 4](https://adventofcode.com/2022/day/4)

Trying to use the [higher order functions I learn't about yesterday](../day03/README.md#part-2) to fit the formula into a single cell!

## [Part 1](https://adventofcode.com/2022/day/4#part1)

It's obvious that I'm still not familiar with how excel likes to deal with dynamics arrays, but this was the solution I came up with:

```
=LET(
  Input, Input[Input],
  FULLYCONTAINS, LAMBDA(input, LET(
    PerElf, TEXTSPLIT(input, ","),
    LeftElf, TEXTSPLIT(INDEX(PerElf, 1), "-") + 0,
    RightElf, TEXTSPLIT(INDEX(PerElf, 2), "-") + 0,
    WITHIN, LAMBDA(a,b, AND(MIN(b) <= MIN(a), MAX(a) <= MAX(b))),
    OR(WITHIN(LeftElf, RightElf), WITHIN(RightElf, LeftElf))
  )),
  SUM(MAP(Input, FULLYCONTAINS) + 0)
)
```

I was once again caught out from `TEXTSPLIT`'s output not being automatically converted to numbers, but I had even greater difficulty trying to map the second `TEXTSPLIT` over the first. No matter what I tried, the 2nd `TEXTSPLIT` would either only give me the first item in the range, or `#CALC`.  
I'm sure there's probably a way to do it, but I just don't know what it is so I resorted to using `INDEX` to partition to two elves.

## [Part 2](https://adventofcode.com/2022/day/4#part2)

A simple adaptation from part 1.

```
=LET(
  Input, Input[Input],
  HASOVERLAP, LAMBDA(Input, LET(
    PerElf, TEXTSPLIT(Input, ","),
    LeftElf, TEXTSPLIT(INDEX(PerElf, 1), "-") + 0,
    RightElf, TEXTSPLIT(INDEX(PerElf, 2), "-") + 0,
    MinMax, MIN(MAX(LeftElf), MAX(RightElf)),
    MaxMin, MAX(MIN(LeftElf), MIN(RightElf)),
    MaxMin <= MinMax
  )),
  SUM(MAP(Input, HASOVERLAP) + 0)
)
```
## Quick navigation

| Jump to...                     |
| ------------------------------ |
| [Previous](../day03/README.md) |
| [Next](../day05/README.md)     |
| [Home](../README.md)           |
