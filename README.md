# Posterior Fossa Society hospital resource map

A small web map with one pin per hospital and checkbox filters. It is hosted free
on GitHub Pages and shown on posteriorfossasociety.org inside a Squarespace embed.

## What each file does

| File | Purpose |
|---|---|
| `index.html` | The map page: layout, checkboxes, pins, popups, and the hospital list. |
| `hospitals.js` | The hospital data the map displays. Generated; do not edit by hand. |
| `build_map_data.py` | Reads the Society spreadsheet and writes `hospitals.js`, copying public fields only. |
| `.gitignore` | Stops the spreadsheet from ever being uploaded by accident. |

The working spreadsheet (`PFS_hospital_map_data.xlsx`) stays on your computer,
**outside** this folder. GitHub Pages on a free account requires a public
repository, so anything committed here is visible to anyone.

## One-time setup

1. Create a GitHub repository named `pfs-hospital-map` (public).
2. Upload `index.html`, `hospitals.js`, `build_map_data.py`, `README.md`, and `.gitignore`
   (or clone the repo and copy them in, then commit and push).
3. In the repository: **Settings > Pages**. Under "Build and deployment", choose
   **Deploy from a branch**, branch **main**, folder **/ (root)**. Save.
4. After a minute or two the map is live at
   `https://YOUR-USERNAME.github.io/pfs-hospital-map/`.
5. Before launch, edit the two `CHANGE-ME` addresses in the `CONFIG` block near
   the top of the script in `index.html` (corrections email and "About this map" page).

## Putting it on the Squarespace page

Add an **Embed** block (or a **Code** block) to the page and paste:

```html
<iframe src="https://YOUR-USERNAME.github.io/pfs-hospital-map/"
        title="Map of hospital resources for posterior fossa tumor surgery"
        style="width:100%; height:820px; border:0;" loading="lazy"></iframe>
```

The map keeps its list scrolling inside the frame. On phones the layout stacks
(filters, map, list); if the frame feels cramped there, add a plain link under it
to the full-page map at the same URL.

Put the full disclaimer, exclusion statement, and correction process as ordinary
text on the Squarespace page itself (drafts are on the spreadsheet's Public_Text tab).

## Updating the data (use a branch)

Working on a separate branch means the live map never changes until you have
checked the update and merged it. The pull request also shows exactly which
hospitals changed, which is a useful record for the board.

```bash
cd pfs-hospital-map
git checkout main
git pull                                  # start from the latest live version
git checkout -b data-update-2026-10       # new branch for this update

python build_map_data.py ~/path/to/PFS_hospital_map_data.xlsx
# The script prints how many hospitals it wrote and which it left off.

# Preview: double-click index.html to open it in a browser. Check the pins.

git add hospitals.js
git commit -m "Update hospital data, October 2026"
git push -u origin data-update-2026-10
```

Then on GitHub: open the **Pull request** GitHub offers for the branch, look over
the changes, and click **Merge**. The live map updates within a few minutes.
Delete the branch afterward (GitHub offers a button). If a merged update causes
a problem, the merged pull request has a **Revert** button that undoes it.

Use the same pattern for design or wording changes to `index.html`, with a
branch name that says what you are changing (for example `turn-on-abpns-filter`).

## Common edits in `index.html`

- **Turn a filter on or off:** in the `FILTERS` list, add or remove `//` at the
  start of a filter's lines. The board-certification filter is off until that
  field has been verified for most hospitals.
- **Reword a filter:** change its `label` or `help` text.
- **Colors and fonts:** the `:root` block at the top of the `<style>` section.

## How hospitals get on the map

`build_map_data.py` includes a hospital when it has at least one verified
credential in the spreadsheet, has map coordinates, and has not declined to be
listed. Internal notes, survey answers, volume figures, and conflict-of-interest
flags are never exported.

## Map tiles

The base map uses CARTO's light tiles built on OpenStreetMap data, with the
required attribution shown on the map. This is intended for modest, noncommercial
traffic; review CARTO's current terms before launch.
