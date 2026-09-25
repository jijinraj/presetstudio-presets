# PresetStudio Presets

This repository is a static preset source for PresetStudio.

PresetStudio does **not** clone this repository with Git. It reads `catalog.json`,
then downloads individual preset files referenced by the catalog.

## Repository layout

```text
presetstudio-presets/
├── README.md
├── catalog.json
├── presets/
│   └── warm-film.presetstudio.json
└── previews/
    └── .gitkeep
```

## Catalog contract

`catalog.json` lives at the repository root.

Current v1 catalog shape:

```json
{
  "format": "presetstudio.catalog",
  "schemaVersion": 1,
  "name": "PresetStudio Presets",
  "description": "Default PresetStudio preset collection.",
  "presets": [
    {
      "id": "8d8b5a34-75a9-4f3a-a1c8-e3d95f986d4b",
      "name": "Warm Film",
      "author": "Jijin",
      "revision": 1,
      "preset": "presets/warm-film.presetstudio.json"
    }
  ]
}
```

Rules for v1:

- `format` must be exactly `presetstudio.catalog`.
- `schemaVersion` is the catalog schema version, not the preset revision.
- Every `preset` path is relative to the repository root.
- Use forward slashes in paths.
- `id` must match the referenced preset file's portable preset `id`.
- `revision` must match the referenced preset file's revision.
- Preset names are display labels, not identities.
- A preview image is optional for now. Do not add a `preview` field unless a real preview file exists.
- PresetStudio should validate the referenced preset again after download; the catalog is not trusted as executable data.

## Portable preset contract

Preset files use the `.presetstudio.json` suffix and contain only portable preset data.

They must **not** contain local PresetStudio library metadata such as:

- `libraryId`
- `sourceId`
- `remotePresetId`
- `installedAt`
- `updatedAt`

Example:

```json
{
  "format": "presetstudio.preset",
  "schemaVersion": 1,
  "id": "8d8b5a34-75a9-4f3a-a1c8-e3d95f986d4b",
  "name": "Warm Film",
  "description": "A gentle warm film-inspired look with softer highlights and restrained saturation.",
  "author": "Jijin",
  "createdAt": "2026-09-25T00:00:00.000Z",
  "revision": 1,
  "adjustments": {
    "exposure": 0.15,
    "contrast": 12.0,
    "highlights": -22.0,
    "shadows": 10.0,
    "whites": -6.0,
    "blacks": -10.0,
    "temperature": 10.0,
    "tint": 2.0,
    "vibrance": 16.0,
    "saturation": -4.0
  }
}
```

## Adding a preset

1. Export or create a valid `.presetstudio.json` file.
2. Put it under `presets/`.
3. Add one matching entry to `catalog.json`.
4. Increment the preset's `revision` when the same remote preset is updated.
5. Keep the catalog entry's `revision` in sync with the preset file.
6. Optionally add preview images later under `previews/`.

## Offline behavior

PresetStudio should download and install a selected remote preset into its local
offline preset library. Removing this repository or losing internet access must
not break presets that were already installed.

## Multiple sources

This repository is only one source. PresetStudio's source registry is designed
to support multiple repositories or direct catalog URLs independently.
