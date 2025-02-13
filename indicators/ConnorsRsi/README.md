# ConnorsRSI

Created by Laurence Connors, the [ConnorsRSI](https://alvarezquanttrading.com/wp-content/uploads/2016/05/ConnorsRSIGuidebook.pdf) is a composite oscillator that incorporates RSI, winning/losing streaks, and percentile gain metrics on scale of 0 to 100.  See [analysis](https://alvarezquanttrading.com/blog/connorsrsi-analysis).
[[Discuss] :speech_balloon:](https://github.com/DaveSkender/Stock.Indicators/discussions/260 "Community discussion about this indicator")

![image](chart.png)

```csharp
// usage
IEnumerable<ConnorsRsiResult> results =
  quotes.GetConnorsRsi(rsiPeriods, streakPeriods, rankPeriods);  
```

## Parameters

| name | type | notes
| -- |-- |--
| `rsiPeriods` | int | Lookback period (`R`) for the close price RSI.  Must be greater than 1.  Default is 3.
| `streakPeriods` | int | Lookback period (`S`) for the streak RSI.  Must be greater than 1.  Default is 2.
| `rankPeriods` | int | Lookback period (`P`) for the Percentile Rank.  Must be greater than 1.  Default is 100.

### Historical quotes requirements

`N` is the greater of `R+100`, `S`, and `P+2`.  You must have at least `N` periods of `quotes`.  Since this uses a smoothing technique, we recommend you use at least `N+150` data points prior to the intended usage date for better precision.

`quotes` is an `IEnumerable<TQuote>` collection of historical price quotes.  It should have a consistent frequency (day, hour, minute, etc).  See [the Guide](../../docs/GUIDE.md#historical-quotes) for more information.

## Response

```csharp
IEnumerable<ConnorsRsiResult>
```

- This method returns a time series of all available indicator values for the `quotes` provided.
- It always returns the same number of elements as there are in the historical quotes.
- It does not return a single incremental indicator value.
- The first `MAX(R,S,P)-1` periods will have `null` values since there's not enough data to calculate.

:hourglass: **Convergence Warning**: The first `N` periods will have decreasing magnitude, convergence-related precision errors that can be as high as ~5% deviation in indicator values for earlier periods.

### ConnorsRsiResult

| name | type | notes
| -- |-- |--
| `Date` | DateTime | Date
| `RsiClose` | decimal | RSI(`R`) of the Close price.
| `RsiStreak` | decimal | RSI(`S`) of the Streak.
| `PercentRank` | decimal | Percentile rank of the period gain value.
| `ConnorsRsi` | decimal | ConnorsRSI

### Utilities

- [.Find(lookupDate)](../../docs/UTILITIES.md#find-indicator-result-by-date)
- [.RemoveWarmupPeriods()](../../docs/UTILITIES.md#remove-warmup-periods)
- [.RemoveWarmupPeriods(qty)](../../docs/UTILITIES.md#remove-warmup-periods)

See [Utilities and Helpers](../../docs/UTILITIES.md#utilities-for-indicator-results) for more information.

## Example

```csharp
// fetch historical quotes from your feed (your method)
IEnumerable<Quote> quotes = GetHistoryFromFeed("SPY");

// calculate ConnorsRsi(3,2.100)
IEnumerable<ConnorsRsiResult> results
  = quotes.GetConnorsRsi(3,2,100);
```
