## Input

For this test, I asked Claude to develop an R function which accepts a data
frame of model predictions and true responses. From there, we need to compute
RMSE, MAPE, and total sum of squares. I also asked it to use the tidyverse and
make the function dynamic with non-standard evaluation.

**Please write an R function which accepts a data frame as the first argument.**
**This function needs to consider actual and predicted values and find MAPE,**
**RMSE, and total sum of squares. Use the tidyverse, R namespaces, and the new**
**R pipe `|>`. Make it so we can pass column names using NSE with `{{}}`.**

## Response

Claude did create a function which achieved the expected task. However, it did
not fufill 100% of the requirements I asked of it. For example, the `rlang`
calls are not necessary for NSE to work. Likewise, it did not use namespaces
(`::`) when calling functions or the new R pipe `|>`. I suspect that Claude is
good at using well documented R features which have been around for some time
(like `%>%`) but is ignorant or less likely to use new features like `|>` or
the plain `{{}}` operator.

```r
library(tidyverse)
library(magrittr)

eval_model <- function(data, actual, predicted) {
  
  actual <- rlang::enquo(actual)
  predicted <- rlang::enquo(predicted)
  
  data %>% 
    mutate(
      error = {{actual}} - {{predicted}},
      mape = abs(error / {{actual}}) * 100,
      sq_error = error^2
    ) %>% 
    summarise(
      mape = mean(mape, na.rm = TRUE),
      rmse = sqrt(mean(sq_error, na.rm = TRUE)), 
      sst = sum(sq_error, na.rm = TRUE)
    )
}
```