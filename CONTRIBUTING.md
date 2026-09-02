# Contributing to EDGE LAB Research

EDGE LAB welcomes research clues, replication attempts, critiques and implementation help.

The project is intentionally skeptical. A good contribution does not need to claim a profitable strategy; a strong reason to reject a hypothesis can be equally valuable.

## Best way to contribute

### Research clue

Open a research-clue issue when you have a concrete observation from a trader, market maker, paper, podcast, X/Twitter post, YouTube video, protocol document, exchange behavior, market microstructure pattern or your own repeated observation.

Please answer:

1. **Source** — where did the clue come from? Link and quote only the minimum necessary context.
2. **Mechanism** — what market behavior could create the edge?
3. **Payer** — who or what is economically paying for it?
4. **Persistence** — why might it continue long enough to trade?
5. **Capacity** — why might the opportunity remain interesting at small capital even if large firms exist?
6. **Data** — what public data can test it without lookahead?
7. **Kill condition** — what result would make us stop quickly?
8. **Economic upper bound** — if the hypothesis is true, is the expected return large enough on total required capital?

A sentence such as “funding is usually positive” is too broad. A claim such as “after event X, participant Y is forced to do Z within horizon H, producing an executable price/funding distortion observable in data D” is testable.

## What we do not want

Please avoid:

- screenshots of profitable trades without reproducible rules;
- referral links or token promotion;
- guaranteed-return claims;
- secret/API-key requests;
- requests to run real-money trades;
- strategies justified only by an AI model’s confidence;
- post-hoc optimization presented as preregistered research;
- generic indicator recipes without a proposed economic mechanism.

## Replication and bug reports

If you find a calculation error, timestamp mismatch, lookahead path, fee omission, data-quality issue or unsupported claim, open a replication/bug issue.

A high-value report includes:

- exact file/result;
- expected behavior;
- observed behavior;
- minimal reproduction if possible;
- whether the issue can change the research verdict.

## Pull requests

For research-code PRs:

- keep the change focused;
- do not add credentials, private endpoints or paid-data secrets;
- preserve frozen assumptions unless the PR explicitly defines a new hypothesis;
- add deterministic tests for sign conventions, timestamps, cost accounting and candidate gates;
- separate diagnostics from pass/fail criteria;
- state whether the change can alter an already-published result.

## Publication ethics

Do not paste private communities, paid research, leaked material or copyrighted content into this repository. Link to the original public source and summarize the relevant mechanism in your own words.

If a practitioner clue may expose a currently exploitable vulnerability or create security risk, do not publish operational exploit details. Use a responsible disclosure path instead.

## Tone

Critique ideas, assumptions and evidence — not people.

A result marked `FAIL`, `COST_KILLED` or `BLOCKED_DATA` is not wasted work. Eliminating a false edge cheaply is one of the main outputs of EDGE LAB.