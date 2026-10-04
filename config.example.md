# Flat Hunt — Config (example)

> Copy this into your scheduled task prompt, or save it as `config.md` (gitignored).
> Don't commit your real config — this fork is public.

```yaml
PROFILE: "Name, works in <field>, hybrid, no pets, non-smoker, references ready"   # enquiry drafts; leave "" to skip them
SKIP_UNFURNISHED: false     # true = drop flats listed only as Unfurnished

BEDROOMS: 2
MAX_RENT: 2000              # £ pcm
LATEST_MOVE_IN: 2026-12-01  # listings available after this are LOW
AVOID_ABOVE_SHOPS: true

PRIMARY_AREAS: [Ealing, Brentford, Kew]
SECONDARY_AREAS: [Isleworth, Putney]

# Only keep listings whose outward postcode is in this list.
# Zoopla pads results with "close matches" from neighbouring areas — this throws those out.
ALLOWED_POSTCODES: [W5, W13, TW8, TW9, TW7, SW15]

# Per-area location values for each site.
# Rightmove: REGION id — get it from https://los.rightmove.co.uk/typeahead?query=<area>
# Zoopla: URL slug, e.g. /to-rent/flats/<slug>/ — check the page heading names your area
# Use none to skip a site for an area.
AREAS:
  Ealing:    { rightmove: 87504, zoopla: ealing }
  Brentford: { rightmove: 206,   zoopla: brentford }

SEARCH_URLS:
  rightmove: "https://www.rightmove.co.uk/property-to-rent/find.html?locationIdentifier=REGION%5E{rightmove}&minBedrooms={BEDROOMS}&maxBedrooms={BEDROOMS}&maxPrice={MAX_RENT}&propertyTypes=flat&includeLetAgreed=false&sortType=6"
  zoopla:    "https://www.zoopla.co.uk/to-rent/flats/{zoopla}/?beds_min={BEDROOMS}&beds_max={BEDROOMS}&price_frequency=per_month&price_max={MAX_RENT}&results_sort=newest_listings"

SITES: [rightmove.co.uk, zoopla.co.uk]
MAX_LISTING_PAGES: 25

# Google Sheet to append to (must already exist, with a header row)
SHEET: "<spreadsheet id>"
SHEET_TAB: "Rentals"
NEW_STATUS: "New (hunt)"
COLUMNS: [Property link, Area, Monthly rent, Furnishing, Available from, Bedrooms, Status, Next action]  # your header row, in order
```

## Notes

- Not searched, deliberately: OnTheMarket (its robots.txt disallows automated access to search pages) and OpenRent
  (it serves a human-verification page to automated browsing). Use their own email alerts instead.
- Rightmove `sortType=6` = newest listed.
- To stop the hunt, pause or delete the scheduled task.
