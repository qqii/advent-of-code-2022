# [Day 5](https://adventofcode.com/2022/day/5)

Using pure excel formula was a mistake. If the goal was to just get the answer, preprocessing the text manually would have made this a lot easier. 

## [Part 1](https://adventofcode.com/2022/day/5#part1)

I gave up with a one cell formula, although it should be doable with `REDUCE`. You can see the result of this attempt in the `InitialState` column and `NextState` column, who's outputs used to take an actual array.

Unfortunately this proved to be incredibly annoying to work with, and when trying to fold the solution back into a single cell I had great difficulty in diagnosing any issues that arose. Thus I have opted to my previous style of using tables, although this was probably not too wise either.

A far more elegant solution would be to relax the constraints of tables and do the output in raw cells. This would also be a much nicer visualisation than my use of `#` to `CONCAT` state together. 

## [Part 2](https://adventofcode.com/2022/day/5#part2)

After all that the "CrateMover 9001" solution was easier than the "CrateMover 9000" since excel doesn't have a `REVERSE` function. For reference this was my implementation of `REVERSE`:

```
LAMBDA(str, CONCAT(MID(str, SEQUENCE(LEN(str),, LEN(str), -1), 1)))
```

In retrospect I relied on `REDUCE` a bit too much when trying to squeeze part 1 into a single cell, but alas I no longer want to excel anymore so it's going to stay as is.

## Success

I've done it. It's uh, not the prettiest thing but it works. `MAP`ping or `REDUCING` over excel arrays is pretty wonky - here I've had to `MAP`/`REDUCE` over a brand new `SEQUENCE` to or else it would `#CALC` out. Sometimes nesting too deep also causes problems, such as using the `REVERSE` lambda in the below doesn't work but defining it as another `LET` binding is fine.


```
=LET(
  Input, Input[Input],
  Pad, "#",
  Concat, CONCAT(Input & Pad),
  Split, TEXTSPLIT(LEFT(Concat, LEN(Concat) - 1), Pad & Pad),

  Stacks, TEXTSPLIT(INDEX(Split, 1), Pad),
  NumStacks, MAX(TEXTSPLIT(INDEX(Stacks, COUNTA(Stacks)), "   ") + 0),
  OnlyBoxes, INDEX(Stacks, SEQUENCE(COUNTA(Stacks) - 1)),
  StackN, LAMBDA(text,N, MID(text, IF(N=1, 2, N*4-2), 1)),
  StrN, LAMBDA(text,N, SUBSTITUTE(CONCAT(StackN(text, N)), " ", "")),
  StackStrs, MAP(
    SEQUENCE(NumStacks),
    LAMBDA(N, StrN(OnlyBoxes, N))
  ),

  Moves, TEXTSPLIT(INDEX(Split,2), Pad),
  ExtractMoves, LAMBDA(move, LET(
    SubA, SUBSTITUTE(move, "move ", ""),
    SubB, SUBSTITUTE(SubA, "from ", ""),
    SubC, SUBSTITUTE(SubB, "to ", ""),
    TEXTSPLIT(SubC, " ") + 0
  )),

  DoStep, LAMBDA(stacks,count,from,to, LET(
    MovedBlocks, LEFT(INDEX(stacks, from), count),
    MAP(
      SEQUENCE(COUNTA(stacks)),
      stacks,
      LAMBDA(i,stack,
        SWITCH(i,
          from, RIGHT(stack, LEN(stack) - count),
          to, MovedBlocks & stack,
          stack
        )
      )
    )
  )),
  RunSteps, REDUCE(
    StackStrs,
    SEQUENCE(COUNTA(Moves)),
    LAMBDA(acc,val, LET(
      Extracted, ExtractMoves(INDEX(Moves, val)),
      DoStep(acc, INDEX(Extracted, 1), INDEX(Extracted, 2), INDEX(Extracted, 3))
    ))
  ),

  CONCAT(LEFT(RunSteps))
)
```

I've also added "Visualisation" into the `vis` sheet just for fun.

https://user-images.githubusercontent.com/6891353/205775699-039e7315-b383-4f05-8068-0c789ee57cf4.mp4

## Quick navigation

| Jump to...                     |
| ------------------------------ |
| [Previous](../day04/README.md) |
| _Next (come back later...)_    |
| [Home](../README.md)           |
