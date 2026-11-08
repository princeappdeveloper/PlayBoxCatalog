# ArtPuzzle Daily Puzzle artwork

This namespace is reserved for date-specific Daily Puzzle images. It is independent of the regular
`manifest.json` and `artworks/` catalog. Never copy, rename, crop, or otherwise reuse regular-level
artwork as a Daily Puzzle asset.

## Publication status

The catalog contains **242** licensed photographs with complete monthly manifests:

- November 2026: 30 images (bundled in Android)
- December 2026: 31 images (bundled in Android)
- January 2027: 31 images (bundled in Android)
- February 2027: 28 images (remote-only)
- March 2027: 31 images (remote-only)
- April 2027: 30 images (remote-only)
- May 2027: 31 images (remote-only)
- June 2027: 30 images (remote-only)

The 150 February-June images are catalog-only; do not copy them into Android assets. All images are
distinct from the regular catalog and have individual records in [`ATTRIBUTION.md`](./ATTRIBUTION.md).
The original files for the 92-image bundled set and the first seven February images were checked
against Wikimedia Commons SHA-1 metadata. The other 143 February-June images were acquired from
Wikimedia Commons' standardized 3840px thumbnail endpoint after direct original-file requests were
rate-limited; their source pages, creators, and licenses were verified. All outputs preserve aspect
ratio, are converted to WebP at a maximum dimension of 2560 pixels, and have measured dimensions,
sizes, and SHA-256 digests in their manifests. A manifest is included only when every calendar day
in its month has a valid image.

## Layout and naming

Publish a complete month using this layout:

```text
daily_puzzles/
  README.md
  ATTRIBUTION.md
  manifest/
    2026/11.json
    2026/12.json
    2027/01.json
    2027/02.json
    2027/03.json
    2027/04.json
    2027/05.json
    2027/06.json
  images/
    2026/11_november/v1/
    2026/12_december/v1/
    2027/01_january/v1/
    2027/02_february/v1/
    2027/03_march/v1/
    2027/04_april/v1/
    2027/05_may/v1/
    2027/06_june/v1/
```

Use the stable ID `daily_YYYY_MM_DD` and the matching `.webp` filename. For example, the ID and
filename for February 1, 2027 are `daily_2027_02_01` and `daily_2027_02_01.webp`. Keep assets in
their date's year/month folder. Include a version directory in image paths so a published pack
version is immutable; increment `packVersion` and use a new version directory for updates.

Create a monthly manifest only when all valid dates in that month have actual image files. Do not
advertise or create manifests for future months that are not ready.

## Android manifest contract

The Android `DailyPuzzleManifestParser.parseRemote` requires `schemaVersion: 1`, a non-empty
`packVersion` string using letters, digits, `.`, `_`, or `-`, the exact integer `year` and `month`,
and one `days` entry for every calendar date in the requested month. A `source: "remote"` field may
be included for catalog clarity; the parser determines remote source from the download path.

Each day entry must contain:

- `id`: `daily_YYYY_MM_DD`, matching its date exactly.
- `date`: an exact `YYYY-MM-DD` calendar date in the manifest month.
- `path`: a safe path relative to the configured Daily Puzzle base URL, such as
  `images/2027/02_february/v1/daily_2027_02_01.webp`.
- `format`: `webp`.
- `width` and `height`: actual positive decoded dimensions, supplied together.
- `sizeBytes`: the actual positive file size.
- `sha256`: the actual 64-character hexadecimal SHA-256 digest.
- `attribution` and `license`: truthful non-empty strings.

The Android parser expects `attribution` and `license` as strings, not nested objects. Put the full
creator, source-page URL, license URL/terms, and required attribution text in
[`ATTRIBUTION.md`](./ATTRIBUTION.md), with one verified record per image. Do not copy the regular
catalog's unrelated metadata into a Daily manifest.

## Hosting

The existing catalog is published from the `princeappdeveloper/PlayBoxCatalog` GitHub repository
using raw GitHub URLs. The Android downloader appends `manifest/YYYY/MM.json` to the configured
Daily Puzzle base URL, and appends each entry's `path` to the same base. For this directory, the
base URL must therefore be:

```text
https://raw.githubusercontent.com/princeappdeveloper/PlayBoxCatalog/main/ArtPuzzle/daily_puzzles
```

This yields the manifest URL
`https://raw.githubusercontent.com/princeappdeveloper/PlayBoxCatalog/main/ArtPuzzle/daily_puzzles/manifest/YYYY/MM.json`
and image URLs beneath
`https://raw.githubusercontent.com/princeappdeveloper/PlayBoxCatalog/main/ArtPuzzle/daily_puzzles/`.
The Android build uses this Daily base URL by default; it can be overridden with the Gradle property
`dailyPuzzleBaseUrl`. Do not change the regular catalog URL.

## Image, licensing, and attribution requirements

Only add genuine, unique, high-resolution photographs whose source and license permit public
redistribution in this repository and use in the game. Images must be sharp, watermark-free, free of
promotional marks, and detailed enough for jigsaw cropping. Preserve aspect ratio and use high-quality
WebP conversion. Remove unnecessary metadata when appropriate.

Before accepting a file, verify that it is not already in the regular artwork collection, is not a
near-duplicate of another Daily image, decodes as WebP, and has accurate dimensions, byte count, and
SHA-256. Store accurate attribution and license details in `ATTRIBUTION.md`; never infer those
details from a filename or an image host.

## Adding and validating a future month

1. Acquire each image from a source whose license explicitly permits the intended redistribution.
   Record its original image page, creator, license, license URL, and required credit immediately.
2. Compare the candidate images against all regular catalog images and one another; reject duplicate
   and near-duplicate photographs.
3. Convert verified originals, or a Commons-standard high-resolution thumbnail when direct original
   access is rate-limited, to high-quality WebP without distorting or aggressively compressing the
   image. Record which source rendition was used. Keep each actual file in the corresponding
   year/month/version directory.
4. Generate the month manifest using the Android field names above. Use actual measured dimensions,
   file sizes, and SHA-256 digests. Confirm that IDs and dates are unique, all dates belong to the
   month, and every `path` resolves to an actual file under this directory.
5. Decode every image and verify its WebP signature, dimensions, size, and SHA-256 against the
   manifest. Ensure the manifest has exactly 28, 29, 30, or 31 entries as appropriate.
6. Confirm the raw manifest and image URLs resolve on the `main` branch. Build the Android app with
   the Daily Puzzle base URL configured and let its downloader validate and install the complete
   month before announcing availability.
7. Publish the immutable manifest, files, and attribution together. Do not overwrite assets or
   change a manifest without incrementing `packVersion` and publishing to a new versioned image path.

The Android downloader fetches one exact year/month manifest, requires a complete date set, checks
image signatures, decoding, dimensions, optional byte counts and SHA-256, then installs the pack
atomically. It prefetches the next month and keeps downloaded versions in app-private storage.

## Current files

All 242 daily files are present in versioned image directories and referenced by complete
manifests. The catalog must be committed and pushed to `main` before the raw URLs can serve these
files.
