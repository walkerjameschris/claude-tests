# Claude Tests

A gamut of test run against Claude by Anthropic as study of GenAI programming
tools. This repository is primarily focused on writing code for *analytics*.
That is, we will be leveraging R, Python, and a bit of SAS to conduct these
tests.

Tests are stored as individual files in the `tests/` directory. Files in this
directory are ordered in the approximate level of difficulty.

## Contents

* `README.md`: Information about this repository and test results
* `tests/`
  * `01_translate_r_to_python.md`: A simple R to Python translation test
  * `02_translate_sas_to_r.md`: A simple SAS to R translation test
  * `03_fix_bugs_in_r.md`: A moderately complex bug-fix test in R
  * `04_fix_bugs_in_python.md`: A moderately complex bug-fix test in Python
  * `05_prompt_new_r_simple.md`: A moderately complex prompt-to-R test
  * `06_prompt_new_r_complex.md`: A difficult prompt-to-R test

## Findings

On the first R to Python translation test, Claude was able to successfully
translate R into Python with minimal effort. It dropped some of my quality of
life comments, but produced a largely ready to go drop in replacement for the
R script I wrote prior.
