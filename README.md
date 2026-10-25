# A one HTML encapsuled script to visualize an ADIF file

**index.html** is a self-contained web dashboard for the PLUSSCOUTS PA3EFR/J amateur-radio/scouting activity. It presents a QSO logbook, station and contact statistics, an interactive map, and a tool that generates a downloadable “Results Poster” image from the logged contacts.

# What it does
The page is a single HTML application: it loads its interface and data in the browser, without requiring a separate backend or installation. Its main purpose is to visualize and summarize radio contacts (“QSOs”) made during the scouting/radio event.

# The interface includes:

- A QSO Log table with contact details such as callsign, date, band, mode, country, DXCC entity, grid locator, coordinates, and distance.

- Search and Band/Mode filters, so the log can be narrowed without hiding the filter controls.

- Statistics for total QSOs, countries/DXCC entities, active days, modes, bands, and the longest contact.

- A map view showing contact locations relative to the home station.

- A Results Poster generator that turns the loaded QSO data into a social-media-style PNG.

# Results Poster
The poster generator is exposed through the browser function window.openResultsPoster(Q), where Q is the array of loaded QSO records. It opens a modal dialog titled Results Poster.

- Poster controls
Control	Function
Choose image	Select a group photo or logo (image/*) to place at the top of the poster. The image is scaled to fit and is never cropped. 
Generate poster	Becomes available after selecting an image; builds the poster from the current QSO data. 
Use club logo instead	Uses the globally defined PA3EFR_LOGO image, if available. 
×	Closes the poster dialog. 
Download PNG (2160×3840)	Saves the finished poster as a PNG file. 
Poster contents
The generated image is a vertical 1080×1920 design, exported at 2160×3840 pixels. It contains:

- The selected photo or club logo.

- The station callsign, defaulting to PA3EFR/J when no station callsign is present in the data.

- The slogan “SCOUTS CONNECT WORLDWIDE AND BEYOND.”

- Statistic cards for total QSOs, unique DXCC countries, number of active days, and longest QSO.

- A world map with curved connection lines from the home grid locator to contacted stations; marker size reflects the number of QSOs per country.

- Bar charts for QSOs per mode and QSOs per band.

- A Top 10 countries list with national flags.

- A footer with the callsign, “SCOUTING · RADIO · FRIENDSHIP,” and “Echt wel gaaf !”.

# Data and calculations
The script calculates its summary directly from the QSO records. It counts unique DXCC entities, determines the first and last contact dates, counts modes and bands, identifies the contact with the greatest distance, and ranks countries by QSO count.

It also performs these checks:

- Contacts without a valid positive DXCC value are excluded from the country count and listed in the report as “No DXCC.”

- Contacts without usable latitude/longitude, or with an invalid AA00 grid locator, are excluded from the map and reported as “Not on map.”

- If more than ten countries tie for tenth place, the extra tied countries are reported as “Tie, not shown in top 10.”

- If a country flag cannot be found, the country is reported under “No flag found.”

- The home position is derived from the first valid myGrid Maidenhead locator in the QSO data. If none is available, it falls back to approximately 5.3°E, 52.1°N, in the Netherlands.

- Map and logbook fixes
A separate enhancement script improves the original page after it loads:

- It adds a “Logboek verbergen / Logboek tonen” button to the QSO Log section. This hides or shows only the log table and pagination, while keeping the search and Band/Mode filters visible.

- It sets the Leaflet map container to a height of 560 pixels and full width, then triggers a browser resize event so Leaflet recalculates its tile layout. This compensates for the original bundle including Leaflet JavaScript but omitting its stylesheet.

- It uses a MutationObserver and delayed initialization checks, so the fixes are reapplied when the React-rendered interface changes.

# Internet requirements
The page needs an internet connection for several external resources:

- D3.js and TopoJSON Client from jsDelivr, used to draw the poster’s world map.

- The world-atlas land geometry file, also loaded from jsDelivr.

- Country flag SVGs from FlagCDN.

- Google Fonts for the Poppins typeface.

- Map tiles from OpenStreetMap, OpenTopoMap, and ArcGIS, depending on the map layer in use.

If these resources are blocked, the main dashboard may still work, but the poster map, flags, fonts, and interactive map tiles can fail or render incomplete. The poster explicitly reports that its map requires internet access for D3/world-atlas.

# How to use it
Open **index.html** in a modern browser.

Wait for the dashboard and QSO data to load.

Use the search box and Band/Mode filters to inspect contacts.

Use **Logboek verbergen** if you want to focus on the statistics and map without scrolling through the full log.

To create a shareable summary, run openResultsPoster(QSO_DATA) in the browser console, or use the page control that calls it.

Choose a photo, select Generate poster, then download the resulting PNG.

Technical notes
The application is built around a React bundle, with Leaflet used for the interactive map.

The custom additions are contained in two inline scripts: one for the logbook toggle/map layout fix and one identified as results-poster.

No local browser storage keys are used by the custom poster/logbook code.

The poster output filename is based on the station callsign, for example PA3EFR-J_results-poster.png.

