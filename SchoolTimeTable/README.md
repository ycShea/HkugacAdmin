# Timetable PDF → Excel

🔗 **[Click here to access the Live Tool](https://ycshea.github.io/HkugacAdmin/SchoolTimeTable/)**

A single-page web tool that turns the PDF timetables exported from **aSc Timetables** into Excel files:

- **Teacher Timetable PDF → Excel** (one page per teacher)
- **Class Timetable PDF → Excel** (one page per class)

Everything runs in your browser. The PDF is read on your computer and is **not uploaded anywhere**.

---

## Quick start

1. Open the [live tool](https://ycshea.github.io/HkugacAdmin/SchoolTimeTable/). It opens on the **Teacher** tab; the **Class** tab is next to it.
2. **Step 1:** drop in the PDF (or click to choose it).
3. **Step 2 (teacher list):** nothing to do in most cases. See [Teacher list (CSV)](#teacher-list-csv).
4. **Step 3:** check the summary, then download the Excel files.

## What you get

| Tab | Button | File saved as | Contents |
|---|---|---|---|
| Teacher | Download teacher timetables | `<pdf name>_teacher_timetables.xlsx` | One sheet per teacher (sheet name = teacher code) |
| Teacher | Download teacher database | `<pdf name>_database.xlsx` | One row per teacher / day / period |
| Class | Download class timetables | `<pdf name>_class_timetables.xlsx` | One sheet per class, with two tables (see below) |
| Class | Download class database | `<pdf name>_database.xlsx` | One row per teacher / day / period |

The page shows the exact file names under the buttons.

### Timetable workbooks

Each sheet has days `A1`–`B5` across the top and periods `1`–`7` down the side.

- **Class timetables:** the table at the top (rows 1–8) shows the subject. A second table in **rows 11–18** has the same structure with more detail. Each cell holds the subject, the teachers and the venue on separate lines, for example `ICT` / `LG / MH` / `207A`. Double periods fill both periods.
- **Teacher timetables:** each cell holds the subject, the classes and the venue on separate lines. Cell A1 is the teacher code and the full name is in A10. If a teacher has two lessons in the same period, both are shown.

### Database workbooks

The first columns are:

| Teacher | Day | Period | Class | Venue | Co-teaching (Y or N) |
|---|---|---|---|---|---|

- The **teacher** database adds **Lesson/Meeting/Support** after Co-teaching.
- Extra reference columns follow: Subject, Time, Teachers sharing this venue, Pairing, Teacher Name.
- A double period is two rows (one per period).
- In the class database, rows that differ only by class are combined, for example `S2H/S2K/S2U`.
- A `Teacher Lookup` sheet and a `Notes` sheet are included.

**Lesson / Meeting / Support (teacher database):** a card with classes is a *Lesson*. A card whose name contains "Meeting" is a *Meeting*. A card whose name contains "SUPPORT" is *Support*. For meetings and support the Subject column holds the name printed on the card.

---

## How teachers, venues and subjects are matched

Where a lesson lists several teachers, venues and subjects, they are matched **by position**.

> `AF / HC / SYC` with venues `601,602,603` means AF uses 601, HC uses 602 and SYC uses 603.
> Subjects work the same way: `CH/BIO` with `TH / NYC` and `604,213` means TH teaches CH in 604, and NYC teaches BIO in 213.

| Situation | What happens |
|---|---|
| Same number of teachers and venues | Matched by position |
| **One** venue (or subject) | Every teacher shares it |
| **More teachers than venues** (or subjects), with more than one venue | The first teachers are matched by position. Each remaining teacher gets the full venue list, a **line break**, then the full teacher list (for example `301,302,303,207B,311,308,409` ⏎ `AH / NC / DW / EL / CK / STC / AYI / CT / HH`). Subjects work the same way (`EC/PEDSE` ⏎ `ALC / EL / HH`). These rows are flagged in the **Pairing** column |
| **Group sessions**: `HD`, `MS`, `CASTLE`, `OPT1`, `OPT2`, `OPT3`… | **Not matched.** Every teacher row shows the full venue list |

The result page lists how many rows have no venue or subject of their own and gives a breakdown by subject.

**Co-teaching** is `Y` when more than one teacher uses the same venue in the same lesson, otherwise `N`. Group sessions are `Y`. For teachers with no venue of their own, the count in "Teachers sharing this venue" is an estimate (at least 2). Meetings and support are `N`.

---

## Teacher list (CSV)

Step 2 on both tabs is one shared list of teacher initials, names and departments, with the columns `Initial,Name,Department`.

- **Reading a Teacher PDF creates the list.** The page reads each teacher's name and department from the page header. The Class tab then uses the list to fill the **Teacher Name** column.
- **Finding the initials.** The PDF shows the full name but not the initials. The initials are the code that appears in every lesson on that teacher's page. Where several teachers always teach together, codes already taken by other pages are removed. A teacher with no lessons has no code, so they are left out and named in the Step 2 message.
- **Your own CSV.** You can choose a CSV instead. It stays as the list, and a Teacher PDF never replaces it.
- **Verification.** Whenever a Teacher PDF is read and a list is already loaded, the PDF is checked against it. The page reports different names or departments, teachers only in the PDF, and teachers only in the list. If a page has no code in the PDF, the code is taken from the list when the name and department match. A list that came from an earlier Teacher PDF is replaced after the check.
- **Buttons.** With no data the button is **Download sample CSV** (headers only). With data it is **Download teacher list (CSV)**.
- **File names.** `2627_teacher_names.csv` and `2627_teacher_names_sample.csv`. The `2627` comes from the PDF name when it starts with four digits (for example `2728_…`); otherwise `2627` is used.

---

## Limitations

- Built and tested for the **HKUGA College 2026–27 Term 1** aSc exports (landscape, a 10-day cycle `A1`–`B5`, 7 periods). The page reads the PDF by text position and font size, so a different print layout may need the layout constants in `index.html` adjusted.
- Free periods have no row.
- Where a teacher has two lessons in the same period, both are listed.
- In grouped lessons (for example EN or MA across several classes) the PDF does not say which teacher takes which class, so each teacher is listed against every class in the group.
- Teacher codes, and the teacher list built from a Teacher PDF, are worked out from the PDF rather than printed in it. Check the Teacher Lookup sheet, or verify against your own CSV.
- The page needs an internet connection to load two libraries from cdnjs.

## Privacy and hosting

- The PDFs stay in your browser.
- **No staff names are stored in `index.html`.** Keep any teacher CSV private. A GitHub Pages site is public, so do not commit a CSV containing staff names to a public repository.
- The repository needs only `index.html`, which is served at `/SchoolTimeTable/`. It uses [pdf.js 3.11.174](https://mozilla.github.io/pdf.js/) and [ExcelJS 4.4.0](https://github.com/exceljs/exceljs) from cdnjs.

## Customising

All settings are at the top of the script in `index.html`:

- `GROUP_RE`: the subject codes treated as group sessions (default `HD`, `MS`, `CASTLE`, `OPT…`).
- `X`, `Y`: column and row edges of the class timetable grid. `TX` is the column edges for teacher pages.
- `DAYS`, `TIMES`: day codes and period times.
