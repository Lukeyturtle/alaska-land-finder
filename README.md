# Alaska Land Finder

A self-contained web app for browsing plots of land for sale in Alaska — map, filters, and links to each listing.

## Run it

Open `index.html` directly in any browser, or serve the folder:

```bash
python3 -m http.server 8942 --directory .
```

then visit http://localhost:8942.

## What's inside

- **index.html** — the whole app (Leaflet map, filters, cards). No build step.
- **data-1.js … data-4.js** — 500 Alaska listings scraped from LandWatch on 2026-08-19
  (price, acres, location, coordinates, property types, listing URL).
- **data-dnr.js** — 171 State of Alaska auction parcels (auctions #498 & #499) scraped
  from dnr.alaska.gov on 2026-08-19, with coordinates joined from DNR's ArcGIS
  `MLW/LandSalesParcels` service (140 of 171 have map locations; 31 list-only).
  State parcels show as blue-ringed markers; most require Alaska residency —
  the "Open to non-residents only" filter finds the 19 that don't.

## Filters

- Text search, borough/state-region, price range, acreage range
- Source: State of Alaska auctions / private listings; "open to non-residents only"
- **Vacant land only** (default on — hides listings with houses/cabins)
- Waterfront only, hide pending sales
- **⛰️ Mountain land** — parcels with ≥390 ft of terrain relief within ~1 mile,
  or ≥3,000 ft elevation. Derived from ASTER 30 m elevation data
  (5 samples per parcel via opentopodata.org), stored in `data-terrain.js`.
- **⛏️ Gold country** — parcels within ~6 miles (10 km) of a known gold
  occurrence in the USGS Alaska Resource Data File (ARDF, 4,205 Au sites,
  1,771 of them placer). Cards show distance and whether the nearest site is
  placer. **Caveat:** owning the surface ≠ owning mineral rights — Alaska
  reserves subsurface rights on state-sold land, and most private parcels
  don't convey them either. Recreational panning has its own rules.
- Sort by $/acre, price, or acreage. Map markers are colored by $/acre
  (green < $5k, amber < $20k, red above); state parcels have a blue ring.

## Refreshing the data

The dataset is a snapshot. To refresh, ask Claude Code to re-scrape
LandWatch (the site blocks curl, so it has to go through the in-app browser)
and regenerate the `data-*.js` files.

## Other places to find Alaska land

- **Alaska DNR State Land Sales** — the state sells land directly:
  sealed-bid auctions and first-come "over-the-counter" parcels at fixed prices.
  https://dnr.alaska.gov/mlw/landsales/ — current auction parcels are included
  in the app; the OTC pool was empty at scrape time (unsold auction parcels
  typically move there after the auction closes).
- LandWatch (full ~1,950 listings): https://www.landwatch.com/alaska-land-for-sale
- LandSearch: https://www.landsearch.com/properties/alaska
