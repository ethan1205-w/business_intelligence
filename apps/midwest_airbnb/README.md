# ISA 401 Midwest Airbnb Chat

**Ask a question in plain English, get the SQL, a table, or a chart back**

A twelve-line [querychat](https://github.com/posit-dev/querychat) app built in ISA 401 (Miami University) on Airbnb listings across Chicago, Columbus, and the Twin Cities. It is the Assignment 05 deliverable, rebuilt from the Job Scout starter on the Airbnb data, deployed to [Render](https://render.com) from a GitHub repository, and extended with a `visualize` tool so it can hand back a chart.

**Live app:** https://midwest-airbnb-chat-uivj.onrender.com/

---

## What is this app?

The app connects to a SQLite database (`data/midwest_airbnb.db`), hands the `listings` table to querychat, and lets an LLM translate your question into SQL, a filtered table, or a chart. Every answer shows the query it ran, so you can check the logic and reuse the SQL yourself.

**Example queries:**
- "What's the median nightly price in each city?"
- "Plot average price by room type for Chicago."
- "Show superhost listings in Columbus with more than 50 reviews."

---

## Dataset Information

**Dataset:** `listings` table in `data/midwest_airbnb.db` (14,887 rows)
**Source:** Airbnb listings across Chicago, Columbus, and the Twin Cities
**Data dictionary:** `data/data_desc.md` (started in class; you complete it in Assignment 05)
**Query rules for the LLM:** `data/extra_instructions.md` (one starter rule; you add more)

### Key Fields

| Field | Description |
|-------|-------------|
| `city` | Metro area of the listing: Chicago, Columbus, or Twin Cities |
| `neighbourhood` | Neighborhood or sub-area as listed |
| `room_type` | `Entire home/apt`, `Private room`, `Shared room`, or `Hotel room` |
| `price` | Nightly price in USD |
| `number_of_reviews` | Total reviews received |
| `host_is_superhost` | `1` if the host holds superhost status, `0` otherwise |

---

## Required Secret

The app calls OpenAI (`gpt-5.6-luna (reasoning off)`) through [ellmer](https://ellmer.tidyverse.org/), so it needs one environment variable:

```bash
export OPENAI_API_KEY="your-api-key-here"
```

On Render, add it under your service's **Environment** tab as a secret named `OPENAI_API_KEY`. Never commit the key; `.Renviron` is listed in `.gitignore` for that reason.

---

## Running Locally

**With R (4.6.0, querychat 0.3.0):**
```r
# from inside apps/midwest_airbnb_chat/
shiny::runApp(".", port = 7860)
```

**With Docker:**
```bash
docker build -t midwest_airbnb_chat .
docker run --rm -p 7860:7860 -e OPENAI_API_KEY=$OPENAI_API_KEY midwest_airbnb_chat
```

Then open http://localhost:7860.

---

## Technology Stack

- **[Shiny](https://shiny.posit.co/)** - Web application framework for R
- **[querychat](https://github.com/posit-dev/querychat)** - Natural language data querying, filtering, and charting
- **[ellmer](https://ellmer.tidyverse.org/)** - LLM client for R
- **[RSQLite](https://rsqlite.r-dbi.org/)** - SQLite driver for R

---

## Course Information

This application was developed for **ISA 401** at **Miami University**. The polished version of the same idea, built on BLS wage data, is the [OEWS Jobs Explorer](https://huggingface.co/spaces/fmegahed/querychat_demo).
