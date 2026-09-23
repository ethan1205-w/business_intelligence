# Midwest Airbnb Listings: Data Dictionary

**Dataset:** `listings` table in `midwest_airbnb.db` (SQLite), 14,887 rows and 29 columns
**Source:** Inside Airbnb (https://insideairbnb.com/get-the-data/), the detailed `listings.csv.gz` file for each of three regions: Chicago (snapshot 2026-07-20), Columbus (snapshot 2026-07-23), and Twin Cities MSA (snapshot 2026-07-21). Column meanings follow Inside Airbnb's data dictionary and assumptions (https://insideairbnb.com/data-assumptions/).
**Course:** ISA 401, Miami University

> One row is one listing that showed a nightly price on the snapshot date; listings with no price were dropped. Empty cells are stored as SQL `NULL`.

---

## Field Definitions

| Field | Type | Description |
|---|---|---|
| `city` | text | Which Inside Airbnb region the listing came from: `Chicago` (7,439 rows), `Columbus` (2,587), or `Twin Cities` (4,861). The Twin Cities file covers the Minneapolis-St. Paul metro area, not just the two cities. |
| `snapshot_date` | text | Date Inside Airbnb compiled the file, stored as an ISO text string, not a date: `2026-07-20` for Chicago, `2026-07-23` for Columbus, `2026-07-21` for Twin Cities. Every row of a city shares the same value. |
| `id` | text | Airbnb's listing id. Unique across the table (14,887 distinct values). Stored as text even though it looks numeric, so compare it to a quoted string. |
| `name` | text | Listing title as shown on Airbnb (for example "Tiny Studio Apartment 94 Walk Score"). Never empty. |
| `price` | real | Nightly price in U.S. dollars on the snapshot date, with the dollar sign and commas removed. Ranges from 2.56 to 11,412; never `NULL` (rows without a price were dropped). |
| `room_type` | text | Airbnb's four listing categories: `Entire home/apt` (11,652 rows), `Private room` (2,951), `Hotel room` (246), or `Shared room` (38). |
| `host_id` | text | Airbnb's host id. Stored as text even though it looks numeric, so compare it to a quoted string, like `id`. 6,970 distinct hosts across 14,887 listings; never `NULL`. |
| `host_name` | text | Host's display name. 25 `NULL` (0.2%). 3,327 distinct values; many are property-management brands rather than individuals (for example `Evolve`, `RoomPicks`, `Luxury Bookings Fze`), so one name does not always mean one person. |
| `host_since` | text | Date the host account was created, as an ISO string. **100% `NULL` in this table** — every row is empty. Do not filter, sort, or aggregate on this column; there is no usable data in it. |
| `host_is_superhost` | text | Superhost flag stored as the text values `t` or `f`, not a 1/0 boolean. 25 `NULL` (0.2%). 7,982 rows `t`, 6,880 rows `f`. |
| `neighbourhood` | text | Inside Airbnb's `neighbourhood_cleansed` column, geocoded against public shapefiles. Never `NULL`; 119 distinct values. Granularity differs by city: Chicago and Columbus use community-area or neighborhood names (for example `Near North Side`, `West Town`), while Twin Cities uses county names (`Hennepin`, `Ramsey`) since that file covers the whole metro area. |
| `latitude` | real | Listing latitude, WGS84 projection. Never `NULL`. Ranges from 39.8753494 to 46.24415. |
| `longitude` | real | Listing longitude, WGS84 projection. Never `NULL`. Ranges from -94.52678888 to -82.78095340. |
| `property_type` | text | Host-selected property type, more granular than `room_type`. Never `NULL`; 62 distinct values. Most common: `Entire rental unit` (5,581), `Entire home` (3,805), `Private room in home` (1,441), `Entire condo` (780), `Room in hotel` (557). |
| `accommodates` | integer | Maximum guest capacity of the listing. Never `NULL`. Ranges from 1 to 16; median 4. |
| `bedrooms` | real | Number of bedrooms. 2,976 `NULL` (20.0%) — the most incomplete numeric column; studios and some listings simply do not report it. Ranges from 1 to 16 (where reported); median 2. |
| `beds` | real | Number of beds. 668 `NULL` (4.5%). Ranges from 1 to 32 (where reported); median 2. |
| `bathrooms_text` | text | Bathroom count as Airbnb's free-text field; there is no separate numeric `bathrooms` column in this table. 71 `NULL` (0.5%); 32 distinct values, for example `1 bath`, `2 baths`, `1 shared bath`, `1 private bath`, `1.5 baths`. To filter or sort numerically, pull the leading number out of the text (for example with SQL string/regex functions). |
| `minimum_nights` | integer | Minimum required stay in nights (calendar rules may differ). 15 `NULL` (0.1%). Ranges from 1 to 365 (where reported); median 2. |
| `availability_365` | integer | Days the listing is bookable over the next year, per Inside Airbnb's calendar snapshot. Never `NULL`. Ranges from 0 to 365; median 274. |
| `number_of_reviews` | integer | Total number of reviews the listing has received, all-time. Never `NULL`. Ranges from 0 to 2,246; median 27. |
| `number_of_reviews_ltm` | integer | Number of reviews received in the last twelve months. Never `NULL`. Ranges from 0 to 1,220; median 9. |
| `first_review` | text | Date of the listing's earliest review, as an ISO string. 1,761 `NULL` (11.8%) — listings with zero reviews. The same 1,761 rows are `NULL` across every review-related column below. |
| `last_review` | text | Date of the listing's most recent review, as an ISO string. Same 1,761 `NULL` rows (11.8%) as `first_review`. |
| `review_scores_rating` | real | Overall guest rating on Airbnb's 1-to-5 scale (not 1-to-100). Same 1,761 `NULL` rows (11.8%) as `first_review`, representing listings with no reviews rather than a rating of zero. Ranges from 1.0 to 5.0 (where reported); median 4.89. |
| `reviews_per_month` | real | Average reviews received per month since the listing's first review. Same 1,761 `NULL` rows (11.8%) as `first_review`. Ranges from 0.01 to 77.72 (where reported); median 1.55. |
| `instant_bookable` | text | Flag for whether a guest can book without host approval, stored as the text values `t`/`f`. **100% `NULL` in this table** — every row is empty, the same issue as `host_since`. Do not filter, sort, or aggregate on this column. |
| `estimated_revenue_l365d` | real | Inside Airbnb's modeled estimate of the listing's revenue over the past 365 days (roughly `price` times estimated booked nights). This is a model output, not a confirmed transaction total, so describe it as an estimate when answering questions. Never `NULL`. Ranges from 0 to 1,114,800; median 15,750. |
| `amenities_count` | integer | **Not an Inside Airbnb column** — computed for this course as the number of items in each listing's `amenities` list. Never `NULL`. Ranges from 0 to 100; median 45. |
