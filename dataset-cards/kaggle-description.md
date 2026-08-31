# Pakistan Fuel Prices (OGRA)

Official retail fuel prices for Pakistan, as notified by the **Oil and Gas
Regulatory Authority (OGRA)** and parsed from the price notification PDFs the
regulator publishes. Updated within minutes of each new notification — and OGRA
has been notifying close to daily.

Currently notified: **2026-09-01**. 322 records in total.

## Coverage

The four series do **not** share a start date. Check this before quoting a
range: kerosene reaches back furthest, while petrol and diesel hold recent
notifications only.

| Product | Records | Earliest | Latest | Unit |
| --- | ---: | --- | --- | --- |
| Superior Kerosene Oil (SKO) | 210 | 2015-07-01 | 2026-08-29 | PKR/litre |
| Liquefied Petroleum Gas (LPG) | 54 | 2022-03-01 | 2026-09-01 | PKR/kg |
| High Speed Diesel (HSD) | 29 | 2026-07-21 | 2026-09-01 | PKR/litre |
| Motor Spirit (Petrol) | 29 | 2026-07-21 | 2026-09-01 | PKR/litre |

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

## Licence

CC BY 4.0. Please credit [OilPrices.pk](https://oilprices.pk) with a link.
