# Heikin-Ashi

Created by Munehisa Homma, [Heikin-Ashi](https://en.wikipedia.org/wiki/Candlestick_chart#Heikin-Ashi_candlesticks) is a modified candlestick pattern that uses prior day for smoothing.
[[Discuss] :speech_balloon:](https://github.com/DaveSkender/Stock.Indicators/discussions/254 "Community discussion about this indicator")

![image](chart.png)

```csharp
// usage
IEnumerable<HeikinAshiResult> results =
  quotes.GetHeikinAshi();  
```

## Historical quotes requirements

You must have at least two periods of `quotes`; however, more is typically provided since this is a chartable candlestick pattern.

`quotes` is an `IEnumerable<TQuote>` collection of historical price quotes.  It should have a consistent frequency (day, hour, minute, etc).  See [the Guide](../../docs/GUIDE.md#historical-quotes) for more information.

## Response

```csharp
IEnumerable<HeikinAshiResult>
```

- This method returns a time series of all available indicator values for the `quotes` provided.
- It always returns the same number of elements as there are in the historical quotes.
- It does not return a single incremental indicator value.
- The first period will have `null` values since there's not enough data to calculate.

### HeikinAshiResult

| name | type | notes
| -- |-- |--
| `Date` | DateTime | Date
| `Open` | decimal | Modified open price
| `High` | decimal | Modified high price
| `Low` | decimal | Modified low price
| `Close` | decimal | Modified close price
| `Volume` | decimal | Volume (same as `quotes`)

### Utilities

- [.ConvertToQuotes()](../../docs/UTILITIES.md#convert-to-quotes)
- [.Find(lookupDate)](../../docs/UTILITIES.md#find-indicator-result-by-date)
- [.RemoveWarmupPeriods(qty)](../../docs/UTILITIES.md#remove-warmup-periods)

See [Utilities and Helpers](../../docs/UTILITIES.md#utilities-for-indicator-results) for more information.

## Example

```csharp
// fetch historical quotes from your feed (your method)
IEnumerable<Quote> quotes = GetHistoryFromFeed("MSFT");

// calculate
IEnumerable<HeikinAshiResult> results = quotes.GetHeikinAshi();
```
