# Midterm 1: answers

GitHub username: Danchonaldo
Session code: MAPLE-66
Fingerprint: a0b570c4

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration

I used Claude (Anthropic's assistant) during the exam. I gave it the exam README and my GitHub username and session code. It ran make_my_data.py and a Python script (standard library only) that computed the recall metrics, Wilson intervals, annual costs, pipeline accuracies and the Part 3 numbers.  I read the results, checked them against the formulas in the README, and I take responsibility for every number and sentence below. I used no other tools and did not share answers with anyone.

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A | 0.567 | 0.750 | 0.817 | 0.660 |
| B | 0.300 | 0.567 | 0.650 | 0.441 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 | 0.750 | 0.628 | 0.842 |
| A, k=5 | 0.817 | 0.701 | 0.894 |
| B, k=3 | 0.567 | 0.441 | 0.684 |
| B, k=5 | 0.650 | 0.524 | 0.758 |

Reading: all four intervals overlap with their neighbours (A,3 vs B,3: 0.628 < 0.684; A,5 vs B,5: 0.701 < 0.758; B,3 vs B,5 overlap), so with 60 questions no candidate is shown to be better than another.

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 | 8086.47 |
| A, k=5 | 10974.31 |
| B, k=3 | 4821.75 |
| B, k=5 | 5533.12 |
