# PS Mapping

🔗 **[Click here to access the Live Tool](https://ycshea.github.io/HkugacAdmin/PSMapping/)**

🔗 **[PS Mapping (Most Updated, NEW method)](https://docs.google.com/spreadsheets/d/1-tcFI4oAFt4nPBKSfCQcwHwCGd8QQJnW8eFmSBeVYjc/edit?gid=450436726#gid=450436726)**

A small web tool that turns the primary school names typed by families (spelling mistakes, abbreviations, Chinese or English) into one **standard school name**. It is connected to a Google Sheet: it loads the schools that are not yet mapped, helps you decide the unclear cases, and saves the new mappings back to the sheet with one click.

## What is in this repository

| File | What it is |
|---|---|
| `index.html` | The web tool (matching runs in the browser) |
| `rules.js` | *Optional.* Your `AISearch` function. If it is in the same folder, the tool uses it as an extra matching step (see [Rules](#rules-optional-rulesjs)) |
| `AppsScript.gs` | Code for the Google Sheet: the `[PS Tools]` menu and the Web App that the tool talks to |
| `Upload.html` | The upload box used by the old menu item *Append new mapping to AllHashTable* (add it to Apps Script as an HTML file named `Upload`) |
| `README.md` | This guide |

## The Google Sheet

| Sheet | Content |
|---|---|
| `AllHashTable` | Column A **Input** (a spelling seen before), column B **Unified** (the standard name), column C the type (your VLOOKUP, maintained in the sheet) |
| `香港小學名單` | The list of standard names. A = name, B = abbreviation, C = district, **D = type: `HK`, `International`, `China` or `Oversea`** |
| `AdmInfoDay(Responses)` | The form responses; column A holds the school name typed by the family |

The sheet can stay **private**. The tool reads and writes it only through the Web App (see [Setup](#setup)).

## Typical workflow

1. In the Google Sheet, click **[PS Tools] → Update PS list**, then click **Open PS Mapping tool**. (**Copy link** copies the same link if you prefer to paste it in another browser.)
2. In the tool, click **Start matching**. It loads the schools in `AdmInfoDay(Responses)` that are not yet in `AllHashTable`.
3. Go through the rows (see below). Click **✓ Confirm** on each row, or **✔ Confirm all mapped**.
4. Click **Save to AllHashTable**. The tool shows how many rows will be added and asks you to confirm.
5. Go back to the Google Sheet. The new rows are at the bottom of `AllHashTable`.

The tool remembers the connection in your browser, so later you can open the plain tool link. A new browser, a new browser profile or cleared site data needs the link from **Update PS list** once.

If the page shows a red "Not connected" message, open it from the Sheet menu as in step 1.

### Old manual route (still available)

1. **[PS Tools] → Export unmatched schools** and download the CSV.
2. Open the tool (not connected), choose the CSV, and enter the Google Sheet URL in **Settings** (the sheet must be shared as *Anyone with the link can view* for this route). Click **Start matching**.
3. Click **Download new mapping** to get `PS_Result.xlsx`.
4. In the Sheet: **[PS Tools] → Append new mapping to AllHashTable** and choose `PS_Result.xlsx`.

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

**Saving and downloads**

| Button | What it does |
|---|---|
| **Save to AllHashTable** | Appends the new mappings to `AllHashTable` through the Web App. Needs the connection. Existing entries are never overwritten. |
| **Download new mapping** | `PS_Result.xlsx`: sheet **New mappings** (`Input` \| `Standard name`) first, then sheet **Result** (every row of your list with its standard name, status and the three ticks) |
| **Download updated AllHashTable** | `PS_Mapping_updated.xlsx`: `AllHashTable` with the new mappings added, plus `香港小學名單` |

*New mappings* contains the rows you **confirmed**, plus the Auto rows whose exact spelling was not yet in `AllHashTable`. **Skipped** rows are not added. All names are trimmed of leading and trailing spaces.

> Progress is not saved. Click **Save to AllHashTable** (or download your files) before closing the page.

## Rules (optional: `rules.js`)

Put your `AISearch` function in a file called `rules.js` next to `index.html`. When the page loads it, the top of the page shows "Rules file loaded.".

- A rule hit appears as a **100% "rule"** suggestion and is pre-filled, but stays in **Review**, so you still confirm it (or use **✔ Confirm all mapped**).
- If a rule returns a `? ...` list, all the choices are shown and none is pre-filled.
- If a rule returns a name that is not in your standard list, it is only used when it closely matches a listed name.
- Loading adjusts the file automatically: Latin keywords of **4 letters or fewer must match a whole word** (so `SH` no longer matches *Shek Kip Mei*), the `##` prefix is removed, the call to `lookupFromAllHashTable` is skipped (the page already does the lookup), and a few known mistakes are corrected (`SKHSJPS`, `HKMLCPS` and the *Kei Wan* brackets).

## Google Sheet menu: `[PS Tools]`

| Item | What it does |
|---|---|
| **Update PS list** | Shows a link that opens the tool already connected to this Sheet, and a **Copy link** button. Creates the access token automatically the first time. |
| **Export unmatched schools** | *(old route)* Pop-up with 3 steps: download a CSV of the unique schools in `AdmInfoDay(Responses)` that are not in `AllHashTable`; open the tool and upload that CSV; append the result |
| **Append new mapping to AllHashTable** | *(old route)* You choose `PS_Result.xlsx`. It checks that the sheet **New mappings** exists with the headers `Input` \| `Standard name`, then shows how many rows are new, already there with the same name, already there with a **different** name, or repeated in the file. It appends only the new ones. A second upload of the same file adds nothing. |
| **Reset access token** | Replaces the access token. All saved links stop working. Use it if the tool link was shared by mistake, then open **Update PS list** again. |
| **Help** | Short guide and a link to this page |

Existing entries are never overwritten. Only columns **A and B** are written, below the last row that has something in column A, and **column C (your VLOOKUP) is never touched**.

## Setup

**The tool (GitHub Pages)**
1. Upload `index.html` (and `rules.js` if you use it) to the repository.
2. In **Settings → Pages**, publish the branch.

**The sheet code (Apps Script), once per spreadsheet**
1. In the Google Sheet, open **Extensions → Apps Script**.
2. Paste `AppsScript.gs` into the code file.
3. Add an HTML file named exactly **Upload** (**+ → HTML**) and paste `Upload.html` into it.
4. Check the settings at the top of `AppsScript.gs`: `SRC_SHEET`, `SRC_COL` (column of the school name; A = 1), `HASH_SHEET`, `SCH_SHEET`, `TOOL_URL`, `HELP_URL`.
5. **Deploy → New deployment → Web app**. Set **Execute as: Me** and **Who has access: Anyone**. Approve the permissions.
6. Reload the Google Sheet and use **[PS Tools] → Update PS list**. The token is created automatically.

**After changing the script later:** use **Deploy → Manage deployments → Edit (pencil) → Version: New version → Deploy**. This keeps the same URL. Do not create a new deployment each time, and archive old ones you no longer use.

**New copy of the spreadsheet (File → Make a copy):** the copy has its own script. Repeat the setup above, including the deployment. Script Properties (the token) are not copied.

## Privacy and security

- Matching runs in your browser. The student list is not uploaded to GitHub.
- In connected mode the tool talks only to **your own Web App**, which reads and writes **your own Sheet**. The Sheet itself can stay private.
- The Web App must be deployed with *Who has access: Anyone*, so a **secret token** protects it. Every request must carry the token, otherwise it is rejected (*Unauthorized*).
- The token is stored in the Apps Script project (**Project Settings → Script Properties → `PS_TOKEN`**). It is **not** in this repository. Your browser keeps a copy (local storage for the tool site) after you open the link from **Update PS list**. A new browser or cleared site data needs the link once.
- **Treat the link from *Update PS list* like a password.** If it is shared by mistake, use **[PS Tools] → Reset access token**.
- The page is public, but it contains no data and no token.
- `rules.js` is public if it is in the repository, so keep only school names and keywords in it.
- Do not put student files in this repository.

## Troubleshooting

| Problem | What to try |
|---|---|
| Red "Not connected" message | Open the tool from **[PS Tools] → Update PS list** |
| *Failed to fetch* when connected | The Web App cannot be reached. Check **Manage deployments**: the deployment must be active, *Who has access* must be **Anyone**, and the latest version must be deployed. Then reopen the tool from **Update PS list** |
| *Unauthorized* | The saved token is old (for example after **Reset access token**). Reopen the tool from **Update PS list** |
| *All schools ... are already in AllHashTable* | Nothing new to map |
| The old route cannot read the sheet | Share it as *Anyone with the link can view*, or choose a mapping file under **Settings** |
| The page looks old after an update | Hard refresh: **Ctrl+F5** (Windows) or **Cmd+Shift+R** (Mac) |
| No "Rules file loaded." message | `rules.js` must be in the same folder as `index.html`. Open the browser console for details |
| *Sheet "New mappings" was not found* | Use the `PS_Result.xlsx` downloaded with **Download new mapping** |
| *Nothing to append* | Those mappings are already in `AllHashTable` |
| Ticks are wrong for a school | Set column D (`HK`, `International`, `China`, `Oversea`) for that school in `香港小學名單` |
| A school never matches | Use **🔍 Google**, pick the correct name and **✓ Confirm**; it will match automatically next time |
| New rows in `AllHashTable` have no type | Make sure your column C VLOOKUP covers the new rows |

## Credits

Code written by Claude Sonnet 5.5 (Anthropic), version 2026-11-10.
Requested and maintained by <Ada@Claude / Primary School Mapping**>
