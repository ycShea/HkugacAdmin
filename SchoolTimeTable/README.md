# Timetable PDF → Excel

A single-file, browser-based tool that converts **aSc Timetables** PDF exports (class or teacher timetables) into database-style Excel files. Everything runs locally in your browser; no PDF is uploaded anywhere.

## Quick start

1. Open `index.html` in a modern browser (Chrome, Edge, Firefox, Safari). No build step or server is needed.
2. Pick a tab: **Class Timetable PDF → Excel** or **Teacher Timetable PDF → Excel**.
3. Drop in the PDF (or click to choose it).
4. Review the stats, warnings and 15-row preview, then download the Excel file(s).

> An internet connection is needed the first time the page loads, because [pdf.js](https://mozilla.github.io/pdf.js/) 3.11.174 and [ExcelJS](https://github.com/exceljs/exceljs) 4.4.0 are loaded from cdnjs.

## Class timetable tab

**Input:** the aSc class-timetable PDF (landscape letter, one page per class, 10 day columns A1–B5 × 7 period rows), e.g. `2627_T1_Class_v2.pdf`.

**Optional teacher names:** the PDF only contains teacher codes (e.g. `KH`, `CSC`). To fill the *Teacher Name* column, upload a CSV with the headers `Initial,Name,Department`. Use **Download sample CSV** to get an empty template.

**Outputs**

| Button | File | Contents |
| --- | --- | --- |
| Download database Excel | database workbook | Sheets: `Timetable`, `Class Lessons`, `Teacher Lookup`, `Notes` |
| Download class timetables | class timetable workbook | One grid sheet per class (periods down, days across) |

`Timetable` columns: Teacher, Day, Period, Class, Venue, Co-teaching (Y or N), Subject, Time, Teachers sharing this venue, Pairing, Teacher Name.

## Teacher timetable tab

**Input:** the aSc teacher-timetable PDF (one page per teacher), e.g. `2627_T1_Teacher_v2.pdf`. Teacher names are read from the PDF, so no CSV is needed.

**Outputs**

| Button | Contents |
| --- | --- |
| Download database Excel | Teacher, Day, Period, Class, Venue, Co-teaching, Lesson/Meeting/Support, Subject, Pairing |
| Download teacher timetables | One timetable sheet per teacher |

## How lessons are interpreted

- **Pairing by position.** If a cell lists several teachers, venues or subjects, they are matched in order. For example, `AF / HC / SYC` with venues `601,602,603` gives AF–601, HC–602 and SYC–603. The same applies to subjects such as `CH/BIO`.
- **One value applies to all.** A single subject or single venue applies to every teacher in the cell.
- **Co-teaching = Y** when more than one teacher shares the same venue in the same slot.
- **Unpaired cells.** If the teacher and venue counts differ and there is more than one venue, the full venue list is shown and the row is flagged in the **Pairing** column.
- **Double periods.** Merged cells (detected from cell layout) are expanded to one row per period.
- **Identical rows across classes** (e.g. S2H / S2K / S2U sharing a lesson) are combined into one row.
- **Checks.** The tool warns about overlapping lessons and class-days that don't add up to 7 periods; teacher PDFs also report clash periods.

## Notes and limitations

- The class parser is tuned to the fixed page layout of the aSc export (column and row positions are hard-coded for the 2026-27 Term 1 PDFs). A differently laid out PDF may need the `X`/`Y` constants adjusted.
- The core logic is exposed as `window.TT` (also usable under Node via `globalThis`), and the page is scriptable through `window.__tt`.
- The `#teacher` URL hash opens the teacher tab directly.

## Tech

Plain HTML/CSS/JavaScript in one file, with light/dark theme support. Libraries: pdf.js (PDF text extraction) and ExcelJS (workbook generation).
