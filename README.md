# Guess What Web Resources

This repository hosts the dynamic card library for the Guess What iOS app.

Production URL:

```text
https://www.chunyangwen.com/guesswhat.app/manifest.json
```

## Structure

```text
manifest.json
images/
  animals/
  cities-china/
  cities-world/
  places/
index.html
.github/workflows/deploy-pages.yml
```

The iOS app downloads `manifest.json`, resolves image paths relative to that file, downloads the images, and caches the valid card library locally.

## Manifest Format

```json
{
  "version": 1,
  "categories": [
    {
      "id": "cities-china",
      "name": "Cities in China",
      "symbol": "building.2.crop.circle.fill",
      "accent": "#F25B3A",
      "cards": [
        {
          "id": "beijing",
          "title": "Beijing",
          "image": "images/cities-china/beijing.png"
        }
      ]
    }
  ]
}
```

Required category fields:

- `id`: Stable unique identifier. Prefer lowercase letters, numbers, dashes, or underscores.
- `name`: Display name in the app.
- `cards`: The card list for this category.

Optional category fields:

- `symbol`: SF Symbol name used by the app.
- `accent`: Hex color in `#RRGGBB` format.

Required card fields:

- `id`: Stable unique identifier within the category.
- `title`: The word or phrase to guess.

Optional card fields:

- `image`: Relative or absolute JPG/PNG URL. Relative paths are resolved from the manifest URL.

## Adding Cards

1. Add a JPG or PNG under `images/<category-id>/`.
2. Add a card entry to `manifest.json`.
3. Commit and push to the default branch.
4. GitHub Actions deploys the updated static site.
5. Open the app and tap the refresh button on the deck picker.

Keep image filenames stable. The app caches downloaded images by category and card ID.

## Fallback Behavior

If this site is offline, the manifest is malformed, or any image download fails, the app keeps the current cached decks. If there is no cache yet, it uses the bundled fallback decks in the app.

Individual image failures do not block the entire library. A card with a missing image remains playable in word mode.
