# [Day 3](https://adventofcode.com/2022/day/3)

I was considering using [SQLite](https://www.sqlite.org/index.html), but having looked at the problem I wasn't sure of the incarnations required, whereas I still had an idea in excel. 

As a learning point from the previous days, I have split the example and mapping tables into it's own sheet. I've also not been as strict with using `LET` everywhere, and only using it when things get complex.

## [Part 1](https://adventofcode.com/2022/day/3#part1)

When I saw this problem, I already knew that this would be a bit of wizardry to do in excel, but still I continued. I've made use of two lesser known features of excel, `LAMBDA` and [dynamic arrays](https://support.microsoft.com/en-us/office/dynamic-array-formulas-and-spilled-array-behavior-205c6b06-03ba-4151-89a1-87a7eb36e531) (`#SPILL`).

### Character to Priority

Sadly Excel's autofill feature really failed me, as it just didn't want to complete the alphabet. Seeing this I opted to code the function in reverse, mapping values to characters. [The intention](https://www.microsoft.com/en-us/research/podcast/advancing-excel-as-a-programming-language-with-andy-gordon-and-simon-peyton-jones/) is to use the name manager to define lambdas global to a workbook or sheet, but the name manager editor does not use newlines so I just used `LET` instead. 

In retrospect the mapping might have been easier to do just by hand. All in all, excel doesn't really define let alone incentivise "good practices" with `LET` and `LAMBDA`.

### Finding Common Characters

This uses [Dynamic arrays](https://support.microsoft.com/en-us/office/dynamic-array-formulas-and-spilled-array-behavior-205c6b06-03ba-4151-89a1-87a7eb36e531), which are a bit of a pain as spillovers don't play very well with tables.
### Splitting Text into Characters

Dynamic lengths in excel can be achieved with `SEQUENCE`, but I opted to create a tally for all possible characters instead. What really got me was the fact that `MATCH`, `XMATCH` and `=` are all case insensitive, so I had to use `EXACT` instead. 

Counting the characters is a bit of a hack, and later I realise that I could have just used `FIND`.

```Excel
=LET(COUNTC, LAMBDA(Char,String, LEN(String) - LEN(SUBSTITUTE(String, Char, ""))), ...)
```

At this point I wasn't aware of `BYROW` or `BYCOL` so I used arithmetic to calculate the `AND`. 

## [Part 2](https://adventofcode.com/2022/day/3#part2)

Using `LET` and `LAMBDA` paid off as this was a trivial addition. I used `MOD` and `OFFSET` for the 3 rows instead of `WRAPROWS` as I had not discovered `BYROW` or `BYCOL`, and I was using a table.

Here I noticed the problem wording:

 > The Elf that did the packing failed to follow this rule for **exactly one item type per rucksack**.

It turns out that excel has [quite a few more array processing functions](https://support.microsoft.com/en-us/office/guidelines-and-examples-of-array-formulas-7d94a64e-3ff3-4686-9372-ecfd5caa57c7) and [higher order functions](https://insider.office.com/en-us/blog/new-lambda-functions-available-in-excel) than I thought, fleshing out some functions that I'd previously considered not that useful. As a brief list for personal reference:

- `BYROW` and `BYCOL`, maps a lambda in an axis which in combination
- `REDUCE`, `SCAN` and `MAP`, `fold`, `scan` and `map`
- `MAKEARRAY`, create a matrix by calling `foo(row, col)`
- `HSTACK` and `VSTACK`, puts arrays together
- `WRAPCOLS` and `WRAPROWS`, reshaping data
- `TOROW` and `TOCOL`, flattening and removing gaps in an array
- `TAKE` and `DROP`, `head` and `tail`
- `EXPAND`, padding with a defailt
- `ISOMITTED`, to define lambdas that do different things when given a different numbers of arguments

With these cool new features in my toolbox, I think I'm going to make an attempt at a 1 cell solution tomorrow.
- ### [Go home](../README.md)
