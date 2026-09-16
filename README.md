# Vertical Web Page Project Knowledge Base

## 1. What This Project Is

This directory is a collection of browser-based dashboards and internal portal prototypes for a Drivetrain / Powertrain organization. It is presented as one portal, but technically it is a set of independent, self-contained HTML applications.

The applications cover:

- An organization and command-center portal
- Three team workspaces
- Learning and competency development
- Skill-matrix analytics
- Personal work and learning status
- Powertrain / BLK data intelligence
- A Park-by-Wire knowledge and document library

There is no framework project, package manager, backend service, shared JavaScript module, shared stylesheet, or database server in this directory. Each HTML file contains its own markup, CSS, JavaScript, and data-handling logic.

## 2. How to Run It

### Recommended: local static server

On Windows, open PowerShell and run:

```powershell
cd "c:\Anand\Vertical_Web_Page\Vertical_Web_Page"
py -m http.server 8000
```

If `py` is unavailable, use:

```powershell
python -m http.server 8000
```

Open:

```text
http://localhost:8000/Vertical_Webpage.html
```

`Vertical_Webpage.html` is the newer unified portal shell and is the best starting point. `old_home.html` is an older, overlapping version of the home portal.

Stop the server with `Ctrl+C`.

### Direct file opening

Any HTML file can also be opened by double-clicking it. A local server is more reliable because browser file-origin restrictions can affect local storage, IndexedDB, downloads, and external library loading.

### Runtime requirements

- A modern browser such as Microsoft Edge or Chrome
- Internet access for CDN-hosted libraries and Google Fonts
- Corporate network access for some PBW videos, documents, and intranet links
- A compatible Excel/CSV file when using workbook-driven pages

There is no `npm install`, build command, or application deployment step.

## 3. Workspace Inventory

### HTML applications

| File | Role | Main data behavior |
|---|---|---|
| [Vertical_Webpage.html](Vertical_Webpage.html) | Newer unified Drivetrain portal | Embedded defaults; optional client-side Excel upload; theme in `localStorage` |
| [old_home.html](old_home.html) | Older unified command center | Mostly hard-coded data; simulated sync; embedded BLK preview |
| [Team_Raaj_Workspace.html](Team_Raaj_Workspace.html) | Raaj team workspace | Reads `Team_Raaj` from an uploaded workbook; saves latest file in IndexedDB |
| [Team_Vinay_Workspace.html](Team_Vinay_Workspace.html) | Vinay team workspace | Reads `Team_Vinay` from an uploaded workbook; saves latest file in IndexedDB |
| [Team_Aditya_Workspace.html](Team_Aditya_Workspace.html) | Aditya team workspace | Reads `Team_Aditya` from an uploaded workbook; saves latest file in IndexedDB |
| [LCD_Dashboard.html](LCD_Dashboard.html) | Learning and competency dashboard | Static, hard-coded learning content; no workbook loading |
| [Skill_Matrix_Dashboard.html](Skill_Matrix_Dashboard.html) | Skill-matrix and mini-BI dashboard | User-selected XLSX/XLS/CSV parsed in the browser |
| [My_Work_Status.html](My_Work_Status.html) | Personal work and learning dashboard | User-selected workbook/CSV parsed in the browser |
| [New_Data_Dashboard_Final.html](New_Data_Dashboard_Final.html) | Powertrain / BLK data intelligence dashboard | Embedded records plus optional workbook/CSV import |
| [pbw.html](pbw.html) | Park-by-Wire digital library | Embedded library plus local document indexing and optional AI configuration |

### Workbooks

- `Vertical Page Database.xlsx`: intended source for portal-style data, but it is not automatically loaded by the portal. The user must select a workbook where the upload workflow exists.
- `Skillmatrix.xlsx`: skill-matrix workbook with sheets including `Database`, `Dashboard`, `Shift_by_wire`, `OEMLib Software Development and`, `OBD Cluster`, `Interface`, `Gen4 Inverter`, `PU_SF_ECSS`, `Module_Testing`, `EEPROM and BLK`, `Data Update status`, and `Yearly Plan`.
- `My_Work_Status.xlsx`: work-status workbook with sheets including `Database_Backend`, `Work_Dashboard`, `Retrospection_Dashboard`, `June_Retrospection`, `Database`, `Sheet1`, `june`, `july`, `may`, `Jira (7)`, and `Impact_Dashboard`.

The presence of a workbook beside an HTML file does not mean that the page reads it automatically. Most pages use a file picker and parse only the workbook selected by the user.

### Image assets

The directory includes member images, `slide1.jpg` through `slide5.jpg`, `slide_0.jpg`, and `IMG_1195.HEIC`. Several HTML files reference additional images that are not present, including `sunita.jpg`, `vinay.jpg`, `raajkumar.jpg`, `aditya.jpg`, `slide6.jpg`, `slide7.jpg`, `slide8.jpg`, and `SBW.jpg`.

## 4. Main User Workflow

```text
Open Vertical_Webpage.html
        |
        +--> Browse organization, projects, delivery, risks, skills, learning and activity sections
        |
        +--> Open a team workspace
        |       |
        |       +--> Select an Excel workbook
        |       +--> Parse the team worksheet in the browser
        |       +--> View cards, tables, charts and searches
        |       +--> Restore the last workbook from IndexedDB on later visits
        |
        +--> Open Skill Matrix / My Work Status / BLK Data / PBW
                |
                +--> Use embedded sample data or select a local workbook/document
                +--> Filter, search, chart, edit, index, export or print locally
```

The portal pages use normal relative links between HTML files. Internal sections are switched by JavaScript functions such as `switchPage()`. Search boxes generally route either to another HTML page or to a section within the current page.

## 5. Page-by-Page Behavior

### `Vertical_Webpage.html`

This is the newer intended homepage. It provides a Drivetrain command center with organization information, team links, project and delivery views, risks, milestones, gallery content, skills, innovation, activity, learning, research, and personal-work sections.

It starts with `DEFAULT_DATA`, then can parse a selected workbook in the browser using SheetJS. Its workbook parser looks for tables such as `Home_Database`, `Consolidated`, `Team_Raaj`, `Team_Vinay`, and `Team_Aditya`. The selected data stays in memory for the current session; it is not persisted. The theme is persisted under `portalTheme` in `localStorage`.

The visible “sync” behavior is not a server synchronization process. Without a selected workbook it is only a status/button simulation.

### `old_home.html`

This is an older, larger command-center implementation with dashboard KPIs, an organization chart, team workspaces, projects, deliveries, learning, research, innovation, quick links, a canned chatbot, and an embedded BLK dashboard preview.

Its `syncWorkbook()` function does not read a workbook or contact a backend. It changes UI status and waits with a timer. Most content is hard-coded in the HTML. Several MB Inside profile destinations are placeholders.

### Team workspaces

The three team pages follow the same architecture but use different worksheet names and domain fields:

- **Raaj:** projects, work packages, deliveries, team members, skills, risks, innovations, and activities.
- **Vinay:** projects, work packages, BTV streams, deliveries, members, skills, defects, risks, and activities.
- **Aditya:** projects, work packages, analytics streams, deliveries, members, skills, KPIs, dependencies, risks, and activities.

Each page lets the user select an Excel file, parses the required team sheet and expected headers, renders dashboards/charts/search results, and stores the latest uploaded workbook in browser IndexedDB. The pages use databases named `TeamRaajMulticolorDB`, `TeamVinayMulticolorDB`, and `TeamAdityaMulticolorDB`; each uses a `files` object store and the key `latest`. These are browser-local stores, not shared databases.

### `LCD_Dashboard.html`

This is a static learning and competency-development experience. It contains a team roster, quarterly planner, Conscious Competence learning model, digital-library mock-up, objective modules, clock, technology feed, modal dialogs, tabs, and canned chatbot responses.

It does not load `Skillmatrix.xlsx`, does not upload files, and does not have a backend or meaningful persistence.

### `Skill_Matrix_Dashboard.html`

This page parses a user-selected `.xlsx`, `.xls`, or `.csv` file with SheetJS. It is designed around a `Database` worksheet and structures such as `tblSkillMatrix`, `tblTopicActions`, and `tblClusters`.

It provides cluster and individual analytics, topic gaps, a pivot explorer, workbook explorer, data dictionary, saved views, bookmarks, row details, CSV/PNG/JSON exports, and an offline chatbot. If the user enters an API base URL, key, and model, the optional live chatbot calls an OpenAI-compatible `/chat/completions` endpoint. The API configuration is stored in `localStorage`, including the key.

### `My_Work_Status.html`

This page is a personal work and learning dashboard. It reads the first selected worksheet containing `Task ID` and `Section`, uses task columns A:J and learning columns K:O, and renders status charts, category/completion trends, learning status, review/forecast views, readiness, workbook exploration, settings, command-palette actions, focus/presentation modes, chatbot answers, exports, and printing.

The page starts empty until a workbook is selected. Themes, views, bookmarks, and hint state are stored in `localStorage`; the workbook itself is not automatically persisted.

The UI title refers to “March Review & April Forecast”, although the bundled workbook also contains June and July sheets. Only the recognized task/learning table is parsed rather than every sheet.

### `New_Data_Dashboard_Final.html`

This is a Powertrain / BLK data catalog and intelligence dashboard. It begins with embedded `rawWorkshopData`, normalizes records into fields such as group, type, classification, source, platform, value stream, application, decision, owner, user domain, and status, and renders cards, filters, charts, data flow, access matrix, and detail drawers.

It can import a workbook or CSV through a flexible column-alias mapper. It can also add, edit, and delete local records, export JSON/CSV/HTML/XLSX, open a configured Microsoft Forms URL, and record feedback locally. “Request Access” is currently a placeholder alert. No feedback is submitted programmatically to Microsoft Forms.

Its local state is stored under keys such as `ptih_merged_data`, `ptih_merged_filters`, `ptih_dashboard_meta`, `ptih_feedback_log`, `ptih_msforms_url`, and `ptih_theme`.

### `pbw.html`

This is a Park-by-Wire corporate digital library. It provides video/session records, PDF/deck/document/tool catalog search and filters, pinned/recent items, a video overlay, PDF viewing with PDF.js, DOCX extraction with Mammoth, XLSX/CSV extraction with SheetJS, a local RAG-lite index, offline metadata-based chatbot behavior, optional cloud AI settings, an admin editor, library import/export, and a self-download function.

The default library is embedded in `LIB_DEFAULT`. Users can select local documents to build an in-browser index. Library content, pins, recent items, theme, admin state, AI settings, and the RAG index use `localStorage` keys beginning with `PBW_` or `CDL_`.

The default media paths include corporate network locations and placeholders, so they are environment-dependent. The displayed `pbw-admin` passphrase is client-side UI protection, not real authentication.

## 6. Data Flow

### Static and embedded data

Many pages render immediately from JavaScript objects embedded in the HTML. This is demo or seed data, not a live organizational data feed.

### Workbook-driven flow

```text
User selects .xlsx/.xls/.xlsm/.csv
        -> SheetJS reads the file in the browser
        -> Page finds a named sheet, headers, or aliases
        -> Rows are normalized into page-specific objects
        -> Cards, tables, charts, filters and search are rebuilt
        -> Optional local save or browser download/export
```

Each page has its own parser and schema assumptions. There is no central schema validator or shared data model, so a workbook that works for one page may not work for another.

### Export flow

Exports use browser APIs and client-side libraries to create downloads such as CSV, JSON, XLSX, PNG, updated HTML, or printed pages. These exports do not write to a server.

## 7. Is There a Database?

### Short answer

There is no conventional application database and no backend database server.

### What is used instead

1. **In-memory JavaScript state:** default records and the currently parsed workbook live in the page while it is open.
2. **`localStorage`:** themes, filters, saved views, bookmarks, locally edited data, feedback logs, chatbot settings, and some library indexes are saved per browser origin.
3. **IndexedDB:** the three team workspaces save the latest uploaded workbook binary in browser-local databases.
4. **User-selected files:** Excel, CSV, PDF, DOCX, and other files are read locally by the browser.
5. **Optional external API:** Skill Matrix can call an OpenAI-compatible chat endpoint only when configured by the user.

Browser storage is tied to the browser/profile and origin. It is not automatically shared with other users, machines, browsers, or pages served from a different origin. Clearing site data or changing the origin can remove or hide saved state.

## 8. Backend, APIs, Authentication and Network Use

- No server-side code, SQL, API routes, REST service, authentication service, or traditional form submission exists in the directory.
- Most third-party libraries load from public CDNs, including Tailwind, Bootstrap, Chart.js, Font Awesome, SheetJS, PDF.js, Mammoth, Anime.js, html2canvas, and Google Fonts.
- The Skill Matrix live chatbot is the clearest implemented `fetch` call. Its API key is stored in browser `localStorage`, which is unsuitable for secrets on shared or untrusted machines.
- PBW URLs may point to corporate network files or intranet resources. Its optional AI settings are also client-side.
- The PBW Microsoft Bot Framework WebChat CDN is present, but it is not the main visible workflow.
- The PBW Home button points to `home.html`, which is not present in this directory.

## 9. Navigation Map

Both portal shells link to the three team workspaces and the standalone dashboards:

```text
Vertical_Webpage.html / old_home.html
  |-- Team_Raaj_Workspace.html
  |-- Team_Vinay_Workspace.html
  |-- Team_Aditya_Workspace.html
  |-- LCD_Dashboard.html
  |-- Skill_Matrix_Dashboard.html
  |-- My_Work_Status.html
  |-- New_Data_Dashboard_Final.html
  `-- pbw.html
```

`old_home.html` and `Vertical_Webpage.html` are competing versions of the same portal concept. They are not two routes backed by one application state, and changes to one do not update the other.

## 10. Known Issues and Risks

- **Two homepages:** choose one canonical portal entry point; the newer candidate is `Vertical_Webpage.html`.
- **Missing assets:** profile photos, `slide6.jpg` to `slide8.jpg`, and PBW's `SBW.jpg` are referenced but absent. Some pages use fallback avatars or fallback gallery behavior.
- **Broken PBW home link:** `home.html` does not exist.
- **Simulated sync:** the old portal's sync action is visual only; the new portal's sync is meaningful only after a workbook is selected.
- **Optional workbook loading:** nearby `.xlsx` files are not automatically consumed by the pages.
- **Duplicate data:** similar team, project, milestone, and risk data is embedded in multiple files and workbooks, so there is no enforced single source of truth.
- **CDN dependency:** blocked internet access can remove styling, charts, file parsing, PDF viewing, fonts, or other functionality.
- **Client-side secrets:** API keys and admin configuration are stored in browser storage and are not protected.
- **Corporate URLs:** PBW content and intranet/profile links may work only on the corporate network.
- **Browser compatibility:** features such as IndexedDB binary storage, `structuredClone`, `color-mix`, `<dialog>`, and `showOpenFilePicker` require a modern browser and may be affected by security policy.

## 11. Recommended Operating Model

For a reliable demo:

1. Run the local static server described above.
2. Start at `Vertical_Webpage.html`.
3. Use the embedded content first to verify the portal.
4. Select the matching workbook explicitly when testing a workbook-driven dashboard.
5. Use a corporate network connection only when testing PBW or intranet-linked content.
6. Treat browser storage as disposable local test state, not shared production data.

For a production system, the project would need one canonical portal, shared CSS/JavaScript, a defined data schema, a real backend/database, authentication and authorization, secure server-side API proxying, managed file storage, and bundled or approved dependencies instead of relying on public CDNs.

## 12. Bottom Line

This project is currently a polished set of static, client-side internal dashboard experiences. It is useful for demonstrations, local exploration, workbook visualization, and browser-local editing. It is not currently a connected enterprise application: there is no live database, no central synchronization, no shared multi-user state, and no server enforcing security or data integrity.
