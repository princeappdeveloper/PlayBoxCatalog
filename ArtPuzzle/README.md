# ArtPuzzle remote artwork

This folder is the public, unauthenticated artwork backend for the ArtPuzzle Android app. The APK
bundles the first **100** catalog entries (levels 1-85 and 87-101; daily levels 1-50 are included).
The remaining **929** catalogued originals are hosted here and downloaded in batches of
100 as the player progresses.

## Layout

```
ArtPuzzle/
  README.md
  manifest.json
  artworks/
    batch_001/   # remote catalog order 1-100
    ...
    batch_010/   # final partial batch
```

Files are the original WebP assets. They keep their established names (`pz_102.webp` ... ) and their
original bytes; nothing is recompressed, resized or renamed. The app verifies each file's size,
SHA-256 and decoded dimensions against the manifest before installing it.

| Batch | Artworks | Folder |
| --- | ---: | --- |
| `batch_001` | 100 | `artworks/batch_001/` |
| `batch_002` | 100 | `artworks/batch_002/` |
| `batch_003` | 100 | `artworks/batch_003/` |
| `batch_004` | 100 | `artworks/batch_004/` |
| `batch_005` | 100 | `artworks/batch_005/` |
| `batch_006` | 100 | `artworks/batch_006/` |
| `batch_007` | 100 | `artworks/batch_007/` |
| `batch_008` | 100 | `artworks/batch_008/` |
| `batch_009` | 100 | `artworks/batch_009/` |
| `batch_010` | 29 | `artworks/batch_010/` |

## Manifest

URL:
`https://raw.githubusercontent.com/princeappdeveloper/PlayBoxCatalog/main/ArtPuzzle/manifest.json`

Artwork base URL:
`https://raw.githubusercontent.com/princeappdeveloper/PlayBoxCatalog/main/ArtPuzzle/`

Each record in `manifest.json` contains:

- `artworkId`, `levelId` - stable app identities.
- `order` - position in the remote catalog sequence (1 = first remote artwork).
- `batch` - 1-based download batch (`(order - 1) / 100 + 1`).
- `filename`, `path` - file name and repository-relative path.
- `sha256`, `sizeBytes`, `width`, `height` - integrity and dimension checks.
- `title`, `theme` - catalog metadata.
- `sourcePage`, `originalUrl`, `author`, `license`, `licenseUrl`, `attributionRequired` -
  Wikimedia Commons attribution and licensing.

The app downloads one batch of at most 100 records only when its remaining locally playable regular
levels drop to 50 or fewer. It never downloads the whole catalog in one run and never needs
authentication.

## Attribution

All artwork is sourced from Wikimedia Commons. The per-file author and license are recorded in every
manifest entry and in the app's `assets/artwork_credits.json`. Follow each license's attribution
requirements when redistributing or reusing these files.

## Publishing

1. Commit this folder to the repository's `main` branch.
2. Push to `https://github.com/princeappdeveloper/PlayBoxCatalog.git`.
3. Confirm the raw manifest URL above resolves and that `sha256` values match the committed files.
