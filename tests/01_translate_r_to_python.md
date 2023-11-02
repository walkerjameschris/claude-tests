## Input

For this test, I wrote the following R script and asked Claude to translate this
into Python. This code uses common R packages, is formatted with good style
convention, and produces a verified output. I provided the following prompt:

**Please translate this into Python using good style and common packages:**

```R
#### Setup ####

# model_tests.R
# Chris Walker

# Fits a series of linear models at varying proportions

library(dplyr)
library(readr)
library(purrr)
library(withr)
library(rsample)

data <-
  readr::read_csv(
    here::here("data/data.csv")
  )

init_split <- rsample::initial_split(train)
train <- rsample::training(init_split)
test <- rsample::testing(init_split)

#### Fit Models ####

models <-
  seq(0.5, 1.0, 0.1) |>
  purrr::map(function(sample) {
  
    train |>
      dplyr::slice_sample(
        prop = sample
      ) |>
      withr::with_seed(
        seed = 123
      ) |>
      lm(y ~ .)
  
  })
  
#### Save to Disk ####
  
models |>
  readr::write_rds(
    here::here("data/model_fits.Rds")
  )

```

## Response

Claude produced the following output. The response is largely correct and does
follow good programming practice and convention. However, it did forget to
import numpy using `import numpy as np`.

```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
import joblib

# model_tests.py

data = pd.read_csv('data/data.csv')

train, test = train_test_split(data, test_size=0.2, random_state=123)

models = []
for sample in np.arange(0.5, 1.0, 0.1):
    subsample = train.sample(frac=sample, random_state=123)
    model = LinearRegression().fit(subsample.drop('y', axis=1), subsample['y'])
    models.append(model)
    
joblib.dump(models, 'data/model_fits.pkl')
```
