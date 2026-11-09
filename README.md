# Agenda Planner

A visual planner for multi-day agendas with **parallel rooms** — built as a **single HTML file** that saves to a **single JSON file**. No server, no build step, no dependencies, works fully offline.

![Agenda image export, two days with rooms](screenshots/agenda-export.svg)

## Quick start

1. Open **`Agendaplanner.html`** in Edge or Chrome (double-click the file — no installation needed).
2. Plan your agenda on the board: add days, items, participants.
3. Press **Ctrl+S** to save. The agenda is stored as **one `.agenda.json` file** — in Edge/Chrome it saves straight to a file and folder you pick; other browsers download a copy. Re-open any agenda with **Ctrl+O** or by dropping the JSON file onto the window.

Everything else — the unsaved draft, known participant names, theme and zoom — lives in your browser's `localStorage`. Nothing is ever sent anywhere.

## The whole app is one file, the agenda is one file

| What | File | Notes |
|---|---|---|
| The application | `Agendaplanner.html` | HTML + CSS + JS, zero dependencies, ~120 KB |
| Each agenda | `*.agenda.json` | Human-readable JSON, easy to diff, version-control or generate |

Because an agenda is a single JSON document, you can keep agendas in git, generate them from scripts, or share them as one attachment.

## Features

- **Days** — one or more days per agenda, each with its own date and start/end time; duplicate or delete days.
- **Rooms (multi-room days)** — split a day into parallel tracks ("Main hall", "Workshop room", …). Add rooms with the `+` on the day header, rename them inline, drag items sideways to move them between rooms or days.
- **All-rooms items** — the *All rooms* toggle in the editor stretches an item across every room (team dinner, Teams call available in all rooms). Rendered full-width with a dashed border.
- **Overbooking flags** — if a person is on two overlapping items in the same day (across rooms), both items get a red outline + ⚠, the day header and participant list show an *overbooked ×n* count, and the editor names the conflict.
- **Participants** — badges on every item, per-person totals, name suggestions learned from your previous agendas.
- **Direct manipulation** — drag item edges to resize (touching items stay glued), drag the body to move, drag further to unglue and reorder, double-click empty time to insert, pin items so edits never move them.
- **Breaks, colors, notes** — session/break types, per-item colors, notes shown on the board and in exports.
- **Exports** — preview, then copy or download as **Markdown**, **formatted HTML** (paste into Outlook/Teams/Word), a **table** (one per day; pastes as real cells into Excel/Word/Outlook, with a Room column on multi-room days), or a **PNG image** of the whole agenda (rooms shown as sub-columns, like the board). Print/PDF works too.
- **Undo/redo** (Ctrl+Z / Ctrl+Y), zoom and snap-step controls, 9 color themes (auto dark/light + Solarized, Nord, Gruvbox, Dracula).

![Multi-room day in the image export](screenshots/export-multi-room.svg)

## Agenda file format

A minimal `.agenda.json` looks like this:

```json
{
  "app": "agenda-planner",
  "version": 1,
  "title": "Team workshop",
  "snap": 5,
  "participants": [{ "id": "p1", "name": "Anna Nowak" }],
  "days": [{
    "id": "d1",
    "date": "2030-03-10",
    "start": 540,
    "end": 1020,
    "rooms": [{ "id": "r1", "name": "Main hall" }, { "id": "r2", "name": "Workshop room" }],
    "items": [
      { "id": "i1", "title": "Welcome", "start": 540, "end": 570, "room": "r1",
        "color": "#f5a524", "type": "session", "participants": ["p1"], "notes": "", "pinned": false, "spanAll": false }
    ]
  }]
}
```

Times are minutes after midnight (`540` = 09:00). `spanAll: true` stretches an item across all rooms of its day. Files saved by older versions without `rooms` load fine — they get a single default room.

## Shortcuts

| Keys | Action |
|---|---|
| `Ctrl+S` / `Ctrl+Shift+S` | Save / Save as |
| `Ctrl+O` | Open agenda file |
| `Ctrl+E` | Export preview |
| `Ctrl+Z` / `Ctrl+Y` | Undo / redo |
| `Ctrl+D` | Duplicate selected item |
| `P` / `Del` | Pin / delete selected item |
| `↑` / `↓` in a time field | Step by the snap interval |
| `?` | Show/hide the help panel |
| `Esc` | Deselect / close dialogs |

## Privacy

The app runs entirely in your browser: no accounts, no analytics, no network calls. Agendas live in the JSON files you save; drafts and name suggestions in `localStorage`.

## License

[MIT](LICENSE) — use it, modify it, share it freely; just keep the copyright notice.
