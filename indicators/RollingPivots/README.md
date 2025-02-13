# Rolling Pivot Points

Created by Dave Skender, Rolling Pivot Points is a modern update to traditional fixed calendar window [Pivot Points](../PivotPoints/README.md#content).  It depicts support and resistance levels, based on a defined _rolling_ window and offset.
[[Discuss] :speech_balloon:](https://github.com/DaveSkender/Stock.Indicators/discussions/274 "Community discussion about this indicator")

![image](chart.png)

```csharp
// usage
IEnumerable<RollingPivotsResult> results = 
  quotes.GetRollingPivots(lookbackPeriods, offsetPeriods, pointType);  
```

## Parameters

| name | type | notes
| -- |-- |--
| `windowPeriods` | int | Number of periods (`W`) in the evaluation window.  Must be greater than 0 to calculate; but is typically specified in the 5-20 range.
| `offsetPeriods` | int | Number of periods (`F`) to offset the window from the current period.  Must be greater than or equal to 0 and is typically less than or equal to `W`.
| `pointType` | PivotPointType | Type of Pivot Point.  Default is `PivotPointType.Standard`

For example, a window of 8 with an offset of 4 would evaluate quotes like: `W W W W W W W W F F  F F C`, where `W` is the window included in the Pivot Point calculation, and `F` is the distance from the current evaluation position `C`.  A `quotes` with daily bars using `W/F` values of `20/10` would most closely match the `month` variant of the traditional [Pivot Points](../PivotPoints/README.md#content) indicator.

### Historical quotes requirements

You must have at least `W+F` periods of `quotes`.

`quotes` is an `IEnumerable<TQuote>` collection of historical price quotes.  It should have a consistent frequency (day, hour, minute, etc).  See [the Guide](../../docs/GUIDE.md#historical-quotes) for more information.

### PivotPointType options

| type | description
|-- |--
| `PivotPointType.Standard` | Floor Trading (default)
| `PivotPointType.Camarilla` | Camarilla
| `PivotPointType.Demark` | Demark
| `PivotPointType.Fibonacci` | Fibonacci
| `PivotPointType.Woodie` | Woodie

## Response

```csharp
IEnumerable<RollingPivotsResult>
```

- This method returns a time series of all available indicator values for the `quotes` provided.
- It always returns the same number of elements as there are in the historical quotes.
- It does not return a single incremental indicator value.
- The first `W+F-1` periods will have `null` values since there's not enough data to calculate.

### RollingPivotsResult

| name | type | notes
| -- |-- |--
| `Date` | DateTime | Date
| `R3` | decimal | Resistance level 3
| `R2` | decimal | Resistance level 2
| `R1` | decimal | Resistance level 1
| `PP` | decimal | Pivot Point
| `S1` | decimal | Support level 1
| `S2` | decimal | Support level 2
| `S3` | decimal | Support level 3

### Utilities

- [.Find(lookupDate)](../../docs/UTILITIES.md#find-indicator-result-by-date)
- [.RemoveWarmupPeriods()](../../docs/UTILITIES.md#remove-warmup-periods)
- [.RemoveWarmupPeriods(qty)](../../docs/UTILITIES.md#remove-warmup-periods)

See [Utilities and Helpers](../../docs/UTILITIES.md#utilities-for-indicator-results) for more information.

## Example

```csharp
// fetch historical quotes from your feed (your method)
IEnumerable<Quote> quotes = GetHistoryFromFeed("SPY");

// calculate Woodie-style 14 period Rolling Pivot Points
IEnumerable<RollingPivotsResult> results
  = quotes.GetRollingPivots(14,0,PivotPointType.Woodie);
```
