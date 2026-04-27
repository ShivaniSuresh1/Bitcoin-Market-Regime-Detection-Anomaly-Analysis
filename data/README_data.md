# Data

## Dataset

**Name:** Bitcoin Historical Data (1-minute resolution)  
**Source:** [https://www.kaggle.com/datasets/mczielinski/bitcoin-historical-data](https://www.kaggle.com/datasets/mczielinski/bitcoin-historical-data)

## How the notebook loads the data

The main notebook fetches the dataset automatically using `kagglehub` — no manual download needed:

```python
import kagglehub
from kagglehub import KaggleDatasetAdapter

df = kagglehub.load_dataset(
    KaggleDatasetAdapter.PANDAS,
    "mczielinski/bitcoin-historical-data",
    "btcusd_1-min_data.csv",
)
```

You will need a Kaggle account and your API token configured. If you haven't done this before:

1. Go to [https://www.kaggle.com/settings](https://www.kaggle.com/settings) → **API** → **Create New Token**
2. This downloads a `kaggle.json` file
3. In Colab, upload it and run:
   ```python
   import os
   os.makedirs("/root/.kaggle", exist_ok=True)
   # Upload kaggle.json, then:
   os.rename("kaggle.json", "/root/.kaggle/kaggle.json")
   os.chmod("/root/.kaggle/kaggle.json", 0o600)
   ```

## Manual download (alternative)

If you prefer to download the file directly:

1. Visit the dataset page linked above
2. Click **Download** to get `bitcoin-historical-data.zip`
3. Extract and place `btcusd_1-min_data.csv` in this `data/` folder
4. Update the notebook's data loading cell to read from the local path:
   ```python
   df = pd.read_csv("data/btcusd_1-min_data.csv")
   ```

## File used

| File | Description |
|------|-------------|
| `btcusd_1-min_data.csv` | BTC/USD OHLCV data at 1-minute resolution, spanning multiple years |

## Note on file size

The raw CSV is large (~200MB+). It is not committed to this repository. Use one of the loading methods above to obtain it before running the notebook.
