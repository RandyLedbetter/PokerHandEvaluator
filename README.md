# PH Evaluator

[![Build Status](https://travis-ci.com/HenryRLee/PokerHandEvaluator.svg?branch=master)](https://travis-ci.com/HenryRLee/PokerHandEvaluator)

A Poker Hand Evaluator based on a Pefect Hash Algorithm

## Overview

Efficiently evaluating a poker hand has been an interesting but challenging
problem. Given two different poker hands, how to determine which one is
stronger? Or more generally, given one poker hand, can we assign a score to
it indicating its strength?

Cactus Kev once gave [an answer](http://suffe.cool/poker/evaluator.html) for
a five card poker hand evaluation. With smart encoding, it ranks each hand
to 7462 distinct values.

Still, Kev's solution is specific for a five-card hand. To evaluate a
seven-card poker hand (which is more popular because of Texas Hold'em) using
Kev's algorithm, one brute force solution is to iterate all 7 choose 5
combination, running his five-card evaluation algorithm 21 times to find the
best answer, which is apparently too time-inefficient.

[PH Evaluator](https://github.com/HenryRLee/PokerHandEvaluator) is designed
for evaluating poker hands with more than 5 cards. Instead of traversing all
the combinations, it uses a perfect hash algorithm to get the hand strength
from a pre-computed hash table, which only costs very few CPU cycles and
considerably small memory (~100kb for the 7 card evaluation).

## Algorithm

This [documentation](Documentation/Algorithm.md) has the description of the
underlying algorithm.

## C/C++ Implementation

The [cpp](cpp) subdirectory has the C/C++ implementation of the algorithm,
offering evaluation from 5-card hands to 9-card hands.

The latest benchmark report generated by Google Benchmark:

```bash
2019-10-08 23:56:37
Running ./benchmark_phevaluator
Run on (2 X 2300 MHz CPU s)
CPU Caches:
  L1 Data 32K (x1)
  L1 Instruction 32K (x1)
  L2 Unified 256K (x1)
  L3 Unified 46080K (x1)
Load Average: 1.00, 1.01, 0.85
***WARNING*** Library was built as DEBUG. Timings may be affected.
--------------------------------------------------------------------
Benchmark                          Time             CPU   Iterations
--------------------------------------------------------------------
EvaluateAllFiveCardHands    64214505 ns     64213633 ns           11
EvaluateAllSixCardHands    544651414 ns    544637242 ns            1
EvaluateAllSevenCardHands 3760178653 ns   3760137811 ns            1
EvaluateAllEightCardHands 23088093903 ns   23087979165 ns            1
EvaluateAllNineCardHands  132522346551 ns   132511341434 ns            1
```

|   | Number of Hands | Time Used | Hands per Second | Memory Used |
|---|---|---|---|---|
| 5-card Hands | 2598960 | 64213633 ns | 40 M/s | 328K |
| 6-card Hands | 20358520 | 544637242 ns | 37 M/s | 328K |
| 7-card Hands | 133784560 | 3760137811 ns | 35 M/s | 328K |
| 8-card Hands | 752538150 | 23087979165 ns | 32 M/s | 328K |
| 9-card Hands | 3679075400 | 132511341434 ns | 27 M/s | 328K |

* Because the evaluators from 5-card to 9-card are sharing to the same library,
  so the memory used measure currently is the maximum of the 5 evaluators.
  This will be fixed in the future.

## Python Implementation

The [python](python) subdirectory has the latest python implementation, which is
still in active development. Contributions are welcome.

## Other Implementations

There is a [javascript implementation](https://github.com/thlorenz/phe) using
the same algorithm.


