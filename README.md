# cryo-energy-lab
Interactive calculator for comparing room-temperature and cryogenic AI data-center energy use, with editable chip-power assumptions, cooling models, and power-breakdown charts.
Purpose: compare RT and cryogenic data-center electricity consumption.
Usage: download index.html and open it in a browser; no installation required.
Calculation flow: RT PUE → IT allocation → redesigned chip power → refrigeration → net savings.
Cooling models: Anhui/Chuzhou, closed-loop LN₂, 50% Carnot, 100% Carnot, and the hypothetical TSMC scenario.
Assumptions and sources: distinguish literature results, our measurements, and transferred GPU/HBM assumptions.
Limitations: equal-throughput comparison; junction temperatures don’t automatically change chip-power ratios; nitrogen properties are manual inputs.
