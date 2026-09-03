# Sensitivity Analysis Results

Generated: 2026-07-23 19:12

## Weight Scenarios Tested

| Scenario | Performance | Media | Social | Events |
|---|:---:|:---:|:---:|:---:|
| Baseline | 40% | 30% | 20% | 10% |
| Performance Heavy | 50% | 25% | 15% | 10% |
| Media Heavy | 30% | 40% | 20% | 10% |
| Equal Weights | 25% | 25% | 25% | 25% |

## Rank Stability (weight scenarios)

| Sponsor | Min Rank | Max Rank | Range |
|---|:---:|:---:|:---:|
| Love's Travel Stops | 4 | 4 | 0 |
| McDonald's | 3 | 3 | 0 |
| FedEx | 1 | 2 | 1 |
| NAPA Auto Parts | 1 | 2 | 1 |

## Weight Impact (vs baseline)

| Scenario | Total rank changes | Max change | Sponsors affected |
|---|:---:|:---:|:---:|
| performance_heavy | 0 | 0 | 0 |
| media_heavy | 2 | 1 | 2 |
| equal_weights | 2 | 1 | 2 |

## Cost Sensitivity (efficiency rank under low/mid/high cost)

| Sponsor | Low | Mid | High | Range |
|---|:---:|:---:|:---:|:---:|
| NAPA Auto Parts | 4 | 4 | 4 | 0 |
| FedEx | 3 | 2 | 2 | 1 |
| McDonald's | 2 | 3 | 3 | 1 |
| Love's Travel Stops | 1 | 1 | 1 | 0 |

## Conclusion

**Model is ROBUST.** Visibility rankings are stable across weight configurations (each sponsor moves at most one position). Core conclusions do not depend on the exact weights.

## Recommendations

1. Use baseline weights (40/30/20/10) for primary analysis.
2. Report the sensitivity findings alongside the rankings for transparency.
3. Where visibility ranks are close (e.g. the top two sponsors), note that ordering can flip under reasonable weight changes.