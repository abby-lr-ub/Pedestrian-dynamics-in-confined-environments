# Pedestrian dynamics in confined environments

GPS trajectories and analysis code from a small citizen-science mobility
experiment in **Zona Universitària (Barcelona)**, carried out for the *Social
Systems* course of the Master's in Physics of Complex Systems and Biophysics
(University of Barcelona).

Five groups of students recorded their walks with a mobile tracking app while
exploring a campus district for about an hour. This repository accompanies the
short *Nature Physics*–style Comment **"Pedestrian dynamics in confined
environments"** (`Comment.pdf`), which shows that spatial confinement
suppresses the heavy-tailed, Lévy-like signature of human mobility while
directional persistence survives.

## Contents

- `Comment_code.ipynb` — full analysis (cleaning, MSD, turning angles, flight-length tail, figures).
- `Comment.pdf` — the written Comment.
- `img/` — figures used in the Comment.
- `Data/rawData/` — the five raw GPS tracks (one CSV per group).
- `Data/processedData/` — outputs written by the notebook: derived tracks (`derivedDF/`) and the series plotted in each figure (`plotData/`).

## Data

### Raw data (`Data/rawData/`)

Five CSV files, one per student group, recorded on 2026-06-01 during ~1 h of
walking around Zona Universitària. Four files share a common four-column schema;
one (`laura_parra_judit_gorgori.csv`) is an unedited GPX export with extra,
mostly empty columns. The loader in the notebook harmonises all of them
(renaming, parsing both time formats, and projecting to local metric `x, y`).

Common schema (4 of the 5 files):

| Column      | Type   | Units            | Description                  |
| ----------- | ------ | ---------------- | ---------------------------- |
| `latitude`  | float  | degrees (WGS84)  | Latitude of the GPS fix      |
| `longitude` | float  | degrees (WGS84)  | Longitude of the GPS fix     |
| `elevation` | float  | m                | Device-reported elevation    |
| `time`      | string | UTC              | Timestamp of the fix         |

Per-file inventory:

| File                             | Group | Points | Schema           | `time` format                       |
| -------------------------------- | ----- | ------ | ---------------- | ----------------------------------- |
| `Adrian_Marc_Maria.csv`          | 1     | 149    | common (4 col)   | `YYYY/MM/DD HH:MM:SS+00`            |
| `laura_parra_judit_gorgori.csv`  | 2     | 286    | GPX export (28 col) | `YYYY/MM/DD HH:MM:SS+00`         |
| `pau-gargallo.csv`               | 3     | 288    | common (4 col)   | `YYYY-MM-DD HH:MM:SS.ffffff+00:00` |
| `polanderruta.csv`               | 4     | 280    | common (4 col)   | `YYYY-MM-DD HH:MM:SS.ffffff+00:00` |
| `zona-universitaria-pqs.csv`     | 5     | 510    | common (4 col)   | `YYYY-MM-DD HH:MM:SS.ffffff+00:00` |

> In the GPX export, longitude and latitude are stored as `X` and `Y` and
> elevation as `ele`; the remaining GPX fields (`magvar`, `geoidheight`, `name`,
> `hdop`, `pdop`, …) are empty and unused.

### Processed data (`Data/processedData/`)

Written by the notebook when `saveData = True`. Not required to reproduce the
figures (the notebook regenerates everything from the raw tracks), but included
for convenience.

```
Data/processedData/
├── derivedDF/                     *_derived.csv   cleaned, projected tracks + per-step metrics
└── plotData/                      series behind each figure
```

**`*_derived.csv`** — one file per group: the cleaned and projected
track together with per-step kinematic metrics.

| Column     | Units            | Description                                          |
| ---------- | ---------------- | ---------------------------------------------------- |
| `lat`      | degrees (WGS84)  | Latitude                                             |
| `lon`      | degrees (WGS84)  | Longitude                                            |
| `ele`      | m                | Elevation                                            |
| `time`     | UTC              | Timestamp of the fix                                 |
| `x`, `y`   | m                | Equirectangular projection about the track's centroid |
| `dt`       | s                | Time gap to the previous fix                         |
| `seg`      | –                | Segment id (increments after gaps > 120 s)           |
| `step`     | m                | Distance to the previous fix                         |
| `speed`    | m/s              | `step` / `dt` (spikes > 8 m/s removed)               |
| `heading`  | rad              | Direction of travel, `atan2(Δy, Δx)`                 |
| `turn`     | rad              | Turning angle between consecutive headings, (−π, π]  |
| `cum_dist` | m                | Cumulative path length                               |
| `elapsed`  | s                | Time since the first fix of the track                |

> Note: these are **processed** tracks, not the neutral signal. `step` and
> `speed` carry `NaN` where they were filtered out (gaps > 120 s, speed spikes
> > 8 m/s), and the first fix of every segment has `NaN` step-based metrics by
> construction. `x, y` are local to each track (origin at its own centroid), so
> they are not directly comparable across groups — the notebook re-projects to a
> common origin (`to_common_xy`) when overlaying tracks.

**`plotData/`** — the exact series plotted in the Comment: `trajectories.csv`
(Fig. 1), `MSD_curves.csv` / `MSD_fit.csv` (Fig. 2a), `vonMises_hist.csv` /
`vonMises_fit.csv` / `vonMises_params.csv` (Fig. 2b), and `LevyFlight_*.csv`
(Fig. 2c).

## What the code does

The notebook runs a single top-to-bottom pipeline in four stages:

1. **Load and format** — reads the five raw GPS tracks, unifies their two
   schemas and timestamp formats, and projects latitude/longitude to a local
   metric frame (`x, y`).
2. **Process** — splits each track into gap-free segments, removes speed
   spikes, and derives per-step metrics (step length, speed, heading, turning
   angle).
3. **Analyse** — computes the three mobility descriptors: the time-lag–binned
   MSD with short- and long-lag slopes; the turning-angle distribution with a
   von Mises fit; and the flight-length tail, fitted to power-law, log-normal
   and exponential laws and compared with a likelihood-ratio test.
4. **Plot** — generates Fig. 1 (trajectories) and Fig. 2 (the three
   descriptors), and optionally writes the derived data to `Data/processedData/`.

### Run it

It is neccessary to have the raw data in `Data/rawData/` for the notebook to run and the `Data` folder in the same folder as the notebook.

In order to run the notebook, you will need to install the following Python packages: `numpy`, `pandas`, `matplotlib`, `scipy`. It can be done in the following way:

```bash
pip install numpy pandas matplotlib scipy
```

Run the cells top to bottom; figures are saved into `img/`, which will be created automatically if it does not exist. 

 

## Cite

> López Rivas, A. (2026). *Pedestrian dynamics in confined environments: GPS
> trajectories and analysis from a citizen-science experiment in Zona
> Universitària (Barcelona)* [Data set and code]. GitHub.
> https://github.com/abby-lr-ub/Pedestrian-dynamics-in-confined-environments

## License

Code: MIT (see `LICENSE`). Data in `Data/`: CC BY 4.0.

## Author

Alba López Rivas — University of Barcelona, 2026.
Data collected by the five student groups of the *Social Systems* course.
