# Pakistan Fuel Prices — Open Dataset

Official petrol, diesel, kerosene and LPG prices for Pakistan, parsed from the
price notifications published by the **Oil and Gas Regulatory Authority
(OGRA)** and updated within minutes of each new notification.

Currently notified: **2026-09-03**.
326 records in total.

Maintained by [OilPrices.pk](https://oilprices.pk). This repository is a mirror — the
live API and documentation are at [https://oilprices.pk/fuel-price-api-pakistan](https://oilprices.pk/fuel-price-api-pakistan).

## Coverage

The four series do **not** share a start date. Check this table before quoting
a range: kerosene reaches back furthest, while petrol and diesel hold recent
notifications only.

| Product | Records | Earliest | Latest | Unit |
| --- | ---: | --- | --- | --- |
| Superior Kerosene Oil (SKO) | 210 | 2015-07-01 | 2026-08-29 | PKR/litre |
| Liquefied Petroleum Gas (LPG) | 54 | 2022-03-01 | 2026-09-01 | PKR/kg |
| High Speed Diesel (HSD) | 31 | 2026-07-21 | 2026-09-03 | PKR/litre |
| Motor Spirit (Petrol) | 31 | 2026-07-21 | 2026-09-03 | PKR/litre |

## Files

| File | Contents |
| --- | --- |
| `data/prices.json` | Full price history, one record per product per notification |
| `data/prices.csv` | The same history as CSV, oldest first |
| `data/latest.json` | The currently notified rate for each product |
| `data/platts.json` | OGRA's published 7-day Platts Arab Gulf rolling averages and daily quotes |
| `data/pricing-formula.json` | Formula components from the latest notification: customs duty, IFEM, margins, levies |

### Record shape

```json
{
  "effectiveDate": "2026-08-19",
  "product": "Motor Spirit (Petrol)",
  "pricePkr": 334.54,
  "unit": "litre",
  "source": "ogra",
  "downloadId": "15402"
}
```

`downloadId` is OGRA's own document id, so any record can be traced back to the
notification PDF it came from.

## Live API

No key, no registration, CORS enabled:

```bash
curl https://oilprices.pk/api/latest
curl "https://oilprices.pk/api/price-history?product=Petrol&year=2026&limit=5"
```

Full documentation: [https://oilprices.pk/fuel-price-api-pakistan](https://oilprices.pk/fuel-price-api-pakistan)

## Licence

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Use it commercially,
redistribute it, build on it. The one condition is attribution:

```html
Fuel price data from <a href="https://oilprices.pk">OilPrices.pk</a>, CC BY 4.0.
```

The underlying prices are public notifications issued by
[OGRA](https://www.ogra.org.pk); the licence covers this compilation of them.

## What is deliberately not here

- `commodities.json` — Yahoo Finance derived — same
- `failures.json` — operational log, not data
- `manifest.json` — internal scraper bookkeeping
- `markets.json` — Yahoo Finance / PSX derived — redistribution is against Yahoo's terms

## Caveats

- Prices are the **maximum ex-depot sale price** OGRA notifies. Retail pumps
  beyond the 23 listed depots may add secondary freight.
- LPG is priced per kilogram, the others per litre. Every record carries `unit`.
- A record appears only once its notification has been parsed and reconciled
  against the source PDF. An implausible parse is dropped rather than published.
- Independent project. Not affiliated with or endorsed by OGRA.
