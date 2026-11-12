## 📌 **[The most updated Apps in Spreadsheet](https://docs.google.com/spreadsheets/d/1uJLG4HvhBxKqWsidXGVqhX28Bo0RGYk_4ZO8P3040pA/edit?gid=1214948461#gid=1214948461)**

# Timetable PDF → Google Sheets

A Google Apps Script bound to one spreadsheet (for example **2026-27 Timetable**). It reads the **Class PDF** and the **Teacher PDF** in a pop-up window and puts everything in Google:

- **This spreadsheet** gets five sheets: `Teacher DB`, `DB Summary`, `Class Lessons`, `Teacher Lookup`, `Notes`.
- **Two other Google Spreadsheets** are created for the class timetables (one sheet per class) and the teacher timetables (one sheet per teacher).
- **Free lesson attendance** is written to its own sheet from a second window.

The PDFs are read in your browser and are **not uploaded to Google**. Only the finished rows are sent to your spreadsheet.

---

## Install (once)

1. Open the spreadsheet that should hold the results.
2. **Extensions ▸ Apps Script**.
3. Replace the contents of `Code.gs` with the `Code.gs` from this folder.
4. Click **＋ ▸ HTML** and create four HTML files with **exactly these names**, pasting the matching file into each:

   | Name to type | Paste the file |
   |---|---|
   | `Import` | `Import.html` |
   | `Free` | `Free.html` |
   | `Core` | `Core.html` |
   | `Convert` | `Convert.html` |

5. Click **Save**, then reload the spreadsheet. A **Timetable** menu appears, with three items: **Import the two PDFs…**, **Free lesson attendance…** and **README (how to use)**. The last one opens this guide on GitHub.
6. The first time you use it, Google asks you to authorise the script. It needs to edit this spreadsheet, create spreadsheets, and **see and move files in your Google Drive** (to save the two timetable spreadsheets in a folder). Click **Review permissions**, choose your account, then **Advanced ▸ Go to … (unsafe)** and **Allow**. The warning appears because the script is your own and has not been published.

## Use

### Timetable ▸ Import the two PDFs…

1. The window first checks this spreadsheet. If any of its result sheets already exist, a **warning lists them and says they will be overwritten**. Hidden ones are unhidden.
2. **Step 1:** choose the **Class PDF**, then the **Teacher PDF**. Both are required.
   - **File-name check (a warning):** the Class PDF's name should contain “Class” (for example `2627_T1_Class_v9.pdf`) and the Teacher PDF's name should contain “Teacher”. If a name does not match, a warning appears under that box, and it also says so when the two files look swapped. You can still import.
   - **Content check (stops the import):** the Class PDF must show class names such as S1H on its pages, and the Teacher PDF must show names such as “CHAN Clara (Chinese)”. If not, the import stops with a message before anything is written.
3. **Step 2: Class and teacher timetables.** The box is ticked by default; untick it to refresh only the five sheets in this spreadsheet.
   - **Save in:** by default the **same folder as this spreadsheet**. Choose **Another folder** to browse your My Drive folders (click a folder to open it, then **Use this folder**) or to paste a folder link.
   - **File names:** proposed from the PDF names, for example `2627_T1_Class_v9_class_timetables` and `2627_T1_Teacher_v9_teacher_timetables`. You can edit them.
   - **If a spreadsheet with that name is already in the folder**, a warning asks you to choose: **Overwrite it** (the same file is reused and all its sheets are replaced) or **Keep it as `<name>_old1`** (then `_old2`, and so on) **and create a new file**.
4. Click **Import** (the button reads **Import and overwrite** when something will be overwritten).
5. The log shows the checks (pages, entries, rows, pages with no lessons in red), a **cross-check of the two PDFs** (they should agree on every lesson row), and links to the two new spreadsheets.
6. At the end the log shows big, highlighted lines: **Done: 36 sheets in …**, **Done: 108 sheets in …** and **All done.** The **two links to the new spreadsheets** (class timetables, then teacher timetables) come right after "All done." Click one to open it.
7. Click **Open Free lesson attendance ▶** to continue.

### Timetable ▸ Free lesson attendance…

Reads the `Class Lessons` sheet. Choose the levels (S4–S6 by default, with Select all / Unselect all), map the electives of S5C / S6A if needed, and click the write button. If the sheet `Free lesson attendance` already exists, the window warns that it will be overwritten (and unhidden if hidden). If there are warnings about the mapping you are asked to confirm. When the sheet is written, a highlighted line says **Done: 1194 rows written to the sheet “Free lesson attendance”.**

## Sheets

| Where | Sheet (in this order) | Contents |
|---|---|---|
| This spreadsheet | `Teacher DB` | One row per teacher, day and period (lessons, meetings, support). It already contains every lesson row of the class data (checked on your PDFs) |
| | `Class Lessons` | One row per lesson per class, as printed in the Class PDF (used by Free lesson attendance) |
| | `Free lesson attendance` | Subject, Day, Period (written from the second window; it takes its place in the order when it is created) |
| | `Teacher Lookup` | Teacher code, name, department |
| | `DB Summary` | Rows with no venue/subject of their own, by subject (formulas over `Teacher DB`) |
| | `Notes` | The rules, the sheets, and the PDF cross-check |
| Class timetables spreadsheet | `S1H`, `S1K` … | Blue table (subject) and green table (subject / teachers / venue) |
| Teacher timetables spreadsheet | `CSC`, `AF` … | Subject, classes and venue in each cell |

The six sheets are placed at the front of the spreadsheet in the order above after every import and after Free lesson attendance is written.

**Overwriting:** the five sheets written by the import, and `Free lesson attendance`, are cleared and rewritten (including any merged cells); names never change and hidden sheets are unhidden. Sheets the script does not write (for example `Sheet1`) are left alone.

## If pdf.js does not work in the pop-up

Open **pdf.js not working? Import the Excel files…** in the same window and choose the Excel files exported by the GitHub tool (teacher timetables, teacher database, class timetables, class database; the class database is needed for `Class Lessons`). The same sheets and spreadsheets are written.

## Time and limits

- The two timetable spreadsheets hold **36 + 108 sheets**, written six at a time. This takes **several minutes** in total (an estimate: it could not be timed on Google's servers). Each batch is a separate server call, so the 6-minute limit of a single run does not apply to the whole import. Keep the window open until it says **All done**.
- A faster way exists (let Google convert an Excel file into a spreadsheet using the Drive API service), but it needs one extra setup step and is not used here.
- Each database sheet is sent in its own call, big sheets are written in blocks of 500 rows, and a failed write is retried (3 attempts on the server, and 3 more in the window) before an error is shown.
- If the window is closed half-way, run the import again. Choose **Overwrite** to reuse the same files.
- While the window is open you cannot edit the spreadsheet.
- The folder browser lists your **My Drive** folders only (the first 300 in each). For a folder in a Shared Drive, paste its link.

## Troubleshooting

| Problem | What to do |
|---|---|
| No **Timetable** menu | Reload the spreadsheet after saving the script |
| "Authorisation required" | Complete the authorisation steps above, including Drive access |
| `Import` / `Free` / `Core` / `Convert` not found | The HTML file names must match exactly |
| The window says pdf.js could not be loaded | Use the Excel files option; check that your organisation allows cdnjs.cloudflare.com |
| "Class Lessons was not found" in the Free window | Run the import first |
| Import stays disabled | Choose both PDFs, and (if step 2 is ticked) a folder and two different file names |
| A warning under a PDF box | The file name does not contain “Class” / “Teacher”, or the files look swapped. Check you chose the right file in each box |
| “The Class PDF does not look like a class timetable…” | The files are in the wrong boxes, or the PDF is not the aSc class-timetable export. Nothing was written |
| `Problem: The sheet “…” could not be written (step: …)` or Google's message “Service Spreadsheets failed while accessing document…” | Google's spreadsheet service refused a write. The script already retries each sheet three times and writes big sheets in blocks of 500 rows. Click **Import** again (it overwrites what was written). If it stops at the same sheet and step again, send the Problem line. Also check that the spreadsheet is not very large or slow (many other sheets, IMPORTRANGE formulas) |
| A teacher page has no code | The page has no lessons; it is shown in red in the log and has no rows |

## What was tested

The pop-up code and `Code.gs` were run against a **simulation** of Google Apps Script, Google Sheets and Google Drive, using your real PDFs: both PDFs through to the five sheets and the two timetable spreadsheets (36 and 108 sheets), text columns kept as text, formulas pointing at `Teacher DB`, existing and hidden sheets (warning, overwrite, unhide), overwrite or `_old1` / `_old2` for existing files, the same folder, another folder (browser and pasted link), the file-name warnings and the content check that stops a swapped import, re-running the import on the same spreadsheet (including the merged title of `DB Summary`), the tab order, the optional tick box, the Free lesson attendance window with its warning and confirmation, and the Excel fallback (identical to the PDF route). It was **not** run in a real Google account: whether pdf.js loads inside Google's pop-up, how the Drive calls behave with your folders, and the real speed still need to be checked by you.
