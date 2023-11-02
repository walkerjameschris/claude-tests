## Input

For this test, I wrote the following Python and asked Claude to identify bugs
in the code. Specifically, I referenced functions which did not exist in Pandas
and referenced functions not yet imported.

**Please help me find bugs in this Python code. I am getting an error about a**
**function not defined. When you find the bugs, rewrite the code with**
**corrections and annotations.**

```python
#### Setup ####

# summary_stats.R
# Chris Walker

# Computes some summary statistics over some data

import pandas as pd

data = pd.dataframe(
  x = random.uniform(1000),
  y = random.normal(1000)
)

data.group_by("z").agg({
  "mean_x_y": lambda x: sum(x.x * x.y)
})
```

## Response

Claude produced the following output. It was successfully able to identify bugs
in this code by (a) finding the column which did not exist and (b) identifying
functions not yet imported to the session. Interestingly, it did call out that
*z* did not exist, but did not offer a solution like the R bug identification
test.

```python
#### Setup #### 

# summary_stats.py
# Chris Walker

# Computes some summary statistics over some data

import pandas as pd 
import random # Need to import random module

data = pd.DataFrame({ # DataFrame needs capital D
    "x": random.uniform(0, 1000, 1000), # Specify size of random data
    "y": random.normal(0, 1, 1000) # Specify mean and stddev
}) 

data.groupby("z").agg({ # 'z' column does not exist
    "mean_x_y": lambda x: (x.x * x.y).mean() # Use .mean() instead of sum
})
```
