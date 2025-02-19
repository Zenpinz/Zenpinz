- 👋 Hi, I’m @Zenpinz
- 👀 I’m interested in coding and programming
- 🌱 I’m currently learning machine learning using python



<!---
Zenpinz/Zenpinz is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->


import pandas as pd
import numpy as np

# Creating the DataFrame
good = pd.DataFrame({'data': [701, 703, np.nan, 706, np.nan, np.nan, 709]})

# Find indices of NaN values
nan_groups = good['data'].isna().astype(int).groupby(good['data'].notna().cumsum()).cumsum()

# Iterate through NaN sequences and fill them
for _, group in nan_groups[nan_groups > 0].groupby(good['data'].notna().cumsum()):
    nan_indices = group.index
    
    prev_idx = nan_indices[0] - 1  # Previous non-NaN index
    next_idx = nan_indices[-1] + 1  # Next non-NaN index

    if prev_idx >= 0 and next_idx < len(good):
        prev_val = good.loc[prev_idx, 'data']
        next_val = good.loc[next_idx, 'data']
        
        step = (next_val - prev_val) / (len(nan_indices) + 1)  # Calculate step size
        
        # Fill NaNs with incremental values
        for i, idx in enumerate(nan_indices, 1):
            good.loc[idx, 'data'] = prev_val + step * i

print(good)
