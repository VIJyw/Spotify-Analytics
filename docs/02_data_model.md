
# Data Model Documentation
**Spotify Global Streaming Analysis**

## 📁 Data Source

| Property | Details |
|----------|---------|
| **Dataset** | 2024 Global Streaming Statistics (Kaggle) |
| **Format** | CSV |
| **Volume** | Comprehensive global streaming records across genres, regions, and timeframes |

---

## 🗄️ Data Structure

### Primary Table: `Streaming_Data`
The main fact table containing streaming metrics and attributes

#### Key Fields:

| Field Name | Type | Description |
|------------|------|-------------|
| `Genre` | Text | Music genre classification |
| `Track_Name` | Text | Individual song identifier |
| `Artist` | Text | Performing artist |
| `Total_Streams` | Numeric | Cumulative stream count |
| `Skip_Count` | Numeric | Number of times track was skipped |
| `Completed_Plays` | Numeric | Streams where user completed full track |
| `Replay_Count` | Numeric | Number of repeat listens by same users |
| `Region` | Text | Geographic market (e.g., USA, Germany, France, UK) |
| `Language_Market` | Text | Derived field - English vs. Non-English speaking regions |
| `Date_Period` | Date | Time dimension for temporal analysis |

---

## 🔄 Data Transformations

### 1. Data Cleaning

✅ **Null Value Handling**: Removed records with missing critical fields (Genre, Total_Streams)  
✅ **Outlier Treatment**: Validated extreme values in replay counts and skip rates  
✅ **Standardization**: Normalized genre naming conventions (e.g., "Hip-Hop" vs "Hip Hop")

---

### 2. Derived Calculations

> **Note**: The following metrics were calculated in Excel and imported to ensure accuracy

#### Skip Rate
```excel
Skip_Rate = (Skip_Count / Total_Streams) × 100
```

- **Measures**: Percentage of streams where user skipped track
- **Key indicator**: Content-fit and user satisfaction
- **Industry benchmark**: ~18-20% average

#### Completion Rate
```excel
Completion_Rate = (Completed_Plays / Total_Streams) × 100
```

- **Measures**: Inverse metric to Skip Rate
- **Shows**: Percentage of streams listened to completion
- **Higher values**: Indicate stronger content engagement

#### Replay Rate
```excel
Replay_Rate = Replay_Count / Unique_Listeners
```

- **Measures**: Average number of times users replay the same track
- **Strong indicator**: Content quality and user loyalty
- **Differentiates**: Viral one-hit content vs. lasting appeal

#### Efficiency Score
```excel
Efficiency_Score = (Replay_Rate × Completion_Rate) / Skip_Rate
```

- **Composite metric**: Balancing engagement quality
- **Higher scores**: Content generates loyalty with minimal waste
- **Used for**: Identifying high-value content for algorithm optimization

---

### 3. Categorical Groupings

#### Region Classification:
- **English-speaking**: USA, UK, Canada, Australia
- **Non-English Europe**: Germany, France, Italy, Spain
- **Purpose**: Test cultural-linguistic hypothesis in skip behavior

#### Genre Groupings:
- **Mainstream**: Pop, Hip Hop, Rock, EDM
- **Regional**: Reggaeton, K-Pop, Latin, Regional Mexican
- **Niche**: Jazz, Classical, Blues, Folk

#### Popularity Tiers:
- **High**: >100M total streams
- **Medium**: 10M-100M streams
- **Low**: <10M streams

---

## ✅ Data Quality Validation

### Validation Checks Performed:

1. ✅ `Total_Streams = Skip_Count + Completed_Plays` (no data leakage)
2. ✅ `Skip_Rate + Completion_Rate = 100%` (mathematical consistency)
3. ✅ Cross-verified calculations in Excel vs. Power BI DAX
4. ✅ Replay_Rate outliers investigated and confirmed valid (Reggaeton genre behavior)
5. ✅ Regional data completeness: all major markets represented

### Known Limitations:

⚠️ Dataset represents sample of platform, not full catalog  
⚠️ Replay counts may undercount mobile/offline plays  
⚠️ Genre classifications are platform-defined (some tracks could fit multiple categories)  
⚠️ No demographic data (age, gender) available for deeper segmentation

---

## 🔗 Relationships & Grain

**Fact Table Grain**: One row per track-region-date combination  
**Primary Key**: `Track_ID + Region + Date_Period` (composite)

### Conceptual Model:
```
[Streaming_Data] (Fact)
    ├── Genre (Attribute)
    ├── Region (Attribute)  
    ├── Date_Period (Time Dimension)
    └── Calculated Metrics (Skip_Rate, Replay_Rate, etc.)
```

> **Note**: Single-table model appropriate for this analysis scope. Future enhancements could separate into dimension tables (Genre_Dim, Region_Dim, Date_Dim) for scalability.

---

## 📊 Data Volume Summary

| Metric | Count |
|--------|-------|
| Total Genres Analyzed | 15+ major genres |
| Geographic Markets | 8+ countries/regions |
| Time Period | Full year 2024 |
| Total Records | 500 |
