# DAX Measures Documentation

> **Implementation Note**: Core calculations performed in Excel for validation accuracy. These DAX equivalents shown for reference and future scalability.

---

## 📊 Core Engagement Metrics

### Skip Rate (%)

**Purpose**: Percentage of streams where user skipped before completion  
**Business Context**: Primary indicator of content-audience mismatch. Higher skip rates indicate algorithmic over-promotion or poor content-market fit.
```dax
Skip Rate (%) = 
DIVIDE(
    SUM(Streaming_Data[Skip_Count]),
    SUM(Streaming_Data[Total_Streams]),
    0
) * 100
```

#### Interpretation Benchmarks:

| Range | Performance | Action |
|-------|-------------|--------|
| <15% | ⭐ Excellent fit | Content resonates strongly |
| 15-20% | ✅ Average | Typical platform performance |
| 20-25% | ⚠️ Below average | Consider reducing promotion |
| >25% | ❌ Poor fit | Investigate cause or remove from recommendations |

---

### Completion Rate (%)

**Purpose**: Percentage of streams listened to completion  
**Business Context**: Indicates content quality and user satisfaction. Strong leading indicator for replay behavior.
```dax
Completion Rate (%) = 
DIVIDE(
    SUM(Streaming_Data[Completed_Plays]),
    SUM(Streaming_Data[Total_Streams]),
    0
) * 100
```

**Validation Rule**: `Skip Rate + Completion Rate = 100%`

---

### Average Replay Rate

**Purpose**: Mean number of times users replay the same content  
**Business Context**: Strongest indicator of content value and user loyalty. High replay rates suggest content worth acquiring/promoting.
```dax
Avg Replay Rate = 
DIVIDE(
    SUM(Streaming_Data[Replay_Count]),
    DISTINCTCOUNT(Streaming_Data[Track_Name]),
    0
)
```

#### Benchmarks by Genre:

| Genre | Avg Replay Rate | Insight |
|-------|-----------------|---------|
| Reggaeton | 309.90 | 🏆 Exceptional loyalty |
| Pop | ~45-60 | 📊 Average, volume-driven |
| EDM | ~80-120 | 🎉 Event/mood-driven replays |

---

### Efficiency Score

**Purpose**: Composite metric measuring content ROI (engagement value vs. skip waste)  
**Business Context**: Identifies content that maximizes platform value - high engagement with minimal skip waste.
```dax
Efficiency Score = 
VAR ReplayRate = [Avg Replay Rate]
VAR CompletionRate = [Completion Rate (%)]
VAR SkipRate = [Skip Rate (%)]
RETURN
DIVIDE(
    ReplayRate * CompletionRate,
    SkipRate,
    0
)
```

**Usage**: Rank genres/tracks by Efficiency Score to prioritize promotion budget and algorithm weighting.

---

## 📈 Comparative Analysis Measures

### Skip Rate vs. Average

**Purpose**: Shows genre performance relative to platform average  
**Business Context**: Identifies over/under-performing content segments
```dax
Skip Rate vs Avg = 
VAR GenreSkipRate = [Skip Rate (%)]
VAR AvgSkipRate = 
    CALCULATE(
        [Skip Rate (%)],
        ALL(Streaming_Data[Genre])
    )
RETURN
GenreSkipRate - AvgSkipRate
```

**Interpretation**:
- ✅ **Negative values**: Genre performs better than average (lower skips)
- ❌ **Positive values**: Genre underperforms (higher skips)

---

### Regional Skip Rate Differential

**Purpose**: Compare skip behavior between English vs. Non-English markets  
**Business Context**: Tests cultural-linguistic hypothesis in content engagement
```dax
Regional Skip Differential = 
VAR EngSkipRate = 
    CALCULATE(
        [Skip Rate (%)],
        Streaming_Data[Language_Market] = "English"
    )
VAR NonEngSkipRate = 
    CALCULATE(
        [Skip Rate (%)],
        Streaming_Data[Language_Market] = "Non-English"
    )
RETURN
NonEngSkipRate - EngSkipRate
```

**Key Finding**: EDM and Hip Hop show **+15-20 percentage point differentials**, confirming cultural hypothesis.

---

## 🎯 Segmentation Measures

### Top Performing Genres (by Efficiency)

**Purpose**: Rank genres by overall value to platform
```dax
Genre Rank (Efficiency) = 
RANKX(
    ALL(Streaming_Data[Genre]),
    [Efficiency Score],
    ,
    DESC,
    DENSE
)
```

---

### High-Volume Low-Engagement Filter

**Purpose**: Identify content generating streams but poor user experience
```dax
High Volume Poor Engagement = 
VAR TotalStreams = SUM(Streaming_Data[Total_Streams])
VAR SkipRate = [Skip Rate (%)]
RETURN
IF(
    TotalStreams > 50000000 && SkipRate > 20,
    "Flag for Review",
    "OK"
)
```

---

## 💡 Calculation Strategy

### Why Excel-First Approach?

| Reason | Benefit |
|--------|---------|
| ✅ **DAX Complexity** | Measures can behave unexpectedly with complex filter contexts |
| ✅ **Validation** | Excel ensures mathematical accuracy before visualization |
| ✅ **Auditability** | Easier to troubleshoot calculation logic |
| ✅ **Consistency** | Power BI imports pre-calculated values for consistent reporting |

### Future DAX Enhancements:

- ⏱️ Time-based trending measures (MoM, QoQ engagement changes)
- 👥 User cohort analysis (if user-level data becomes available)
- 🔮 Predictive scoring for new content based on early engagement signals

---

## 📚 References

- [DAX Guide - DIVIDE Function](https://dax.guide/divide/)
- [Power BI Performance Best Practices](https://docs.microsoft.com/power-bi/guidance/)
