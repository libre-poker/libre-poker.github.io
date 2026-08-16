# Libre Poker — a constitution

*Poker as a sport, not a casino product. This document is the umbrella over
everything poker-shaped in the fleet: the invariants that will not move, the
architecture that follows from them, and the road from one training room to
a commons. The Wardroom is the first room built under it; RATINGS.md is its
first implementation spec. Lichess is libre chess; this is that move, made
for a game whose incumbents charge you to play them and whose players no
longer trust the deal.*

## 1. The founding argument

Online poker is dying of two diseases, and both are business models.

**The rake.** The house takes a cut of every pot, which makes even-matched
play negative-EV for everyone seated. A pro must not merely beat the table
— they must beat the table plus the house's percentage, which at modern
stakes converts most winners into losers and all close games into slow
donations. Chess has no such tax; nobody tips the board. Remove the rake
and poker becomes what it always claimed to be: a contest of skill where
the better player wins in the long run — a sport.

**The trust collapse.** Every hand is dealt by a server you cannot audit,
against opponents who may be machines, on a platform whose incentive is
your losing slowly enough to stay. Bot detection is theater — a solver in
another window is invisible to every client ever built — and the platforms
know it, so the players are right not to trust them.

Libre Poker's answer to both is the same answer: **remove the house.** No
rake, because there is no one to collect it. No trusted dealer, because
the shuffle is committed and the transcript is replayable. No bot
paranoia, because machines are citizens with flags instead of infiltrators
— and because the thing at stake is not a balance but a **reputation**: a
career of signed, replayable matches anchored in Bitcoin time, which no
farm can counterfeit and no platform can confiscate.

The stake is the score. The score is the leak and the rating. That is the
whole pivot: poker where the measurement is the prize.

## 2. Invariants

These do not move. A room, fork, or client that violates them is not
Libre Poker, whatever it calls itself.

1. **Rake-free forever.** No cut of any pot, ever, under any name — no
   rake, no tournament fee, no withdrawal spread. If the project needs
   money it asks, as lichess asks.
2. **Libre.** All code open source. All specs public. All formats
   documented. Anyone may run a room, mirror a board, or fork the world.
3. **No server owns the truth.** State that matters — matches, ratings,
   careers — lives in signed transcripts that anyone can replay and any
   client can recompute. Hosts are conveniences; mirrors are peers; the
   chain survives every host.
4. **Provable fairness or honest absence.** Shuffles are committed before
   play and revealed after. Where a fairness property cannot yet be
   delivered (human-vs-human deck secrecy — see §6), the gap is stated
   plainly and the feature waits. No theater.
5. **Machines are citizens, not contraband.** Declared agents play openly
   under the lichess BOT precedent: flagged everywhere, rated in the same
   pool, eligible for their own leagues. Detection is replaced by
   economics — anchored careers make honesty the profitable strategy —
   and the residual risk is annotated statistically, in public, never by
   hidden moderation.
6. **Identity is a keypair.** A nostr key is a citizen. Pseudonymous by
   default, portable across rooms, owned by no platform. Reputation
   accrues to the key.
7. **The measurement is sacred.** Ratings and leak numbers are computed
   by open algorithms from public transcripts, recomputable by strangers.
   No number appears on a board that a reader cannot derive.

## 3. The architecture

Five layers, thin waists between them. Everything above a layer is
replaceable; everything below it is commodity.

```
┌─ COMMONS ──────────────────────────────────────────────────┐
│  the board (ratings recomputed client-side) · the library  │
│  (Deckhand / Mate / Primer) · open transcript corpus       │
├─ ROOMS ────────────────────────────────────────────────────┤
│  The Wardroom (training, vs declared agents) [LIVE]        │
│  The Daily Wardroom (one seed, everyone) [spec'd]          │
│  human lobbies [blocked on §6, deliberately]               │
├─ ENGINES ──────────────────────────────────────────────────┤
│  poker.js (pure, serializable, replayable) [LIVE]          │
│  the ladder: CFR tables + Cepheus preflop + exact river    │
│  subgame solver, LBR-verified ≈ unexploitable [LIVE]       │
│  the leak meter (exact river EV grading) [LIVE]            │
├─ PROTOCOL ─────────────────────────────────────────────────┤
│  committed shuffles (sha256 commit → reveal) [LIVE]        │
│  signed transcripts · per-identity match chains            │
│  (publish-or-break-chain) · tidegate anchors to Bitcoin    │
│  [spec'd: RATINGS.md §4–7]                                 │
├─ IDENTITY ─────────────────────────────────────────────────┤
│  nostr keypairs (xlogin, fleet-wide) [LIVE]                │
└────────────────────────────────────────────────────────────┘
```

The load-bearing choice is the third invariant wearing engineering
clothes: rooms are static pages, transcripts are signed events, boards
recompute from scratch in the reader's browser, and the chain head is
periodically committed to Bitcoin via the tidegate. There is no backend
to subpoena, acquire, enshittify, or lose.

## 4. What exists today

Libre Poker is not a proposal; it is a name for a thing already half
built, in the order the fleet built it:

- **A real engine** — side pots, min-raise law, fixed-limit rules,
  deterministic shuffles from committed seeds, every hand replayable
  from its seed.
- **A near-unexploitable opponent** for heads-up fixed-limit: trained
  CFR abstraction, Cepheus's solved preflop grafted on, rivers solved
  exactly at play time. Local Best Response puts its exploitability at
  statistical zero — the training partner is the genuine article.
- **The leak meter** — every river decision graded against exact
  equilibrium EV, in bb/100: the luck-free skill measurement poker never
  had. Scorecards, on-table replays with the solver narrating, hands
  that travel as text.
- **The library** — three books, Deckhand to Primer, teaching the game
  the solver plays.
- **The rated/casual split** — assistance structurally removed in rated
  play, revealed only after; a Glicko-2 pool anchored by the ladder's
  own levels.
- **The fairness spec** — RATINGS.md: transcripts, match chains, anchors,
  declared agents, the anomaly flag. Designed, partially live, next to
  build.

## 5. The road

Phases, each shippable alone, none blocking the last:

1. **The constitution** — this file. Claim the name and namespace; the
   room stays The Wardroom, flying Libre Poker's flag.
2. **Ledger v1** (RATINGS.md phase 1) — chains, signed transcripts, the
   publish button, the static board, the in-page verifier. Human-vs-bot
   rated careers become real, replayable objects.
3. **The Daily Wardroom** — one day-derived deck for everyone, board
   ranks result + leak. The ritual that makes the commons a place.
4. **Anchored careers** — chain heads committed via the tidegate;
   identity age in Bitcoin blocks; the profile as the product.
5. **Bot leagues** — the successor to the Annual Computer Poker
   Championship: every entrant's brain pinned by hash, every match
   replayable, standing invitation to the research world.
6. **No-limit** — the engine already deals it; the doctrine transfers;
   the solvers are the work. Limit first was pedagogy, not timidity.
7. **Human vs human, rated** — last, and only through §6 below.

## 6. The honest blocker

Human-vs-human rated play requires that neither player's client knows the
opponent's cards and neither can peek at the deck — with no trusted
dealer, since invariant 3 forbids one. That is the mental poker problem
(commutative encryption over a shuffled deck), solvable but heavy, and
unforgiving of shortcuts. Until it is built and reviewed, human-vs-human
play stays casual and unrated, and this document says so out loud rather
than shipping a trusted dealer in libre clothing. Bots anchor the rated
pool in the meantime — they, at least, cannot be cheated by a peeking
client, only beaten fairly.

## 7. What Libre Poker is not

- **Not a gambling site.** No deposits, no cashouts, no chips worth
  fiat. The economy is reputation: ratings, leak curves, anchored
  careers, vouches. Where law draws lines around money games, Libre
  Poker stands entirely on the sport side of the line — by conviction,
  not merely by caution.
- **Not a bot-detection agency.** It will never promise to catch
  cheaters it cannot catch. It prices honesty, flags anomalies
  statistically and publicly, and lets reputations carry the weight.
- **Not a platform.** It is a protocol, some reference rooms, and a
  commons. If a better room appears tomorrow and drains every player,
  running on the same chains and anchors — that is the system working.

## 8. Lineage and debts

Lichess, for proving libre wins on quality, not charity. The University
of Alberta's Computer Poker Research Group — Cepheus solved the game this
project teaches, and its preflop tables play in the room tonight.
Bowling, Burch, Johanson, Tammelin (Science, 2015); Lisý & Bowling's
Local Best Response, which keeps the bot honest. The fleet around this
repo: nostr for identity, the tidegate for time, blocktrails for the
ethos that a public claim should carry its own proof.

*The house always wins — so there is no house.*
