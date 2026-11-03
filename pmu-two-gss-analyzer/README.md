# Two-GSS PMU Analyzer

Browser tool for analysing PMU data from two grid substations (GSS A and GSS B): power-flow direction over time,
angle difference, positive/negative-sequence voltage and unbalance, frequency and ROCOF, oscillations and events.
No backend, no installation: it is one `index.html` file.

## Input CSV (one file per GSS)
Header row + one row per PMU sample. Columns (names are auto-detected, you can remap them in the GUI):

| Time | VA mag | VA angle | VB mag | VB angle | VC mag | VC angle | Frequency |
|---|---|---|---|---|---|---|---|

Time may be seconds, epoch seconds/ms, or a date-time string. Angles in degrees (or radians via the settings).
Both PMUs must be GPS-synchronised so their angles share one reference.

## Run on your computer
Option 1: double-click `index.html` (needs internet once per session for the Chart.js library).
Option 2 (recommended, matches "localhost"): in this folder run `python -m http.server 8000`, open http://localhost:8000
Offline use: `mkdir vendor`, download https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js into `vendor/`,
and change the `<script src=...>` line in `index.html` to `vendor/chart.umd.min.js`.

## Method (short)
Positive-sequence phasor V1 = (Va + a·Vb + a²·Vc)/3, unbalance = |V2|/|V1|. delta = angle(V1_A) - angle(V1_B);
positive delta means A leads, so power flows A to B. With line reactance X: P = VA·VB·sin(delta)/X,
Q(A to B) = (VA² - VA·VB·cos(delta))/X. This is a lossless single-line estimate, not a metered value: validate against SCADA.
