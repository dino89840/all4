# Allgabar listing mirror (CM FLIX — Myanmar + All 4)

The app reads its Allgabar list from `allgabar.json` in this repo instead of
scraping allgabar.com directly, because the site intermittently blocks
listing requests (Cloudflare, IP/rate/time dependent).

**This file carries listing data only** (title / thumb / detail URL).
It never contains stream URLs — Play / Download still resolve server-side
via the CM FLIX backend.

## File format

```json
{
  "status": {
    "complete": true,        // false while the backfill is still running
    "mode": "incremental",
    "totalPages": 728,       // listing pages fetched
    "totalItems": 14560,     // unique movies (deduplicated by detail URL)
    "lastPageFetched": 728,
    "failedPages": [],       // must be [] for complete=true
    "lastFullCheck": "2026-10-19T...",
    "asOf": "2026-10-16T...",
    "firstItem": {"title": "...", "thumb": "...", "detail": "..."},
    "lastItem": {"title": "...", "thumb": "...", "detail": "..."}
  },
  "items": [
    {"title": "...", "thumb": "https://allgabar.com/...", "detail": "https://allgabar.com/archives/12345"}
  ]
}
```

Items are newest first. `complete=true` means every listing page was
fetched (next-page link exhausted or HTTP 404 past the end), no page
failed, and no duplicates exist.

## Sync

Synced hourly by the private `all4-upload` repo (cron-job.org triggers its
`sync.yml` workflow). The sync code lives there; this repo holds only
the data file + this README.
