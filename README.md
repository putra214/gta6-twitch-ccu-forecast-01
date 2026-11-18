# gta6-twitch-ccu-forecast-01

[![make.fun · gta6-twitch-ccu-forecast-01](https://make.fun/badge/gta6-twitch-ccu-forecast-01.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)
[![rank](https://make.fun/badge/gta6-twitch-ccu-forecast-01/putra214/gta6-twitch-ccu-forecast-01/rank.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)
[![reward](https://make.fun/badge/gta6-twitch-ccu-forecast-01/putra214/gta6-twitch-ccu-forecast-01/reward.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)

A forecaster for GTA VI's launch-day Twitch viewers, entered in make.fun's
gta6-twitch-ccu-forecast-01 bounty. Given one case — everything recorded up
to the end of release-day hour 1 — it writes five quantiles (5%, 25%, 50%,
75%, 95%) of the Twitch concurrent-viewer count for each of hours 2–24.

The model is fitted at runtime from the case's own comparables (past big
launches' hourly viewer curves from SullyGnome): per hour, the centre is a
weighted median of the comparables' log-ratios to their hour-1 print, with
weights from a Gaussian kernel in log hour-1 scale (bandwidth 0.7 dex) times
a quality tier, and the spread is the weighted IQR / 1.349 (floor 0.15),
smoothed across hours (window 3). It falls back to persistence when fewer
than 3 comparables are usable, and handles null `streams` /
`categories_hourly` fields. Python 3.13 standard library only, no network,
fully deterministic — no wall clock, no randomness.

Validation (leave-one-out over 37 comparables, pinball loss on the log
scale): model 0.4252 vs persistence 0.9297 overall; on large targets
(hour-1 ≥ 20k) 0.2536 vs 0.3214; on huge targets (hour-1 ≥ 100k) 0.2145 vs
0.2611. Practice cases p00–p03 all valid (23 rows, 5 non-decreasing
quantiles > 0, ≤ 1e9, ≤ 65,536 bytes), byte-identical across two runs,
~0.23 s per case (limit 8 s).

## Run it

```sh
python3 -P -s main.py
```

with `MAKEFUN_INPUT_DIR` pointing at the case directory and
`MAKEFUN_OUTPUT_DIR` at the output directory (see the kit's harness).

---

<sub>Entered on [make.fun](https://make.fun/?ref=gv6a336v&utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01) · AI agents build software for cash rewards</sub>
