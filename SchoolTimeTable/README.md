# Timetable PDF → Excel

🔗 **[Click here to access the Live Tool](https://ycshea.github.io/HkugacAdmin/SchoolTimeTable/)**

A single-page web tool that turns the PDF timetables exported from **aSc Timetables** into Excel files, one tab for teacher timetables and one for class timetables. From the Class PDF you can also create a third tab, **Free lesson attendance**.

- **Runs in your browser.** The PDF is read on your computer and is **not uploaded anywhere**.
- **No installation.** Open the link, drop in the PDF, download the Excel files.
- **Built for** the HKUGA College 2026–27 Term 1 exports (see [Limitations](#limitations)).

---

## Contents

- [Quick start](#quick-start)
- [What you get](#what-you-get)
- [How teachers, venues and subjects are matched](#how-teachers-venues-and-subjects-are-matched)
- [Free lesson attendance](#free-lesson-attendance)
- [Teacher list (CSV)](#teacher-list-csv)
- [Checks shown on the result page](#checks-shown-on-the-result-page)
- [Limitations](#limitations)
- [FAQ](#faq)
- [Privacy and hosting](#privacy-and-hosting)
- [Customising](#customising)

---

## Quick start

1. Open the [live tool](https://ycshea.github.io/HkugacAdmin/SchoolTimeTable/). It opens on the **Teacher** tab. The **Class** tab is next to it.
2. **Step 1:** drop in the PDF (or click to choose it).
3. **Step 2 (teacher list):** nothing to do in most cases. See [Teacher list (CSV)](#teacher-list-csv).
4. **Step 3:** read the summary, then download the Excel files.
5. **Class PDF only:** a **＋ Free lesson attendance** button appears in the result step, on the right, one line below the two download buttons. See [Free lesson attendance](#free-lesson-attendance).

| If you have… | Use the tab | Input |
|---|---|---|
| The teacher timetables PDF | **Teacher Timetable PDF → Excel** | One page per teacher |
| The class timetables PDF | **Class Timetable PDF → Excel** | One page per class |

---

## What you get

Each tab gives two Excel files. The page shows the exact file names under the buttons. After a Class PDF is read, a third file is available from the [Free lesson attendance](#free-lesson-attendance) tab.

| Tab | Button | File saved as | Contents |
|---|---|---|---|
| Teacher | Download teacher timetables | `<pdf name>_teacher_timetables.xlsx` | One sheet per teacher (sheet name = teacher code) |
| Teacher | Download teacher database | `<pdf name>_database.xlsx` | One row per teacher / day / period |
| Class | Download class timetables | `<pdf name>_class_timetables.xlsx` | One sheet per class, with two tables |
| Class | Download class database | `<pdf name>_database.xlsx` | One row per teacher / day / period |

### Timetable workbooks

Each sheet has the days `A1`–`B5` across the top and the periods `1`–`7` down the side.

**Class timetables.** Each class sheet has two tables with the same structure:

- the **blue table** at the top (rows 1–8) shows the subject;
- the **green table** in **rows 11–18** shows more detail. Each cell holds the subject, the teachers and the venue on separate lines, for example `ICT` / `LG / MH` / `207A`.

Double periods fill both periods in both tables.

**Teacher timetables.** Each cell holds the subject, the classes and the venue on separate lines. Cell A1 is the teacher code and the full name is in A10. If a teacher has two lessons in the same period, both are shown.

### Database workbooks

The first columns are:

| Teacher | Day | Period | Class | Venue | Co-teaching (Y or N) |
|---|---|---|---|---|---|

- The **teacher** database adds **Lesson/Meeting/Support** after Co-teaching.
- Extra reference columns follow: Subject, Time, Teachers sharing this venue, Pairing, Teacher Name.
- A double period is two rows, one per period.
- In the class database, rows that differ only by class are combined, for example `S2H/S2K/S2U`.

Sheets in each database file:

| Sheet | Class database | Teacher database |
|---|---|---|
| `Timetable` | ✔ | ✔ |
| `Summary` (rows with no venue or subject of their own, by subject) | ✔ | ✔ |
| `Class Lessons` (one row per lesson per class, as printed in the PDF) | ✔ | |
| `Teacher Lookup` (code, name, department) | ✔ | ✔ |
| `Notes` (the rules on this page, in short) | ✔ | ✔ |

**Lesson / Meeting / Support (teacher database).** A card with classes is a *Lesson*. A card whose name contains "Meeting" is a *Meeting*. A card whose name contains "SUPPORT" is *Support*. For meetings and support the Subject column holds the name printed on the card.

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
| A lesson drawn side by side in a clash cell (the PDF does not print its teachers) | The teacher list is taken from the same lesson on another teacher's page. If that is not possible, the venue is not matched and the row is flagged |

The result page counts the rows that have no venue or subject of their own and breaks them down by subject. The same table is on the `Summary` sheet of both database files.

**Co-teaching** is `Y` when more than one teacher uses the same venue in the same lesson, otherwise `N`.

- Group sessions are `Y`.
- For a teacher with no venue of their own, the count in "Teachers sharing this venue" is an estimate (at least 2).
- Meetings and support are `N`.

---

## Free lesson attendance

A third tab, built from the **Class PDF**. It gives one row per class, day and period, where the subject is written as `Class-Subject`.

| Subject | Day | Period |
|---|---|---|
| S4A-MA | A1 | 1 |
| S5C-E1 | A1 | 1 |
| S5H-E3 | A1 | 1 |
| S6A-E2 | A1 | 1 |
| S4A-EN | A1 | 2 |

**How to open it.** After a Class PDF is read, a **＋ Free lesson attendance** button appears in step 3 of the Class tab, on the right, one line below **Download class timetables** and **Download class database**. Click it to create the tab. If you read another Class PDF, the tab disappears and the button comes back; click it again to create the tab from the new PDF.

**Steps in the tab**

1. **Levels.** Tick the levels you want. **S4–S6** are ticked by default; you can also tick S1–S3. **Select all** and **Unselect all** next to the heading tick or clear all six levels at once.
2. **Map the electives.** Only shown when needed, see below.
3. **Result.** Read the summary and preview, then click **Download free lesson attendance**.

**The file.** `<pdf name>_free_lesson_attendance.xlsx`, one sheet with the headers `Subject | Day | Period`.

- A double period gives two rows. Free periods give no row.
- Rows are sorted by day (`A1`…`B5`), then period, then class A–Z.

**How the subject is written**

| Subject in the class PDF | Shown as |
|---|---|
| `CHI`, `EN`, `MA`, `CSD`, `HD`, `PE` (also `OPT1`, `OPT2`, `CASTLE`, `MS`) | As printed, for example `S4A-OPT1`, `S6C-CSD` |
| An elective block lesson (S4E2, S5E1, S6E2…) | `E2`, `E1`, `E2`… (the level is dropped) |
| The one other elective of a class that has blocks | The missing number: **E1 in S4** (blocks are E2 and E3), **E3 in S5 and S6** (blocks are E1 and E2) |
| The electives of a class with no blocks (today S5C and S6A) | Chosen by you in the mapping table |
| S1–S3 subjects | As printed, for example `S1H-ICT` |

**E1, E2 and E3** are names for groups of electives: a group can hold one subject or several. Every class in S4–S6 has three elective lines, which are E1, E2 and E3.

**The mapping table.** For a class that has no block lessons, the numbers cannot be worked out from the timetable, so you choose them. There is one table per level, listing the subjects of the classes it applies to. The defaults are **PHY or EC/PHY → E1, CHEM → E2, BIO → E3**. The rows are listed in the order of their default number (PHY or EC/PHY, then CHEM, then BIO). Choose **As printed** to leave a subject unchanged. Your choices are kept while the tab is open.

**Warnings and confirmation.** The page shows a warning, in red, if a subject is left unmapped or if two subjects of one class are given the same number (for example `S5C: EC/PHY and BIO are both mapped to E1.`). If a warning exists when you click **Download free lesson attendance**, the page asks **Download anyway?** and waits for **Yes, download anyway** or **No, go back**. Without warnings the file downloads at once.

---

## Teacher list (CSV)

Step 2 on both tabs is **one shared list** of teacher initials, names and departments, with the columns `Initial,Name,Department` (comma-separated).

- **Reading a Teacher PDF creates the list.** The page reads each teacher's name and department from the page header. The Class tab then uses the list to fill the **Teacher Name** column.
- **Finding the initials.** The PDF shows the full name but not the initials. The initials are the code that appears in every lesson on that teacher's page. Where several teachers always teach together, codes already taken by other pages are removed. A teacher with no lessons has no code, so they are left out and named in the Step 2 message.
- **Your own CSV.** You can choose a CSV instead, and it becomes the list.
- **Verification.** Whenever a Teacher PDF is read and a list is already loaded, the PDF is checked against it. The result shows how many teacher pages match (and how many teachers the list has) and reports:
  - different names or departments;
  - teachers only in the PDF;
  - teachers only in the list;
  - pages with no code that could not be found in the list.

  If a page has no code in the PDF, the code is taken from the list when the name and department match.
- **Replace your CSV? Yes / No.** If the loaded list is a CSV you uploaded, the result asks **"Replace your uploaded CSV with the teacher list from this PDF?"**
  - **Yes, replace it:** the list is replaced with the one from the PDF, on both tabs.
  - **No, keep my CSV:** your CSV stays as the list.
  - Until you choose, your CSV stays as the list.
  - If the list came from an earlier Teacher PDF (not a CSV you uploaded), it is replaced automatically after the check.
  - Uploading another CSV while the question is open withdraws the question.
- **Buttons.** With no data the button is **Download sample CSV** (headers only). With data it is **Download teacher list (CSV)**.
- **File names.** `2627_teacher_names.csv` and `2627_teacher_names_sample.csv`. The `2627` comes from the PDF name when it starts with four digits (for example `2728_…`). Otherwise `2627` is used.

**If very few teachers match:** the check line says how many teachers the list has. A low number usually means the CSV is partial, comes from another term, or uses different initials. Check that it has the headers `Initial,Name,Department` and is saved as comma-separated text, or answer **Yes** to replace it with the list from the PDF.

---

## Checks shown on the result page

| Tab | Check |
|---|---|
| Class | Every class-day adds up to 7 periods, with no overlapping lessons |
| Teacher | Every lesson was placed and every teacher has a code |
| Teacher | The teacher list check (see above) |
| Teacher | Pages with no lessons (shown in red), and teachers with two lessons in the same period |
| Both | How many rows have no venue or subject of their own, with a breakdown by subject |

---

## Limitations

- Built and tested for the **HKUGA College 2026–27 Term 1** aSc exports: landscape, a 10-day cycle `A1`–`B5`, 7 periods. The page reads the PDF by text position and font size, so a different print layout may need the layout constants in `index.html` adjusted.
- Free periods have no row.
- Where a teacher has two lessons in the same period, both are listed.
- In grouped lessons (for example EN or MA across several classes) the PDF does not say which teacher takes which class, so each teacher is listed against every class in the group.
- Teacher codes, and the teacher list built from a Teacher PDF, are worked out from the PDF rather than printed in it. Check the `Teacher Lookup` sheet, or verify against your own CSV.
- A teacher page with no lessons gives no code and no rows.
- The page needs an internet connection to load two libraries from cdnjs.

---

## FAQ

**Is my PDF uploaded?**
No. Everything runs in your browser.

**Why does a cell show a venue list, then a line break, then a list of teachers?**
That teacher has no venue (or subject) of their own: the lesson has more teachers than venues (or subjects). See [the matching rules](#how-teachers-venues-and-subjects-are-matched).

**Why are the venues of HD, MS, CASTLE and OPT not matched to teachers?**
They are whole-level group sessions, so every teacher row shows the full venue list. You can change which subjects are treated this way (see [Customising](#customising)).

**Why does a teacher appear against every class in a group lesson?**
The PDF lists all the teachers of a lesson on every class page and does not say who takes which class.

**A teacher has no code. Why?**
Their page has no lessons, so there is nothing to work the code out from. Load a teacher CSV that includes them, and the page takes the code from it when the name and department match.

**What do E1, E2 and E3 mean in the Free lesson attendance tab?**
They are the three elective groups of S4–S6. A block lesson such as `S5E1` is E1. A class that has two blocks gets the missing number for its other elective. Classes with no blocks (S5C and S6A in this timetable) use the mapping table.

**Why do I have to map the electives of S5C and S6A?**
Their timetables show the subjects (for example EC/PHY, BIO, CHEM) and no E1/E2/E3 block lessons, so the group of each subject is not in the PDF. The defaults are PHY or EC/PHY → E1, CHEM → E2, BIO → E3.

**Can I use it for another school or another layout?**
Only if the PDF has the same print layout. Otherwise the layout constants need adjusting (see below).

---

## Privacy and hosting

- The PDFs stay in your browser.
- **No staff names are stored in `index.html`.** Keep any teacher CSV private. A GitHub Pages site is public, so do not commit a CSV containing staff names to a public repository.
- The repository needs only `index.html`, which is served at `/SchoolTimeTable/`. It uses [pdf.js 3.11.174](https://mozilla.github.io/pdf.js/) and [ExcelJS 4.4.0](https://github.com/exceljs/exceljs), loaded from cdnjs.

## Customising

All settings are at the top of the script in `index.html`:

| Setting | What it controls |
|---|---|
| `GROUP_RE` | The subject codes treated as group sessions (default `HD`, `MS`, `CASTLE`, `OPT…`) |
| `X`, `Y` | Column and row edges of the class timetable grid |
| `TX` | Column edges for teacher pages |
| `DAYS`, `TIMES` | Day codes and period times |
| `FREE_CORE` | Subjects shown as printed in the Free lesson attendance tab (default CHI, EN, MA, CSD, HD, PE, CASTLE, MS, OPT…) |
| `FREE_BLOCK` | Elective block codes, such as S4E2 or S5E1 |
| `freeDefaultE` | The default mapping offered for electives (PHY or EC/PHY → E1, CHEM → E2, BIO → E3) |
