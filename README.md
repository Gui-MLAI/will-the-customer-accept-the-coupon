# Will the Customer Accept the Coupon?

An exploratory look at who accepts in-car discount coupons and who ignores them, done in Python with pandas, Matplotlib, and Seaborn.

**Analysis notebook:** [coupon-acceptance-analysis.ipynb](coupon-acceptance-analysis.ipynb)

## The question

While driving, people are offered a coupon for a nearby bar, coffee house, restaurant, or takeaway spot. Some take it, some don't. Working from a UCI Machine Learning Repository survey of roughly 12,600 driving scenarios, this project looks at what tends to separate the people who accept from the people who pass.

## The data

Every row is a single driving scenario: the destination, time of day, weather, who's in the car, the driver's going-out habits, and whether they said yes to the coupon (`Y = 1` for accepted, `Y = 0` for rejected). The raw file lives in [`data/coupons.csv`](data/coupons.csv). The `car` column is almost entirely empty (99% missing), so it gets dropped. A handful of the visit-frequency columns are missing 1-2% of their values; those gaps are left in place and only drop out when a particular question filters on that column.

## How the notebook is organized

The notebook follows the assignment's structure and builds up in three passes:

1. **A general look** at the whole dataset, the overall acceptance rate, the mix of coupon types, and the spread of temperatures.
2. **A guided deep dive into the bar coupons**, slicing acceptance by how often people go to bars, their age, their passengers, and a few combined profiles.
3. **An independent investigation of the coffee house coupons**, the most common coupon, comparing acceptance by time of day, passenger, visit frequency, and expiration window.

Throughout, one idea does most of the work: since `Y` is coded 0/1, the mean of `Y` for any group is simply that group's acceptance rate. The plots are built with Seaborn, and several use its automatic confidence intervals as a quick read on how firm each rate is.

## What the analysis found

**Overall.** About 57% of all coupons are accepted, a slim majority, but that headline hides a lot of variation by coupon type and situation.

**Bar coupons.** These are the weakest performer at 41% overall. The dominant signal is habit: people who go to a bar more than three times a month accept 77% of the time, versus 37% for everyone who goes less often. The rate climbs further for frequent bar-goers who are over 25 with no kid in the car (around 71%). Bar coupons essentially work as a reminder to existing regulars, not as a way to win over people who rarely go out.

**Coffee house coupons.** These start near a coin flip at 50%, so context does the tipping. Regular coffee drinkers (one or more visits a month) accept around 65-69% of the time while people who never go sit at 19%. On top of that, mornings beat evenings, driving with a friend or partner beats driving alone, and a one-day expiration beats a two-hour one. The two strongest factors, frequency and timing, reinforce each other.

## Recommendations

- Lead with habit. Send bar and coffee coupons to people who already visit those places, and stop spending bar offers on people who never go.
- Push coffee coupons in the morning, and favor social driving contexts where possible.
- Give coffee coupons a one-day expiration rather than two hours.
- Rethink bar coupons aimed at a general audience, either by sweetening the offer or holding them for frequent visitors.

## Where it could go next

- Run the same breakdown on the coupon types this project didn't cover (cheap restaurants, carry-out, pricier restaurants).
- Fit a simple model such as logistic regression to measure how much each factor contributes once the others are accounted for.
- Test the expiration finding directly with a controlled experiment.

## Repository layout

```
.
├── coupon-acceptance-analysis.ipynb   # the analysis: code, plots, and write-up
├── data/
│   └── coupons.csv                    # source dataset
└── README.md                          # this file
```

## Running it locally

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install pandas matplotlib seaborn jupyter
jupyter notebook coupon-acceptance-analysis.ipynb
```
