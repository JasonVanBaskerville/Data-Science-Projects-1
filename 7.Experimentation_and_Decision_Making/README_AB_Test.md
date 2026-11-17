# A/B Test of Reminder Emails for the Data Science Admissions Quiz

This project analyzes applicants to the WQU data science program and tests whether **reminder emails help applicants who have not finished the admissions quiz complete it**. The data is stored in MongoDB (`wqu-abtest.ds-applicants`) and includes 5,025 applicants from 139 countries. About 74% have completed the quiz and 26% have not. The project has four connected stages.

## Hypotheses

- **H0:** Reminder emails are not related to quiz completion.
- **Ha:** Reminder emails increase the quiz completion rate.

## Project Overview

| Stage | Focus | Main Results |
|---|---|---|
| 1. Applicant Data Exploration (EDA) | Connect to MongoDB, use aggregations (`$group`, `$project`, `$dateDiff`), process data with pandas, and create Plotly charts (bar chart, choropleth map, and histogram). Analyze applicants' countries, ages, and education. | Nigeria has the most applicants (1,238), followed by Pakistan (568), India (511), Kenya (304), and Bangladesh (254). Ages range from 22 to 76. The most common education level is Bachelor's degree (2,643, about 53%), followed by Master's degree (862), high school (832), some college (612), and doctoral degree (76). |
| 2. Data Pipeline (ETL) | Find applicants who have not completed the quiz by a certain date (`find_by_date`), randomly assign them (`seed=42`) to a **control** group (no email) or **treatment** group (email), save the groups to MongoDB (`update_one` / `$set`), and export treatment group emails to CSV. These steps are handled by the `MongoRepository` class. | Daily testing works. For example, on May 2, 2022, there were 49 applicants: 24 in the control group and 25 in the treatment group. |
| 3. Experiment Planning and Analysis | Use power analysis (`GofChisquarePower`), estimate the experiment length with CLT, simulate 10 days (`Experiment`), then run a chi-square test and calculate the odds ratio. | An effect size of 0.2 requires **394 observations** (197 per group). There is about a 98% chance of collecting them in 10 days. Results: 16/217 in the treatment group completed the quiz (about 7.4%), compared with 12/218 in the control group (about 5.5%); **p = 0.427**, odds ratio 1.37. |
| 4. Interactive Dashboard (Dash) | Connect the `MongoRepository`, `GraphBuilder`, and `StatsBuilder` classes to a Dash app. It includes demographic charts (map, age, education), sliders for effect size and duration, and a button to run the experiment and show chi-square results. | The full process, from loading data to testing the hypothesis, can run on one web page. |

## Tools and Libraries

Python, pandas, NumPy, SciPy, statsmodels (`GofChisquarePower`, `Table2x2`), PyMongo, Matplotlib, Plotly, Dash, and `country_converter`.

## Quick Workflow

1. Explore applicant data (country, age, education, and quiz status).
2. Calculate the sample size with power analysis (effect size, alpha 0.05, power 0.8).
3. Estimate how many days are needed to collect the data, based on the average number of applicants who have not completed the quiz each day.
4. Randomly assign applicants to the control and treatment groups, then save them to the database.
5. Email the treatment group using the exported CSV, then collect quiz completion results.
6. Compare the groups using a contingency table, chi-square test, and odds ratio.

## Conclusion

- Applicants are usually young people with bachelor's degrees from developing countries, especially in Africa and South Asia.
- The planned sample size was reached (435 participants, more than the required 394).
- The treatment group had a slightly higher completion rate (about 1.9 percentage points higher), but the difference is **not statistically significant** (p > 0.05). There is not enough evidence that reminder emails increase quiz completion.

## Notes and Limitations

- Completion rates are very low in both groups (below 8%), so the experiment may not have enough power to detect an effect this small.
- The experiment data comes from a simulation (`Experiment`), so the results are not real-world results.
- In the dashboard, `display_results` runs the experiment twice each time it is clicked. This can make the chart and test use different data. Run the experiment once and use the same results for both.
- The `MongoClient(...)` default argument in `MongoRepository` is created when the class is defined. It is better to create it inside `__init__`.
- Applicant data includes personal information (name, email, and date of birth). The exported CSV should only include the needed columns (`email` and `tag`).

## How to Run

```bash
pip install pandas numpy scipy statsmodels pymongo matplotlib plotly dash country_converter names randomtimestamp
```

Run MongoDB locally (port 27017), then run the notebooks in order. Start the dashboard with `app.run(debug=False)` and open `http://127.0.0.1:8050`.
