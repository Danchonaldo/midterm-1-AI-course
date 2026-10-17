# Midterm 1: answers

GitHub username: Danchonaldo
Session code: MAPLE-66
Fingerprint: a0b570c4

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration

I used one AI assistant, Claude (Anthropic), in a chat during the exam. I used no other AI tool.

What I gave it: the exam README and notes, my GitHub username and the session code, which my teacher told me in class.

What Claude did: it cloned the course repository and ran make_my_data.py to generate my data. It then wrote a Python script (standard library only) that computed recall@1/3/5, MRR@10, the Wilson intervals, the annual costs of the four retrieval candidates and of pipelines H and L, the pipeline accuracies, and the numbers for the five Part 3 questions. It also drafted the text of this file: the decision in 1d, the next measurement in 1e, the memo in 2b and the Why lines in Part 3, and it checked the word limits.

What I did myself: I chose to submit these answers, I read the results and the reasoning, and I checked the main numbers against the formulas in the README. I take responsibility for every number and sentence in this file.

I did not share my answers with other students and did not copy anyone's text.

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

### 1d. Decision (at most 150 words)

Chosen candidate: B, k=5

Budget is $6,800. Both A candidates exceed it ($8,086 and $10,974), so they are out whatever their recall. B,3 ($4,822) and B,5 ($5,533) fit. Their intervals overlap (0.567 [0.441, 0.684] vs 0.650 [0.524, 0.758]), so I do not claim B,5 is better. I choose it because its point estimate is higher, it stays $1,267 under budget, and the extra cost is only $711 a year. Recall of B,5 by language: EN 0.70 (14/20), RU 0.55 (11/20), KK 0.70 (14/20). Russian speakers, 37 % of traffic, are served worst: about 45 % of their questions miss the right chunk. Only about two thirds of all questions get the right context, so I would add a fallback ("I am not sure, please contact an agent") when retrieval is weak.

### 1e. Next measurement (at most 60 words)

Test at least 200 real bank questions per language, not 20, on B,5 versus B,3 and on a cheaper A variant (k=3 with shorter paragraph chunks). Narrower intervals would show whether B,5 truly beats B,3, and whether Russian really lags. If it does, I would change the chunking for Russian.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H | 0.883 | KK, degraded | 0.800 | 0.627 | 0.905 | 30922.80 |
| L | 0.811 | KK, degraded | 0.667 | 0.488 | 0.808 | 5400 |

Personal-data pages per year sent abroad under H: 131400

### 2b. Memo (at most 300 words)

To: Head of Operations. Recommendation: pipeline L.

**Who is served worst.** Under both pipelines it is Kazakh invoices with degraded scans. L gets 20/30 = 66.7 % there (95 % interval 48.8 to 80.8 %), H gets 24/30 = 80.0 % (62.7 to 90.5 %). L's overall 81.1 % hides this cell, 14 points lower. The two intervals overlap, so with 30 invoices per cell I cannot say H is really better for Kazakh. Overall, H has 88.3 % against 81.1 %, with slightly overlapping intervals. I am fairly sure Kazakh degraded scans are the weak spot of both, but not sure of the gap's size.

**Personal data.** 9 % of 4,000 pages a day, 131,400 pages a year, carry a name and ИИН, which Law No. 94-V covers. Three questions. Under H: the scan goes to a data centre abroad (known); who stores it, and for how long, I do not know. I would have to find out from the vendor contract: retention, use for training, subprocessors. Under L: the scan stays on the bank's server in Kazakhstan (known); the bank stores it, and retention is ours to set.

**Safeguards anyone can verify.** (1) Firewall rule on the L server blocking outbound traffic; a test shows it. (2) Scans deleted after 30 days, with a deletion log checked monthly. (3) Access log for the server, reviewed quarterly. (4) Human review of every Kazakh degraded page and a monthly check of 100 random pages.

**Choice and remaining risk.** L costs $5,400 against $30,923 a year, keeps the ИИН at home, and loses about 7 points of overall accuracy. Remaining risk: wrong values on Kazakh degraded scans, covered only by the manual review, and server security falling on the bank.

## Part 3: Explain your own numbers

### Q1

Number: 2.57
Why (at most 40 words): Output tokens cost five times more than input tokens, and the output grows only 1.57 times from EN to KK while input grows 2.93 times, so the cost-weighted mix is different from the token ratio (2.84).

### Q2

Number: 414
Why (at most 40 words): The chunk is about 592 e5 tokens, but e5 reads only 512. The sentence ends before about 434, so it is inside the window and can match. The last 80 tokens are cut silently, with no error.

### Q3

Number: 71.8
Why (at most 40 words): Lower T sharpens the distribution (top token 58.6 % at T=1) but never changes the ranking. All five tokens keep nonzero probability, so the set that can be sampled is the same.

### Q4

Number: 31.6
Why (at most 40 words): I left out arithmetic and multiplication: they follow rules and can be learned, so the singleton-rate bound does not apply. A "never invent" prompt does not add knowledge; it can only turn errors into refusals.

### Q5

Number: 16.70
Why (at most 40 words): It has 334 tokens per parameter against Chinchilla's 20, so it is over-trained relative to the compute-optimal ratio. Chinchilla argued models like GPT-3 were too big and under-trained for their data.
