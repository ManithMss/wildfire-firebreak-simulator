# Data

## spetses_fire_season_wind_2015_2024.csv

Real weather data used in notebook Section 6.5 (real wind data).

- **What:** daily maximum wind speed at 10 m (km/h) and dominant wind direction at 10 m (degrees, the direction the wind comes from) for Spetses Island, Greece (latitude 37.26, longitude 23.15), for every day from 1 June to 30 September in each year from 2015 to 2024 (1,220 rows, no missing values).
- **Source:** ERA5 reanalysis (H. Hersbach et al., "The ERA5 global reanalysis," *Q. J. R. Meteorol. Soc.*, 2020, doi: 10.1002/qj.3803), downloaded through the Open-Meteo historical weather API (https://open-meteo.com/, doi: 10.5281/zenodo.7970649) with the request `archive-api.open-meteo.com/v1/archive?latitude=37.26&longitude=23.15&start_date=YYYY-06-01&end_date=YYYY-09-30&daily=wind_speed_10m_max,wind_direction_10m_dominant&timezone=Europe/Athens&wind_speed_unit=kmh`, one request per year.
- **Licence:** Open-Meteo data is published under CC BY 4.0. Weather data by Open-Meteo.com, based on ERA5 (Copernicus Climate Change Service).
- **Processing:** none in the file itself; the notebook converts direction and speed into the model's wind direction and strength (see notebook Section 6.5 and THEORY.md).

## Model parameters

All other model parameters (base ignition probability, ember-spotting probability, firebreak width, grid size, and the wind settings in the main experiments) are synthetic and assumed, chosen to produce a realistic range of containment outcomes rather than taken from a measured source. These assumptions are documented in [THEORY.md](../THEORY.md).
