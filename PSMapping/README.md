# PS Mapping

🔗 **[PS Mapping (Most Updated)](https://docs.google.com/spreadsheets/d/1uz5Um7unTOxC40xlvNtxHZWX2y34kfBo-MQDKGq28TA/edit?gid=976826844#gid=976826844)**

🔗 **[Click here to access the Live Tool](https://ycshea.github.io/HkugacAdmin/PSMapping/)**

A small web tool that turns the primary school names typed by families (spelling mistakes, abbreviations, Chinese or English) into one **standard school name**. It reads the mapping table from the Google Sheet, helps you decide the unclear cases, and saves the new mappings back to the sheet.

## How to use

1. Click **Start matching**. The tool loads the schools in `AdmInfoDay(Responses)` that are not yet in `AllHashTable`.
2. Check each row: click **✓ Confirm**, or **✔ Confirm all mapped** (bottom right). Use **🔍 Google** if unsure, **⏭ Skip** to leave a row out, and **🚫 Not a School** for entries such as `N/A`.
3. Click **Save to AllHashTable**.
4. Go back to the Google Sheet. The new rows are at the bottom of `AllHashTable` (and `Not a School`).

The first time, or after making a copy of the spreadsheet, you must set the Web app URL and the Token once. See [First-time setup](#first-time-setup-once-for-each-copy-of-the-spreadsheet).

## What is in this repository

| File | Where it goes | What it is |
|---|---|---|
| `index.html` | GitHub Pages (or open it from your computer) | The tool |
| `rules.js` | Same folder as `index.html` (optional) | Your `AISearch` rules, used as an extra matching step |
| `AppsScript.gs` | Google Sheet > Extensions > Apps Script | The `[PS Tools]` menu and the web app |
| `WebAppUrl.html` | Apps Script, HTML file named exactly `WebAppUrl` | The dialog behind **Show Web app URL** |
| `Upload.html` | Apps Script, HTML file named exactly `Upload` | The upload box of the manual route |
| `README.md` | Repository | This guide |

## The Google Sheet

| Sheet | Content |
|---|---|
| `AllHashTable` | **A** = Input (a spelling seen before), **B** = Unified (the standard name), **C** = type (your VLOOKUP; the tool never touches it) |
| `香港小學名單` | The standard names. **A** = name, **B** = abbreviation, **C** = district, **D** = type: `HK`, `International`, `China` or `Oversea` |
| `AdmInfoDay(Responses)` | The form responses. Column A holds the school name typed by the family |
| `Not a School` | Names that are not a school (`N/A`, `1`, `Name of Child` ...). Column A, from row 2 |

The sheet names and the column of the school name are settings at the top of `AppsScript.gs`.

## How it works

- The tool (on GitHub Pages) sends requests to a **web app** that lives inside your Google Sheet. Each request carries a secret **Token**.
- The sheet stays **private**. The web app sends back only the school names to match, the mapping table, the standard school list and the `Not a School` names. Nothing else from the responses sheet leaves the spreadsheet.
- Every copy of the spreadsheet has its own script, its own **Web app URL** and its own **Token**. Pasting a different URL in the tool's Settings switches to a different copy.

## First-time setup (once for each copy of the spreadsheet)

1. Open the Google Sheet and go to **Extensions > Apps Script**.
2. Paste `AppsScript.gs`, then add two HTML files (**+ > HTML**) named exactly `Upload` and `WebAppUrl`, and paste `Upload.html` and `WebAppUrl.html` into them. (A copy of the spreadsheet already contains them.)
3. Reload the Google Sheet. A menu **[PS Tools]** appears. The first time, Google asks you to approve permissions.
4. Click **[PS Tools] > Show Web app URL**. The dialog shows the steps, with pictures of the buttons to look for:
   - **First time only:** Deploy > New deployment > Web app, Execute as **Me**, Who has access **Anyone**, Deploy, approve the permissions, then copy the **Web app URL**.
   - **Get the URL again later:** Deploy > Manage deployments, then copy the URL.
   - Paste the URL in the dialog and click **Save**. It must end with `/exec`. The school-domain forms (`/a/<domain>/...`) are converted automatically.
5. In the same dialog, type a secret word (at least 6 characters) under **Token** and click **Set the token**. Write it down: it cannot be shown again.
6. Open the **[Live Tool](https://ycshea.github.io/HkugacAdmin/PSMapping/)**, open **Settings: Google Sheet (saved in this browser only)**, paste the URL and the Token, and click **Start matching**.

**Token.** It is one shared secret word, not a login for each person. Anyone who has the Web app URL and the Token can read the school names and add rows, so share it only with your team. To cut off access, click **Reset token** in the dialog and set a new one. Everyone then has to type the new Token in their own Settings.

**After changing the code** in Apps Script, deploy a new version with **Deploy > Manage deployments > pencil > New version**. Do not choose *New deployment*, because the URL would change.

**A new copy of the spreadsheet** must be deployed again and needs its own Token. If **Show Web app URL** already says "Token: Set" and you did not set it, click **Reset token** and set your own.

## The Settings box in the tool

The line **Settings: Google Sheet (saved in this browser only)** is always on the page.

| Situation | Settings |
|---|---|
| URL and Token are saved | Folded |
| URL or Token is missing | Open |
| **Start matching** fails (wrong URL, wrong Token, no connection) | Opens, with the error |
| The web app answers but the sheet setup is wrong (for example a renamed tab) | Stays folded, with the error |
| Everything works | Folds |

## How the tool matches

Each name goes through these steps and stops at the first one that finds it:

1. **Exact** match in `AllHashTable` (ignoring capitals, spaces and full-width characters).
2. **Normalised** match in `AllHashTable` (also ignoring punctuation, simplified vs traditional Chinese and common words such as *Primary School*; typos in those words, such as *Schiol*, are fixed).
3. **Abbreviation** from column B of `香港小學名單`.
4. **Rules** from `rules.js`, if present.
5. **Fuzzy matching** against all standard names: similarity of the name, matching of words, and initials (`HPCC Hoi Ping` finds *Hoi Ping Chamber Of Commerce*).

Steps 1 and 2 are shown as **Auto 100%**. Everything else is a suggestion for you to confirm. Names in `Not a School` are left out of the list from the start.

## Using the tool

**Statuses**

| Status | Meaning |
|---|---|
| **Auto 100%** | Found in `AllHashTable`. Nothing to do |
| **Review** | A suggestion or none. Up to three suggestions are shown with a score and the best one is pre-filled if it is close enough. Rows with a pre-filled name come first, strongest first |
| **Confirmed** | You accepted the name |
| **Skipped** | You skipped the record |
| **Not a School** | You marked it as not a school name |

A summary line below the buttons shows how many rows are in each status. Use **Show** to list only the rows that need review, or all rows.

**Buttons on a Review row** (hover over a button to see what it does)

```
✓ Confirm (blue)           🔍 Google
🚫 Not a School (orange)   ⏭ Skip
⧉ Copy                     ✕ Clear
```

| Button | What it does |
|---|---|
| **✓ Confirm** | Accepts the name in the box and adds the mapping to the new mappings |
| **🔍 Google** | Opens a Google search for the input in a new tab |
| **🚫 Not a School** | The input is not a school name. It is saved to `Not a School` and not asked again |
| **⏭ Skip** | Skip for now. Nothing is saved |
| **⧉ Copy** | Copies the input into the name box so you can edit it and use type-to-search |
| **✕ Clear** | Empties the name box |
| **Undo** | Shown on finished rows. Puts the row back to Review |
| **✔ Confirm all mapped (N)** | Floating button. Confirms every Review row that has a name in its box. It disappears when there are none |
| **↑ Top** | Floating button. Scrolls back to the top |

The **Standard name** box has type-to-search over all standard names. Click a suggestion under it to fill the box; a suggestion marked *rule* came from `rules.js`.

The three tick columns (*International*, *Mainland*, *Overseas*) come from column D of `香港小學名單` and are only there for your reference. They are not saved. The type in `AllHashTable` comes from your VLOOKUP.

**Buttons above the table**

| Button | What it does |
|---|---|
| **Save to AllHashTable** | Writes the confirmed mappings to `AllHashTable` and the `Not a School` names to the sheet `Not a School`. Existing entries are never overwritten, and a name already in `AllHashTable` is not added to `Not a School`. Shows how many rows were added, already there, or already there with a different name |
| **Download new mapping** | Downloads `PS_Result.xlsx`: only the **new** rows, in two sheets: `AllHashTable` (`Input` \| `Standard name`) and `Not a School` (`Input`) |
| **Download updated AllHashTable** | Downloads `PS_Mapping_updated.xlsx`: a full copy with `AllHashTable` (existing and new rows), `香港小學名單` and `Not a School` |

The new mappings are the rows you confirmed, plus the Auto rows whose exact spelling was not yet in `AllHashTable`. All names are trimmed of leading and trailing spaces.

> Progress is not saved. Click **Save to AllHashTable** or download a file before you close the page.

## Manual route (upload a file)

Open **Manual route: upload a file instead (click to show)** in the tool. It uses the same matching, the same Settings and the same buttons. Only the list of names comes from a file.

1. In the Google Sheet, click **[PS Tools] > (Manual download) Export unmatched schools** and click **Download CSV**. The list leaves out the names in `Not a School`.
2. In the tool, choose the file and click **Start matching (file)**. Names already in `Not a School` are left out, and a line tells you how many.
3. Check the rows. Then either click **Save to AllHashTable**, or click **Download new mapping**.
4. If you downloaded the file, click **[PS Tools] > (Manual upload) Append new mapping to AllHashTable** (or Step 3 of the Export dialog) and choose `PS_Result.xlsx`. The box checks the file and tells you how many rows are new, already there, already there with a different name, or repeated, for both sheets, before anything is written. Uploading the same file twice adds nothing.

## Google Sheet menu: `[PS Tools]`

| Item | What it does |
|---|---|
| **Show Web app URL** | The dialog for the Web app URL and the Token: a big link to the tool, the saved URL with **Copy URL** and **Change URL**, **How to get the Web app URL**, and the Token with **Set the token** and **Reset token** |
| **(Manual download) Export unmatched schools** | A dialog with three steps and a **Download CSV** button |
| **(Manual upload) Append new mapping to AllHashTable** | Appends `PS_Result.xlsx` to `AllHashTable` and `Not a School` |
| **Help** | A short guide and a link to this page |

Only columns **A and B** of `AllHashTable` (and column **A** of `Not a School`) are written, below the last row that has something in column A. Column C (your VLOOKUP) is never touched.

## Rules (optional: `rules.js`)

Put your `AISearch` function in a file called `rules.js` next to `index.html`. When the page loads it, the top of the page shows "Rules file loaded.".

- A rule hit appears as a **100% "rule"** suggestion and is pre-filled, but stays in **Review**, so you still confirm it.
- If a rule returns a `? ...` list, all the choices are shown and none is pre-filled.
- If a rule returns a name that is not in your standard list, it is only used when it closely matches a listed name.
- When the file is loaded, Latin keywords of **4 letters or fewer must match a whole word**, the `##` prefix is removed, the call to `lookupFromAllHashTable` is skipped, and a few known mistakes are corrected (`SKHSJPS`, `HKMLCPS` and the *Kei Wan* brackets).
- `rules.js` may not load when `index.html` is opened from a folder on your computer, because browsers block it.

## Privacy

- The page is public, but it contains no data. The Web app URL and the Token are typed in Settings and stored only in your own browser.
- The sheet is not shared. Only the web app, which needs the Token, can read it.
- `rules.js` is public if it is in the repository, so keep only school names and keywords in it.
- Do not put student files in this repository.

## Troubleshooting

| Problem | What to try |
|---|---|
| "Please enter the Web app URL and the Token below." | Open Settings and enter both |
| "Cannot reach the web app" or "Unauthorized" | Check the URL ends with `/exec`. In Apps Script, deploy the web app with **Execute as: Me** and **Who has access: Anyone**, deploy the latest version, and paste the URL of **this** copy of the spreadsheet |
| "Wrong token." | Type the Token again, or reset it in **Show Web app URL** and enter the new one in Settings |
| "token not set in this spreadsheet" | Set a Token in **[PS Tools] > Show Web app URL** |
| "Sheet not found" | Check the sheet names at the top of `AppsScript.gs` (`SRC_SHEET`, `HASH_SHEET`, `SCHOOL_SHEET`, `NOT_SCHOOL_SHEET`) |
| Saving says the script is an older version | Paste the latest `AppsScript.gs` and deploy a new version |
| "Nothing to match" | Every school in `AdmInfoDay(Responses)` is already in `AllHashTable` or `Not a School` |
| The page looks old after an update | Hard refresh: **Ctrl+F5** (Windows) or **Cmd+Shift+R** (Mac) |
| The append box says "Sheet AllHashTable was not found" | Use the `PS_Result.xlsx` from **Download new mapping** |
| "Nothing to append" | Those mappings and names are already in the sheets |
| Ticks are wrong for a school | Set column D (`HK`, `International`, `China`, `Oversea`) for that school in `香港小學名單` |
| A school never matches | Use **🔍 Google**, pick the correct name and **✓ Confirm**. It will match automatically next time |
