# Reddit Sentiment and Stock Returns

## Overview
This project examines whether retail investor sentiment expressed on Reddit is associated with short-term stock returns. Using posts from major finance-related subreddits, I extract ticker mentions, score sentiment using a lexical NLP model (VADER), aggregate sentiment at the weekly level, and merge it with weekly stock return data.

Rather than attempting to construct a trading strategy, the primary objective is to evaluate the informational content and limitations of social-media sentiment in financial contexts.

---

## Research Question
**Does sentiment expressed in Reddit investment discussions predict short-term stock returns, and how do linguistic and community norms affect the reliability of automated sentiment models?**

---

## Data Sources
- **Reddit posts:** r/wallstreetbets, r/investing, r/finance  
- **Sentiment scores:** VADER compound sentiment (–1 to +1)  
- **Financial data:** Weekly stock returns from Yahoo Finance (adjusted close)

Sentiment is aggregated at the **ticker–week level** and aligned temporally with weekly stock returns.

---

## Technical Implementation and Data Engineering

### Reddit Data Collection
Posts were collected programmatically using Reddit’s API via the `praw` library. To reduce noise and false attribution, analysis was restricted to a predefined universe of widely discussed tickers, including large-cap technology firms, frequently traded equities, and major ETFs.

Data were collected in **weekly windows over a 12-week horizon**, enabling construction of a panel suitable for regression analysis.

Filtering decisions included:
- Minimum post score threshold to exclude low-engagement content  
- Subreddit selection to capture heterogeneous investor norms  
- Strict time-window enforcement to prevent post–return leakage  

---

### Ticker Extraction
Ticker symbols were extracted using regular expressions designed to capture standalone capitalized tokens (e.g., `$AAPL`, `TSLA`). Candidate tickers were further filtered using:
- A hardcoded ticker universe  
- An exclusion list of common all-caps terms (e.g., “CEO,” “GDP,” “YOLO”)  

This approach balances recall and precision while minimizing false positives.

---

### Sentiment Scoring
Each post was scored using **VADER**, a rule-based lexical sentiment model designed for short, informal text. While computationally efficient and transparent, VADER does not account for sarcasm, humor, or community-specific language—limitations addressed explicitly through qualitative analysis.

---

### Aggregation and Returns Integration
Posts mentioning multiple tickers were expanded and aggregated to the ticker–week level, computing:
- Mean sentiment score  
- Post count per ticker–week  

Weekly stock returns were computed using adjusted closing prices from Yahoo Finance over the same weekly windows. Observations with missing or insufficient price data were dropped to ensure consistency.

---

## Quantitative Results (Summary)
Regression analysis and visualization show that Reddit sentiment has limited and unstable explanatory power for short-term stock returns. While isolated positive associations appear in some weeks, the overall relationship is weak and inconsistent across tickers and time.To better understand these results, I also conducted a qualitative analysis of selected posts from r/wallstreetbets and r/investing. Five recurring themes were identified,


## Tools
Python · pandas · praw · nltk (VADER) · yfinance · statsmodels · matplotlib

---

## Conclusion
This project demonstrates the importance of combining **technical execution with analytical judgment**. While Reddit sentiment offers intuitive appeal as a data source, its practical value depends heavily on understanding the linguistic and social contexts in which sentiment is expressed.
