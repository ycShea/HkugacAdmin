# PS Mapping

🔗 **[Click here to access the Live Tool](https://ycshea.github.io/HkugacAdmin/PSMapping/)**

🔗 **[PS Mapping (Most Updated)](https://docs.google.com/spreadsheets/d/1-tcFI4oAFt4nPBKSfCQcwHwCGd8QQJnW8eFmSBeVYjc/edit?gid=450436726#gid=450436726)**

A small web tool that turns the primary school names typed by families (spelling mistakes, abbreviations, Chinese or English) into one **standard school name**. It uses the mapping table kept in a Google Sheet and helps you add new mappings back to it.

## What is in this repository

| File | What it is |
|---|---|
| `index.html` | The web tool (runs entirely in the browser) |
| `AppsScript.gs` | Menu code for the Google Sheet (`[PS Tools]`) |
| `Upload.html` | The upload box used by the menu item *Append new mapping to AllHashTable* |
| `README.md` | This guide |

## The Google Sheet

| Sheet | Content |
|---|---|
| `AllHashTable` | Column A **Input** (a spelling seen before) and column B **Unified** (the standard name) |
| `香港小學名單` | The list of standard school names, with abbreviation and district |
| `AdmInfoDay(Responses)` | The form responses; column A holds the school name typed by the family |

The tool reads `AllHashTable` and `香港小學名單`. For the tool to read the sheet, it must be shared as **Anyone with the link can view**.

## Typical workflow

1. In the Google Sheet, click **[PS Tools] → Export unmatched schools**. This lists the schools in `AdmInfoDay(Responses)` that are not yet in `AllHashTable`. Click **Download CSV**.
2. Open the **[Live Tool](https://ycshea.github.io/HkugacAdmin/PSMapping/)**.
3. Open **Settings**, paste the Google Sheet URL (it is remembered in your browser), choose the CSV as the student list and click **Start matching**.
4. Go through the rows (see below), then click **Download new mapping**. This gives you `PS_Result.xlsx`.
5. Back in the Google Sheet, click **[PS Tools] → Append new mapping to AllHashTable** and choose `PS_Result.xlsx`.

## Using the tool

**Statuses**

| Status | Meaning |
|---|---|
| **Auto 100%** | The input matches `AllHashTable` (exactly, or apart from capitals, spaces and punctuation). Nothing to do. |
| **Review** | No exact match. The tool suggests up to three standard names with a score and pre-fills the best one if it is reasonably close. |
| **Confirmed / Skipped / Blank** | Rows you have dealt with. Use **Undo** to change your mind. |

Rows with a pre-filled suggestion are listed first, strongest first.

**Buttons on a Review row**

| Button | What it does |
|---|---|
| **✓ Confirm** | Accepts the name in the box and adds the mapping to the new mappings |
| **🔍 Google** | Opens a Google search for the input in a new tab |
| **Skip** | Uses the input itself as the name; the row is removed from the list |
| **Blank** | Leaves the standard name empty; the row is removed from the list |
| **⧉ Copy** | Copies the input into the name box so you can edit it and use type-to-search |
| **✕ Clear** | Empties the name box |
| **✔ Confirm all mapped (N)** | Confirms every Review row that has a name in its box. It disappears when there are none. |
| **↑ Top** | Scrolls back to the top |

The **Standard name** box has type-to-search over all standard names. Click a suggestion button below it to fill the box.

**Ticks:** *International*, *Mainland* and *Overseas* are pre-ticked from keywords and the district column. They are only suggestions, so please check them.

**Downloads**

| Button | File | Content |
|---|---|---|
| **Download new mapping** | `PS_Result.xlsx` | Sheet **New mappings** (`Input` \| `Standard name`) first, then sheet **Result** (every row of your list with its standard name, status and the three ticks) |
| **Download updated AllHashTable** | `PS_Mapping_updated.xlsx` | `AllHashTable` with the new mappings added, plus `香港小學名單` |

*New mappings* contains the rows you confirmed and the Auto rows whose spelling was not yet in `AllHashTable`. **Skip** and **Blank** rows are not added to the mapping.

> Progress is not saved. Download your files before closing the page.

## Google Sheet menu: `[PS Tools]`

| Item | What it does |
|---|---|
| **Export unmatched schools** | Downloads a CSV of the unique schools in `AdmInfoDay(Responses)` that are not in `AllHashTable`, and shows the link to the tool |
| **Append new mapping to AllHashTable** | You choose `PS_Result.xlsx`. It checks that the sheet **New mappings** exists with the headers `Input` \| `Standard name`, shows how many rows are new, already there, or already there with a different name, and appends only the new ones. A second upload of the same file adds nothing. |
| **Help** | Short guide and a link to this page |

Existing entries are never overwritten.

## Setup

**The tool (GitHub Pages)**
1. Upload `index.html` to the repository.
2. In **Settings → Pages**, publish the branch.

**The sheet menu (Apps Script)**
1. In the Google Sheet, open **Extensions → Apps Script**.
2. Paste `AppsScript.gs` into the code file.
3. Add an HTML file named exactly **Upload** (**+ → HTML**) and paste `Upload.html` into it.
4. Check the settings at the top of `AppsScript.gs`: `SRC_SHEET`, `SRC_COL` (column of the school name; A = 1), `HASH_SHEET`, `TOOL_URL`, `HELP_URL`.
5. Save and reload the Google Sheet. The first time, Google asks you to approve permissions.

## Privacy

- The tool runs in your browser. The student list is not uploaded to GitHub or to any server.
- The page is public, but it contains no data. The Google Sheet address is entered in Settings and stored in your own browser only.
- Do not put student files in this repository.

## Troubleshooting

| Problem | What to try |
|---|---|
| The sheet cannot be read | Share it as *Anyone with the link can view*, or choose a mapping file under **Settings** |
| The page looks old after an update | Hard refresh: **Ctrl+F5** (Windows) or **Cmd+Shift+R** (Mac) |
| *Sheet "New mappings" was not found* | Use the `PS_Result.xlsx` downloaded with **Download new mapping** |
| *Nothing to append* | Those mappings are already in `AllHashTable` |
| A school never matches | Use **🔍 Google**, pick the correct name and **✓ Confirm**; it will match automatically next time |
