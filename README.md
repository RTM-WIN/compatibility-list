# RTM WIN — compatibility list

JSON files, one per emulated machine (and one for the 32-bit app), read directly by the RTM WIN iOS app.

| file | machine |
|---|---|
| `pc.json` | PC (Windows) titles, 32- and 64-bit |
| `pc-32bit.json` | PC (Windows) 32-bit titles, as the 32-bit app (RTMHub32) runs them |
| `xbox360.json` | Xbox 360 titles |

The app fetches these over HTTPS from `raw.githubusercontent.com` on the
`main` branch, so **an edit merged here reaches every installed app** without a
release. That is the point of keeping them in their own repository.

## Shape

```json
{
  "schemaVersion": 1,
  "platform": "pc",
  "platformName": "PC (Windows)",
  "updated": "2026-09-07",
  "entries": [
    {
      "id": "steins-gate-2010",
      "title": "Steins;Gate",
      "year": 2010,
      "arch": "x86",
      "status": "playable",
      "notes": "UI glitchy"
    }
  ]
}
```

| field | required | meaning |
|---|---|---|
| `id` | yes | stable, lowercase, hyphenated. **Never reuse or rewrite one** — the app keys on it |
| `title` | yes | the title as a player would recognise it |
| `year` | no | release year, shown beside the title |
| `arch` | PC only | `x86` (32-bit) or `x64` (64-bit); the app groups by it |
| `status` | yes | see below |
| `notes` | no | one short phrase, e.g. `UI glitchy` |

### `status`

| value | meaning |
|---|---|
| `playable` | reaches gameplay and can be played through |
| `in-menu` | boots and reaches menus, but not gameplay |
| `intro` | shows logos or an intro and goes no further |
| `boots` | starts but produces nothing usable |
| `unplayable` | does not start |

An unrecognised status is not an error: the app shows it verbatim in a neutral
style rather than dropping the entry, so a new value can be introduced here
before the app knows about it.

## Editing

`schemaVersion` is what protects installed apps from a breaking change. Adding
a field or a `status` value does **not** need a bump — old apps ignore what they
do not understand. Renaming or removing a field, or changing what one means,
**does**: bump it, and expect apps built before the bump to refuse the file
rather than misread it.

Keep `updated` current; the app shows it, and a stale date is how a reader tells
a quiet list from an abandoned one.
