# Extra Instructions

Rules the LLM follows when it writes SQL for `listings`.

- `price` is the nightly price in U.S. dollars. When the user asks what something costs, use `price` and round money to whole dollars in the answer.
- `host_is_superhost` is the text values 't' and 'f', not booleans or 1/0 -- match with host_is_superhost = 't', never = 1 or = TRUE.
- `host_since` and `instant_bookable` are entirely NULL in this table. Never filter, sort, or aggregate on either column, and tell the user those two columns have no usable data if they ask about them.
- When averaging `review_scores_rating` or `reviews_per_month`, exclude NULL rows rather than treating them as 0 -- a NULL means the listing has no reviews yet, not a rating of zero.
- Match `city` case-insensitively and allow partial names for the Twin Cities (for example Minneapolis or St. Paul should resolve to city = 'Twin Cities').
- Search `name` case-insensitively with LIKE and LOWER(), since listing titles mix capitalization.
- There is no numeric bathrooms column -- `bathrooms_text` is free text (for example '2 baths', '1.5 baths', '1 shared bath'). To filter or sort by bathroom count, extract the leading number from `bathrooms_text` rather than comparing it as a number directly.
