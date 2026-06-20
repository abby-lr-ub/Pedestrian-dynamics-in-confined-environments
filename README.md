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
- `Data/processedData/` — outputs written by the notebook: cleaned/derived tracks (`DerivedDF/`) and the series plotted in each figure (`plotData/`).

## Run it

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
