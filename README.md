# Global Historical Climatology Network daily (GHCNd)
# Big Data Climate Analysis Exercise

A hands-on exercise for working with large-scale climate data from the Global Historical Climatology Network (GHCN). This project demonstrates data chunking techniques for processing datasets too large to fit in memory.

## 📊 Project Overview

This exercise uses actual raw data from NOAA's Global Historical Climatology Network to analyze long-term temperature trends at specific weather monitoring stations worldwide. The dataset is approximately 4GB compressed and expands to ~12GB in RAM, making it an ideal learning tool for big data processing techniques.

## 🎯 Learning Objectives

- Process datasets larger than available RAM using chunking
- Work with fixed-width and CSV formatted climate data
- Filter and aggregate large-scale time series data
- Visualize long-term climate trends
- Optimize memory usage during data processing

## 📁 Dataset Information

### Data Source
- **Source**: [Global Historical Climatology Network (GHCN)](https://www.dropbox.com/s/oq36w90hm9ltgvc/global_climate_data.zip?dl=0)
- **Size**: ~4GB compressed, ~12GB in RAM
- **Format**: CSV (converted from fixed-width format)
- **Content**: Daily maximum temperature readings from thousands of weather stations globally

### Files
- `ghcnd_daily.tar.gz` / `ghcnd_daily.csv` - Main daily weather data
- `ghcnd-stations.txt` - Weather station metadata (fixed-width format)
- `README.txt` - Data format specifications

## 🚀 Getting Started

### Prerequisites

```bash
python 3.7+
pandas
matplotlib (for visualization)
```

### Installation

1. Clone this repository
```bash
git clone https://github.com/yourusername/big-data-climate-exercise.git
cd big-data-climate-exercise
```

2. Download the dataset
```bash
# Download from the link provided above
# Place in the project directory
```

3. Decompress the data
```bash
tar -xzf ghcnd_daily.tar.gz
```

## 📝 Exercise Steps

### 1. Data Exploration
- Decompress and check file size
- Load initial sample (200 rows) to understand structure

### 2. Station Selection
- Open `ghcnd-stations.txt` (fixed-width format)
- Select 3 weather stations (GSN, HCN, or CRN flagged)
- Include station `USC00050848` for comparison

### 3. Chunked Data Processing
```python
import pandas as pd

# Read data in chunks to avoid memory overflow
chunks = pd.read_csv('ghcnd_daily.csv', chunksize=150_000)

# Filter for selected stations
selected_stations = {'USC00050848', 'STATION_ID_2', 'STATION_ID_3'}
for chunk in chunks:
    filtered = chunk[chunk['id'].isin(selected_stations)]
    # Process filtered data
```

### 4. Data Analysis
- Calculate earliest year with data for each station
- Compute average maximum temperature by station and month
- Handle missing values (-9999)

### 5. Visualization
- Generate time series plots for each station
- Plot monthly temperature trends over time

## 🔍 Key Techniques

### Memory Management
- **Chunking**: Process data in manageable blocks
- **Filtering**: Keep only relevant observations
- **Type optimization**: Convert strings to numeric types when possible

### Chunk Size Selection
- Start small (1,000-10,000 rows) for debugging
- Increase size (100,000-500,000 rows) for production
- Monitor RAM usage to find optimal size
- Balance: larger chunks = fewer iterations but higher memory usage

### Handling Missing Values
```python
# Replace -9999 with NaN
df.replace(-9999, pd.NA, inplace=True)

# Calculate means ignoring missing values
df.mean(axis='columns', skipna=True)
```

## 📈 Expected Results

After completing the exercises, you should have:

1. Filtered dataset containing only your selected stations
2. Summary statistics showing:
   - Earliest year of data collection per station
   - Average maximum temperatures by month
3. Visualizations showing temperature trends over time

### Example Output Format
```
Station ID: USC00050848
Earliest Year: 1893
Monthly Average Max Temperatures (°F/10):
  January:   72.58
  February:  83.60
  ...
```

## ⚠️ Common Pitfalls

1. **Memory Overflow**: Don't try to load entire dataset at once
2. **Column Indexing**: Python uses 0-based indexing (README uses 1-based)
3. **Missing Values**: Watch for -9999 values in temperature data
4. **Plot Artifacts**: Horizontal lines suggest incorrect data reshaping

## 🏆 Advanced Challenge

For additional practice, work with `ghcnd_daily_30gb.tar.gz`:
- Full GHCN dataset (~30GB)
- Includes all weather variables (temp, precipitation, etc.)
- Fixed-width format requiring custom parsing
- Processing time: ~2 hours with moderate optimization

## 📚 Resources

- [GHCN Daily Documentation](https://www.ncei.noaa.gov/products/land-based-station/global-historical-climatology-network-daily)
- [Pandas Chunking Guide](https://pandas.pydata.org/docs/user_guide/io.html#iterating-through-files-chunk-by-chunk)
- [Fixed-Width File Parsing](https://pandas.pydata.org/docs/reference/api/pandas.read_fwf.html)

## 🤝 Contributing

This is an educational exercise. Feel free to:
- Submit issues for clarifications
- Propose optimization techniques
- Share alternative approaches

## 📄 License

This exercise uses public domain climate data from NOAA. Code and documentation are provided for educational purposes.

## 🙏 Acknowledgments

- NOAA's National Centers for Environmental Information for GHCN data
- Original exercise design for data science education

---

**Note**: This exercise is designed for systems with 8-16GB RAM. Users with 32GB RAM may not experience memory constraints with this dataset size.
