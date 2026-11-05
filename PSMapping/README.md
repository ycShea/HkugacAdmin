# PS Mapping

🔗 **[Click here to access the Live Tool](https://ycshea.github.io/HkugacAdmin/PSMapping/)**

🔗 **[PS Mapping (Most Updated)](https://docs.google.com/spreadsheets/d/1-tcFI4oAFt4nPBKSfCQcwHwCGd8QQJnW8eFmSBeVYjc/edit?gid=450436726#gid=450436726)**

A small web tool that turns the primary school names typed by families (spelling mistakes, abbreviations, Chinese or English) into one **standard school name**. It reads the mapping table kept in a Google Sheet, helps you decide the unclear cases, and gives you the new mappings to add back to the sheet.

## What is in this repository

| File | What it is |
|---|---|
| `index.html` | The web tool (runs entirely in the browser) |
| `rules.js` | *Optional.* Your `AISearch` function. If it is in the same folder, the tool uses it as an extra matching step (see [Rules](#rules-optional-rulesjs)) |
| `AppsScript.gs` | Menu code for the Google Sheet (`[PS Tools]`) |
| `Upload.html` | The upload box used by the menu item *Append new mapping to AllHashTable* (add it to Apps Script as an HTML file named `Upload`) |
| `README.md` | This guide |

## The Google Sheet

| Sheet | Content |
|---|---|
| `AllHashTable` | Column A **Input** (a spelling seen before), column B **Unified** (the standard name), column C the type (your VLOOKUP, maintained in the sheet) |
| `香港小學名單` | The list of standard names. A = name, B = abbreviation, C = district, **D = type: `HK`, `International`, `China` or `Oversea`** |
| `AdmInfoDay(Responses)` | The form responses; column A holds the school name typed by the family |

The tool reads `AllHashTable` and `香港小學名單`. For this, the sheet must be shared as **Anyone with the link can view**.

## Typical workflow

1. In the Google Sheet, click **[PS Tools] → Export unmatched schools**. It lists the schools in `AdmInfoDay(Responses)` that are not yet in `AllHashTable`. Click **Download CSV**.
2. Open the **[Live Tool](https://ycshea.github.io/HkugacAdmin/PSMapping/)**.
3. Open **Settings**, paste the Google Sheet URL (it is remembered in your browser), choose the CSV as the student list and click **Start matching**.
4. Go through the rows (see below), then click **Download new mapping**. This gives you `PS_Result.xlsx`.
5. Back in the Google Sheet, click **[PS Tools] → Append new mapping to AllHashTable** (or Step 3 of the Export pop-up) and choose `PS_Result.xlsx`.

## How the tool matches

Each name goes through these steps, in this order, and stops at the first one that finds it:

1. **Exact** match in `AllHashTable` (ignoring capitals, spaces and full-width characters).
2. **Normalised** match in `AllHashTable` (also ignoring punctuation, simplified vs traditional Chinese, and common words such as *Primary School*; typos in those words, such as *Schiol*, are fixed).
3. **Abbreviation** from column B of `香港小學名單`.
4. **Rules** from `rules.js`, if present.
5. **Fuzzy matching** against all standard names: similarity of the name, matching of words, and initials (for example `HPCC Hoi Ping` finds *Hoi Ping Chamber Of Commerce*).

Steps 1 and 2 are shown as **Auto 100%**. Everything else is a suggestion that you confirm.

## Using the tool

**Statuses**

| Status | Meaning |
|---|---|
| **Auto 100%** | Found in `AllHashTable` (step 1 or 2). Nothing to do. |
| **Review** | A suggestion, or none. Up to three suggestions are shown with a score, and the best one is pre-filled if it is close enough. Rows with a pre-filled name are listed first, strongest first. |
| **Confirmed** | You accepted the name. |
| **Skipped** | You skipped the record. |

A summary line below the toolbar shows how many rows are in each status.

**Buttons on a Review row**

```
✓ Confirm   🔍 Google
⏭ Skip
⧉ Copy      ✕ Clear
```

| Button | What it does |
|---|---|
| **✓ Confirm** | Accepts the name in the box and adds the mapping to the new mappings |
| **🔍 Google** | Opens a Google search for the input in a new tab |
| **⏭ Skip** | Skips this record. The standard name in the result is the input itself, and the row leaves the Review list. It is **not** added to the new mappings. |
| **⧉ Copy** | Copies the input into the name box so you can edit it and use type-to-search |
| **✕ Clear** | Empties the name box |
| **Undo** | Shown on finished rows; puts the row back to Review |
| **✔ Confirm all mapped (N)** | Floating button. Confirms every Review row that has a name in its box. It disappears when there are none. |
| **↑ Top** | Floating button. Scrolls back to the top |

The **Standard name** box has type-to-search over all standard names. Click a suggestion button under it to fill the box. A suggestion marked *rule* came from `rules.js`.

**Ticks.** *International*, *Mainland* and *Overseas* come from column D of `香港小學名單`: `International` ticks International, `China` ticks Mainland, `Oversea` ticks Overseas, and `HK` ticks nothing. For names that are not in the list, the tool guesses from keywords, so please check those.

**Downloads**

| Button | File | Content |
|---|---|---|
| **Download new mapping** | `PS_Result.xlsx` | Sheet **New mappings** (`Input` \| `Standard name`) first, then sheet **Result** (every row of your list with its standard name, status and the three ticks) |
| **Download updated AllHashTable** | `PS_Mapping_updated.xlsx` | `AllHashTable` with the new mappings added, plus `香港小學名單` |

*New mappings* contains the rows you **confirmed**, plus the Auto rows whose exact spelling was not yet in `AllHashTable`. **Skipped** rows are not added. All names are trimmed of leading and trailing spaces.

> Progress is not saved. Download your files before closing the page.

## Rules (optional: `rules.js`)

Put your `AISearch` function in a file called `rules.js` next to `index.html`. When the page loads it, the top of the page shows "Rules file loaded.".

- A rule hit appears as a **100% "rule"** suggestion and is pre-filled, but stays in **Review**, so you still confirm it (or use **✔ Confirm all mapped**).
- If a rule returns a `? ...` list, all the choices are shown and none is pre-filled.
- If a rule returns a name that is not in your standard list, it is only used when it closely matches a listed name.
- Loading adjusts the file automatically: Latin keywords of **4 letters or fewer must match a whole word** (so `SH` no longer matches *Shek Kip Mei*), the `##` prefix is removed, the call to `lookupFromAllHashTable` is skipped (the page already does the lookup), and a few known mistakes are corrected (`SKHSJPS`, `HKMLCPS` and the *Kei Wan* brackets).

## Google Sheet menu: `[PS Tools]`

| Item | What it does |
|---|---|
| **Export unmatched schools** | Pop-up with 3 steps: **1.** download a CSV of the unique schools in `AdmInfoDay(Responses)` that are not in `AllHashTable`; **2.** open the tool link and upload that CSV; **3.** a button to append the result (same as the next item) |
| **Append new mapping to AllHashTable** | You choose `PS_Result.xlsx`. It checks that the sheet **New mappings** exists with the headers `Input` \| `Standard name`, then shows how many rows are new, already there with the same name, already there with a **different** name, or repeated in the file. It appends only the new ones. A second upload of the same file adds nothing. |
| **Help** | Short guide and a link to this page |

Existing entries are never overwritten. Only columns **A and B** are written, below the last row that has something in column A, and **column C (your VLOOKUP) is never touched**.

## Setup

**The tool (GitHub Pages)**
1. Upload `index.html` (and `rules.js` if you use it) to the repository.
2. In **Settings → Pages**, publish the branch.

**The sheet menu (Apps Script)**
1. In the Google Sheet, open **Extensions → Apps Script**.
2. Paste `AppsScript.gs` into the code file.
3. Add an HTML file named exactly **Upload** (**+ → HTML**) and paste `Upload.html` into it.
4. Check the settings at the top of `AppsScript.gs`: `SRC_SHEET`, `SRC_COL` (column of the school name; A = 1), `HASH_SHEET`, `TOOL_URL`, `HELP_URL`.
5. Save and reload the Google Sheet. The first time, Google asks you to approve permissions.

No token or web app deployment is needed.

## Privacy

- The tool runs in your browser. The student list is not uploaded to GitHub or to any server.
- The page is public, but it contains no data. The Google Sheet address is entered in Settings and stored in your own browser only.
- `rules.js` is public if it is in the repository, so keep only school names and keywords in it.
- Do not put student files in this repository.

## Troubleshooting

| Problem | What to try |
|---|---|
| The sheet cannot be read | Share it as *Anyone with the link can view*, or choose a mapping file under **Settings** |
| The page looks old after an update | Hard refresh: **Ctrl+F5** (Windows) or **Cmd+Shift+R** (Mac) |
| No "Rules file loaded." message | `rules.js` must be in the same folder as `index.html`. Open the browser console for details |
| *Sheet "New mappings" was not found* | Use the `PS_Result.xlsx` downloaded with **Download new mapping** |
| *Nothing to append* | Those mappings are already in `AllHashTable` |
| Ticks are wrong for a school | Set column D (`HK`, `International`, `China`, `Oversea`) for that school in `香港小學名單` |
| A school never matches | Use **🔍 Google**, pick the correct name and **✓ Confirm**; it will match automatically next time |
| New rows in `AllHashTable` have no type | Make sure your column C VLOOKUP covers the new rows |

## Credits

Code written by Claude Sonnet 5.5 (Anthropic), version 2026-11-05.
Requested and maintained by <Ada@Claude / Primary School Mapping**>
