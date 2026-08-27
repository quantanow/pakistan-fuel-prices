---
pretty_name: Pakistan Fuel Prices (OGRA)
license: cc-by-4.0
language:
  - en
size_categories:
  - n<1K
task_categories:
  - time-series-forecasting
  - tabular-regression
tags:
  - pakistan
  - energy
  - fuel-prices
  - petrol
  - diesel
  - lpg
  - ogra
  - commodity-prices
  - time-series
configs:
  - config_name: default
    data_files: data/prices.csv
---

# Pakistan Fuel Prices (OGRA)

Official retail fuel prices for Pakistan, as notified by the **Oil and Gas
Regulatory Authority (OGRA)** and parsed from the price notification PDFs the
regulator publishes. Updated within minutes of each new notification — and OGRA
has been notifying close to daily.

Currently notified: **2026-08-28**. 316 records in total.

## Coverage

The four series do **not** share a start date. Check this before quoting a
range: kerosene reaches back furthest, while petrol and diesel hold recent
notifications only.

| Product | Records | Earliest | Latest | Unit |
| --- | ---: | --- | --- | --- |
| Superior Kerosene Oil (SKO) | 209 | 2015-07-01 | 2026-08-22 | PKR/litre |
| Liquefied Petroleum Gas (LPG) | 53 | 2022-03-01 | 2026-08-01 | PKR/kg |
| High Speed Diesel (HSD) | 27 | 2026-07-21 | 2026-08-28 | PKR/litre |
| Motor Spirit (Petrol) | 27 | 2026-07-21 | 2026-08-28 | PKR/litre |

## Columns

| Column | Meaning |
| --- | --- |
| `effectiveDate` | Date the notified price takes effect (ISO 8601) |
| `product` | `Motor Spirit (Petrol)`, `High Speed Diesel (HSD)`, `Superior Kerosene Oil (SKO)` or `Liquefied Petroleum Gas (LPG)` |
| `pricePkr` | Maximum ex-depot sale price in Pakistani rupees |
| `unit` | `litre`, or `kg` for LPG |
| `source` | Always `ogra` |
| `downloadId` | OGRA's own document id — traces any row back to the notification PDF |

## Caveats

- Prices are the **maximum ex-depot sale price**. Retail pumps beyond the 23
  listed depots may add secondary freight, so a forecourt price can be higher.
- LPG is priced per kilogram, the others per litre. Never aggregate across
  products without grouping on `unit`.
- A row appears only once its notification has been parsed and reconciled
  against the source PDF. An implausible parse is dropped rather than published.
- Independent project. Not affiliated with or endorsed by OGRA.

## Live API

Free, no key, CORS enabled:

```bash
curl https://oilprices.pk/api/latest
curl "https://oilprices.pk/api/price-history?product=Petrol&year=2026&limit=5"
```

Documentation: https://oilprices.pk/fuel-price-api-pakistan
Source repository: https://github.com/quantanow/pakistan-fuel-prices

## Attribution

Licensed [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Use it
commercially, redistribute it, build on it — the one condition is credit:

> Fuel price data from [OilPrices.pk](https://oilprices.pk), CC BY 4.0.
