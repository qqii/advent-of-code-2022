# [Day 3](https://adventofcode.com/2022/day/3)

I was considering using [SQLite](https://www.sqlite.org/index.html), but having looked at the problem I wasn't sure of the incarnations required, whereas I still had an idea in excel. 

As a learning point from the previous days, I have split the example and mapping tables into it's own sheet. I've also not been as strict with using `LET` everywhere, and only using it when things get complex.

## [Part 1](https://adventofcode.com/2022/day/3#part1)

When I saw this problem, I already knew that this would be a bit of wizardry to do in excel, but still I continued. I've made use of two lesser known features of excel, `LAMBDA` and [dynamic arrays](https://support.microsoft.com/en-us/office/dynamic-array-formulas-and-spilled-array-behavior-205c6b06-03ba-4151-89a1-87a7eb36e531) (`#SPILL`).

### Character to Priority

Sadly Excel's autofill feature really failed me, as it just didn't want to complete the alphabet. Seeing this I opted to code the function in reverse, mapping values to characters. 

I also took the opportunity to experiment with using `LAMBDA`, which I have practically avoided until now. [The intention](https://www.microsoft.com/en-us/research/podcast/advancing-excel-as-a-programming-language-with-andy-gordon-and-simon-peyton-jones/) seem to be using the name manager to define lambdas in the workbook or sheet scope, to use them in the same way as any excel function. This is a bit messy, especially as defining lambdas in the name manager has to be all done on a single line. 

Instead I have used `LET`, the "poor man's name manager" and defined the lambda inline. 

In retrospect the mapping might have been easier to do just by hand, and defining two lambdas using the name manager. All in all, excel doesn't really define let alone incentivise "good practices" with `LET` and `LAMBDA`.

### Finding Common Characters

To appreciate the madness of my solution, we must first take a foray into how dynamic arrays work.

#### Dynamic Arrays

[Dynamic arrays](https://support.microsoft.com/en-us/office/dynamic-array-formulas-and-spilled-array-behavior-205c6b06-03ba-4151-89a1-87a7eb36e531) are a lesser known (citation needed), strangely inconsistent but extremely powerful feature.

Many common excel function accept both values and ranges, such as `SUM`. You can call `SUM(A1, A2, A3)` or `SUM(A1:A3)`. Other still reasonably known functions, such as [`X`](../day02/README.md#part-1)`MATCH` are designed to use ranges. Lesser known functions such as `MUNIT` (matrix unit) and `RANDARRAY` can themselves generate a range, where their outputs spill over from one cell to another. [In previous versions](https://support.microsoft.com/en-us/office/dynamic-array-formulas-in-non-dynamic-aware-excel-696e164e-306b-4282-ae9d-aa88f5502fa2) this had to be triggered by pressing keying `Ctrl+Shift+Enter`, which adds `{}` around the formula.

As of recently (2022?) more and more excel functions have begun to accept dynamic ranges, such as `IF` and `+` (e.g. `A1:A5 + B1:B5`). Sadly this leaves the existing range based functions at a bit of an inconsistency - `AND(A1:A5, B1:B5)` reduces to a single value (as it did originally) instead of a spillover. The worst part is that dynamic arrays and tables do not play well, so you are forced to use the name manager instead.

### Splitting Text into Characters by Casting Incantations

In a normal language, splitting a _dynamic length_ string into characters to process them individually is simple. This is not so much in excel. I came up with two avenues to solve this problem:

- Use `SEQUENCE`, (aka `range` or `iota`) which can generate a dynamic array, map over the sequence to extract each character in the left half using `MID`. Then map over this to extract characters in the right half.
- Mapping over all of the possible characters, tally how many time it occurs. Then `AND` these arrays to be left with the singular character.

Both of these are quite involved, but I chose the 2nd one as it seemed simpler. There were three problems:

- Excel doesn't have many text manipulation functions, and nothing to count the number of occupance a character appears in some text
- `MATCH`, `XMATCH` and `=` are all case insensitive 
- `AND` is an old-style function that reduces any number of ranges to a single value.

The final solution is verbose, but hopefully still digestible. I can only hope I went with the right approach for part 2.

```Excel
=LET(
  COUNTC, LAMBDA(Char,String, LEN(String) - LEN(SUBSTITUTE(String, Char, ""))),
  HasLeft, COUNTC(Priority[Character], [@Left]) > 0,
  HasRight, COUNTC(Priority[Character], [@Right]) > 0,
  HasBoth, (HasLeft + HasRight) = 2,
  INDEX(Priority[Character], XMATCH(TRUE, HasBoth))
)
```

## [Part 2](https://adventofcode.com/2022/day/3#part2)

### [Go home](../README.md)
