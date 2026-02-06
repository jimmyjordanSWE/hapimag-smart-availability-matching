# Data Sources Strategy

## Goal

Build Smart Availability Matching without using Hapimag internal data.

## Primary dataset (required)

Use synthetic but realistic operational data:

- `resorts` (location, attributes, room types)
- `inventory_calendar` (availability by date and room type)
- `pricing_or_points` (cost signals)
- `waitlist_history` (release rates and delays)
- `member_preferences` (synthetic profiles and flexibility)

This is the source of truth for MVP.

## External datasets (optional)

Use open data only to stress-test scenarios and assumptions:

- Hotel Booking Demand dataset (seasonality/cancellation patterns)
  - https://www.sciencedirect.com/science/article/pii/S2352340918315191
- Swiss tourism statistics (context and seasonality validation)
  - https://www.pxweb-r.bfs.admin.ch/pxweb/en/px-x-1003020000_102/-/px-x-1003020000_102.px/

## Licensing and hygiene

- Keep external sources documented with license and attribution.
- Do not mix uncertain-license scraped data.
- Do not store any real customer records in repository.

## Folder split

```text
data/
  synthetic_resort_policies/      # current synthetic baseline assets
  synthetic_inventory/            # add inventory, waitlist, preference CSVs
  external_open_datasets/         # optional scenario stress data
```

## Decision

For interview credibility and safety, prioritize synthetic operational data plus reproducible generation scripts.

