CNC 2026 Location Lookup
An interactive tool that lets anyone check whether their location falls within a registered City Nature Challenge 2026 project boundary.
Live tool: https://alisonkestrel22.github.io/cnc2026-lookup/

What it does
A user types any location — a city, neighborhood, address, or landmark — and the tool:

Geocodes it using OpenStreetMap Nominatim (free, no API key required)
Checks the coordinates against the real iNaturalist place boundaries for all 773 registered CNC 2026 projects
Returns a yes/no result with a direct link to the matching iNaturalist project(s)

If a location isn't in any participating project area, it also links to the CNC 2026 Global Project.

How it was built
The tool is a single self-contained HTML file (index.html) with all project boundary data embedded directly — no server, no database, no API key needed. It works entirely in the browser.
Data sources:

Project list: CNC 2026 City Registration Form (Google Sheets export), filtered to projects with "Project Added to Umbrella Project" checked
Place boundaries: fetched from the iNaturalist .json endpoint for each project (using project_observation_rule_terms and rule_place fields), then matched against the iNaturalist Places CSV export to get bounding box coordinates
Geocoding: Nominatim/OpenStreetMap at query time

Built with: Claude (Anthropic) — the entire tool including data processing, boundary matching logic, and UI was built through a Claude conversation.

How to update for CNC 2027 (or mid-season changes)
When new projects are added to the umbrella or the project list changes, here's what to do:
1. Export the updated project spreadsheet
Export the CNC City Registration Form responses as a CSV, filtered to only rows where column K ("Project Added to Umbrella Project") is checked and column H ("Project Link") has a value.
2. Fetch place data for any new projects
Use the fetch_project_places.py script (in this repo) to fetch boundary data from iNaturalist for each project link. Run it from your terminal:
python3 fetch_project_places.py
This produces a project_places.json file.
3. Upload both files to Claude
Upload the CSV and project_places.json to a new Claude conversation and ask it to rebuild the lookup tool. The prompt can be as simple as:

"Here are the updated CNC project spreadsheet and project_places.json. Please rebuild the cnc2026_lookup tool with this new data."

4. Upload the new index.html to GitHub

Go to this repository on GitHub
Click Add file → Upload files
Upload the new index.html (rename from cnc2026_lookup.html if needed)
GitHub Pages will update automatically within a minute or two


Files in this repository
FilePurposeindex.htmlThe lookup tool itself — the only file needed to run the toolfetch_project_places.pyScript to fetch iNaturalist place data for a list of projectscnc_project_list.jsonInput file for fetch_project_places.py — list of project slugsREADME.mdThis file

Notes

The tool requires an internet connection to geocode user input (Nominatim call), but all 773 project boundaries are embedded in the HTML and work offline
773 of 773 projects use precise iNaturalist place boundaries; results may not always be 100% accurate so users are advised to verify by clicking the project link
Built April 2026 for the California Academy of Sciences / Natural History Museum of Los Angeles County CNC Global Organizing Team


Questions or issues?
Open an Issue on this repository.
