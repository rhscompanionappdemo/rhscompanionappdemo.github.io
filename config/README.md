# School configuration revisions

The app fetches `v1/<district>/<school>/manifest.json`, then the named revision.
Each revision applies to the app's bundled base, **not the previous revision**.
Copy all still-applicable overrides into every new revision. Publish the revision
file and manifest together; keep old revision files available.

## Schedule accessories

`schedule.accessories` maps school-local dates (`YYYY-MM-DD`) to details:

```json
"accessories": {
  "2026-09-29": {
    "title": "Grad Boat Cruise",
    "systemImage": "ferry.fill",
    "markdown": "Event details…"
  }
}
```

The app shows an accessory only while its Schedule view is previewing that date.
It hides on other dates, other tabs, and the map. The title is also the sheet title.
Use a short, descriptive title (up to 120 characters, one line of source text).
`systemImage` is optional; an unavailable SF Symbol falls back to the calendar icon.
Markdown is limited to 32 KiB. Use plain language, meaningful HTTPS link labels,
headings, emphasis, and flat lists. Non-HTTPS links are rendered without actions.
No remote HTML, scripts, images, or downloaded UI code are executed.

To embed the app's existing timeline rows, put a plan identifier in a `schedule`
fenced code block within Markdown:

````markdown
## Event schedule

```schedule
boatCruise
```
````

The identifier must exist in `schedule.plans` in the resolved revision. Embedded
plans use explicit items (`activity`, `block`, `plt`, `collaboration`, `lunch`),
not `nextBlock`, which requires a school-day rotation. Times, locations, grade
eligibility, and calendar actions reuse the app's timeline components. The
accessory date supplies the date for those times. Embedding a plan does not
replace classes or add the day's unrelated insertions. Change `schedule.dates`
or `schedule.insertions` separately only when the actual school schedule changes.

`accessories` replaces the entire accessory dictionary; `{}` removes every entry.
Omitting it inherits the bundled base (currently empty), not the previous revision.
Invalid dates or plan references reject the revision; the app retains its last
valid cached revision. Valid details remain available offline. Older apps that
predate accessories ignore the new field.

Revision 6 adds the September 29, 2026 cruise and retains every timetable override
from revision 5. Event source:
https://rhs.sd38.bc.ca/news/2026/09/grad-2027-boat-cruise-information
