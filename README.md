# Spotify Insights

Find your most-played artists and tracks, and see when you listen to music, using a Spotify listening-history export. Built by [VijaySreekar](https://github.com/VijaySreekar) and [GanapathiThota](https://github.com/GanapathiThota).

[Run locally](#run-locally) · [Sample data](sample_data/README.md)

![Spotify Insights showing analysis of fictional listening history](docs/images/spotify-analysis.png)

*Screenshot using the included fictional listening history.*

## Try it

Run the app locally using the instructions below, then select **Explore a fictional demo dataset** in the sidebar. This loads 714 fictional plays without requiring an upload. Turn the option off to analyze your own JSON or CSV file.

The [hosted app](https://appspotifyinsights-zdr4w9e8yfszel8mxq275p.streamlit.app/) needs redeploying: it still showed an old dependency error when checked on 6 September 2026. Use the local setup for now.

## What you can explore

- **Artist analysis:** unique artists, play counts, listening time, and word clouds.
- **Track analysis:** frequently played tracks and unique-track counts.
- **Day-wise usage:** listening patterns across days of the week.
- **Hourly usage:** activity across hours of the day.
- **Listening time:** totals and daily patterns derived from play durations.

The sidebar switches between analyses. The page also displays the cleaned data and a statistical overview so the input behind the charts can be inspected.

## How it works

`app.py` contains the Streamlit interface and analysis pipeline:

1. Read a JSON or CSV export using pandas. CSV uploads support comma, tab, semicolon, or a custom delimiter.
2. Normalize a supported timestamp column and derive year, month, day, weekday, and hour.
3. Convert play duration from milliseconds into listening-time fields.
4. Group the data by artist, track, day, or hour and render tables and charts with Matplotlib, Seaborn, and WordCloud.

Data loading uses Streamlit's cache. This application analyzes uploaded exports; it does not connect to a Spotify account or request Spotify API credentials.

## Run locally

Tested with Python 3.12.

```bash
git clone https://github.com/VijaySreekar/StreamLitSpotifyInsights.git
cd StreamLitSpotifyInsights
python3 -m venv .venv
```

Activate the environment:

```bash
# macOS / Linux
source .venv/bin/activate

# Windows PowerShell
.venv\Scripts\Activate.ps1
```

Install dependencies and start the app:

```bash
python -m pip install -r requirements.txt
python -m streamlit run app.py
```

Open the local URL printed in the terminal, then select the fictional demo dataset.

## Input format

For all analyses, include an artist, track, timestamp, and duration. This example is fictional:

```json
[
  {
    "endTime": "2026-01-01 18:30",
    "artistName": "Harbour Lights",
    "trackName": "Blue Hour",
    "msPlayed": 210000
  }
]
```

| Field | Accepted names |
| --- | --- |
| Timestamp | `endTime`, `Play Time`, `timestamp`, or `dateTime` |
| Duration in milliseconds | `msPlayed`, `Duration_ms`, or `duration_ms` |
| Artist | `artistName` |
| Track | `trackName` |

A CSV file uses these names as its header row. Exports using other schemas need conversion first.

## Current limitations

The app expects valid timestamps and the fields listed above. Missing artist, track, or duration fields can break individual views. Duration calculations were built for song plays and need checking before analysing unusually long recordings.

For personal listening history, run locally if you do not want to upload it to the hosted application.

## Validation

Checked with Streamlit's app testing interface: empty state, demo selection, all five analysis views, and switching back to the empty state. These checks use the bundled sample data.

## Contributing

For a bug report, include the failing analysis and a small synthetic example that reproduces it. Avoid attaching personal listening history. For code changes, include a short explanation and how you tested them.
