# THAT Token Lists

This repository hosts the **static** and **dynamic** token-list JSON files used by the THAT app and contributed by the community.

## Repository Structure

```
├── static/       # Tokens bundled in the latest app release
│   ├── 1.json    # Ethereum mainnet
│   ├── 137.json  # Polygon mainnet
│   └── …         # Other chain IDs
└── dynamic/      # Community-submitted deltas and new chains
    ├── 1.json    # Overrides or extras for Ethereum
    ├── 137.json  # Overrides or extras for Polygon
    └── 1234.json # New EVM chain token-list (if not in static)
```

* **static/** contains the token lists that are shipped in the latest app build. These files are updated by the THAT team as part of each app release (to reduce app data consumption).
* **dynamic/** contains only the *delta* entries (new tokens or updated metadata) that should overlay the static lists at runtime. After a release, the entries from `dynamic/` get merged into `static/` in the next version, and `dynamic/` is reset to `[]`.

## JSON Schema

All token-list files must be valid JSON arrays of objects conforming to the following structure:

```jsonc
[
  {
    "address":      "0x…",           // The contract address (lowercase)
    "chainId":      137,             // EVM chain ID
    "name":         "THAT",          // Full token name
    "symbol":       "THAT",          // Ticker symbol
    "decimals":     18,              // Number of decimals
    "logoURI":      "https://…",     // (optional) URL for token logo
    "coingeckoId":  "that",          // (optional) CoinGecko identifier
    "listedIn":     ["coingecko"]    // (optional) If other indexers list this token
  }
]
```

## Contributing

We welcome pull requests! To add or update tokens:

1. **Fork** this repository.
2. **Add or edit** the relevant JSON file:
   * To update an existing token’s metadata, edit the object in `dynamic/<chainId>.json`.
   * To add a new token on an existing chain, append it to `dynamic/<chainId>.json`.
   * To support a brand-new EVM chain, create a new file `dynamic/<newChainId>.json` with your token entries.
3. **Submit** a pull request describing your changes.

### Naming & Location

* Files must be named exactly `<chainId>.json` (e.g. `56.json` for BSC).
* Place your additions/overrides in the **dynamic/** directory only.
* Ensure no duplicate `address+chainId` entries within the same file.

## Release Process

1. On **app release**, the THAT team:

   * Copies any updates / extras from `dynamic/` into `static/`.
   * Clears out `dynamic/…` files (set them back to an empty array `[]`).
2. New app builds will bundle the updated `static/` files.
