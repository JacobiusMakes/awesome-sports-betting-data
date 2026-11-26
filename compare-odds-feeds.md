# Compare two odds feeds with one fixture

Suppose a client wants to display first-half basketball totals from one bookmaker. A provider can list the right league, book and market on its website while the particular quote the client needs is missing or too old to display. Use a small trial to decide whether to expand the evaluation.

## Write down what the client will accept

In this fictional example, the client wants a pre-match row for Harbor at Valley in the Example Basketball League. The row must show North Book's **first-half total, over 82.5**, a decimal price and the time the source updated the quote. At collection, the quote must be at most 30 seconds old. The client accepts a CSV row for the prototype. Permission to display prices to customers is a separate requirement for launch.

For your own trial, agree on the period, outcome, line, odds format, maximum age and output before collecting a sample. Match the event by teams, competition and scheduled start. Store each provider's event ID separately.

## Compare what actually came back

Both fictional feeds returned a price for the same event and bookmaker at **18:00:00 UTC**, before tipoff. Their timestamps are on the same date and use UTC.

| Check | Feed A | Feed B |
| --- | --- | --- |
| Outcome returned | First half, over 82.5 at 1.91 | Full game, over 162.5 at 1.91 |
| Quote last updated at source | 17:59:40 | 17:59:50 |
| Age when collected | 20 seconds, within cutoff | 10 seconds, within cutoff |
| Usable row for the client | Yes, for this observation | No, the period and line differ |
| Customer display rights | Unverified | Unverified |

Equal prices alone would hide the mismatch. Keep A's row as a sample. For B, check the documented period filter and repeat the same observation while the market is open. A response with only a collection timestamp leaves source age **unknown**. A correctly matched quote last updated at 17:59:29 would be 31 seconds old and would fail this trial's cutoff.

## Put a bound on the next trial

A 15-minute window sampled once a minute, starting at minute 0 and ending before minute 15, gives 15 polls. If one poll takes two pages, budget 30 HTTP requests per feed, or 60 across two feeds. Retries and event discovery add requests. Use each provider's documented billing unit to calculate credits or cost. A per-game price needs a game count, even when you already know the request count.

## Copy this into the client brief

| Field | Fill in before making the decision |
| --- | --- |
| Required row | League, scheduled start in UTC, teams, bookmaker, market, period, outcome and line |
| Acceptance rule | Price format, source timestamp definition, maximum age and required output |
| Observation | Provider event ID, collection time, source time, matched fields and a redacted sample |
| Intended use | Internal prototype or customer display, with the applicable terms or permission |
| Decision | Pass, fail or unknown for each requirement, plus the next sample or document needed |

Keep the fixture and collection time alongside the result. One passing row supports a decision to test the next fixture. Broader coverage and freshness claims need observations across the events and times your client plans to serve.

The directory maintainer also runs ParlayAPI. Its optional [evaluation lab](https://parlay-api.com/tools/evaluation-lab) can help you work through a trial. The table above works on its own with any provider.
