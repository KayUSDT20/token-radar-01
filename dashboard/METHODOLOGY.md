# Token Radar — methodology (model radar-2)

## What the board is

A ranking of every protocol in the sealed snapshot that could plausibly launch a
token, by an implied value: what its token would be worth if the protocol's record
stayed as it is. It does not predict whether or when a token is issued.

## What counts as rankable

A protocol is ranked when it records no token (market cap, CoinGecko id and symbol
all null) and it is not an exchange, a bridge, a liquid staking or restaking
product, or a versioned pool of another protocol. That is the house watchlist's own
rule, applied to the whole snapshot instead of only the $50M floor. The watchlist's
40 protocols are all in; 162 pre-token protocols below the floor join them, so the
board holds 202 rows. Live-token protocols are kept apart and shown at market value.

## The model

```
implied = round( tvl_usd x base_rate
               x (1 + clamp(change_7d, -60, 120) / 100 x 0.5)
               x (0.6 + min(age_days, 1095) / 1095 x 0.4)
               x (1 + 0.03 x min(chains - 1, 5)) )
```

- **Base rate.** The aggregate market cap over aggregate TVL for the snapshot's
  live protocols with TVL at or above $100M, computed from the snapshot at run time
  (32 comparables, $88.6B of TVL, $6.16B of market cap, rate ≈ 0.070). It is never
  hardcoded. A per-category rate was tested and rejected: most categories have one
  or two live comparables, several of them tiny protocols whose tokens trade far
  beyond the TVL DefiLlama counts (one at 400x), so a per-category median would
  chase noise. The aggregate over large live protocols is dominated by the big
  names, the same scale as the board's top rows.
- **Growth.** The seven-day TVL change, clamped to −60%…+120% and weighted at 0.5.
  A protocol growing fast is worth more; a shrinking one less. Missing growth
  counts as 0.
- **Maturity.** Age since DefiLlama first listed the protocol, from 0.6 for a new
  protocol to 1.0 at three years. A protocol that has run for years with no token
  has a longer record and a larger user base behind a launch. When the snapshot
  records no listing date, the median age of the protocol's own category is used,
  then the median age of the whole board; the detail view says which.
- **Breadth.** One chain is the baseline; each further chain adds 3%, capped at
  five extra chains (1.15x). More chains means more distribution for a token.

No protocol name is special-cased: the same function scores every row.

## What the numbers mean, and what they do not

- "No token" means the snapshot records none, never that the protocol has none.
- The base rate uses circulating market cap, not fully diluted value. The snapshot
  carries no FDV, so the rate is conservative: a launch FDV would typically print
  higher on a low float.
- Implied value is an estimate from four snapshot figures. It is not a price, not
  a prediction of a launch, and not investment advice.

## Farm plans

Each row carries six steps: a fresh wallet, one category-specific action (the
action follows the protocol's category, chains, TVL and age, never its name), one
qualifying action a week, a hold of 30–60 days, a check of the protocol's own
points page before withdrawing, and a farm log. Each step states its cost and its
time. Nothing in a plan claims a points program exists: the steps say to check the
protocol's own page, and never to pay to claim.

## The biggest find

The highest-ranked protocol, Sentora Curator ($2.76B TVL), is not on the house
watchlist at all — the largest pre-token protocol the floor missed. The board exists
to find those: the watchlist is the floor, never the ceiling.
