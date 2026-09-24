# Cryo Energy Lab

An interactive calculator for comparing room-temperature (RT) and cryogenic AI data-center electricity consumption.

Explore how chip redesign, IT power allocation, facility overhead, and refrigeration efficiency affect net energy savings.

## Quick Start

1. Download `cryo-energy-lab-ver0.1.html`.
2. Open the downloaded file in a modern web browser.
3. Adjust the assumptions to update the power breakdowns and savings.

No installation, build process, or backend server is required.

## Features

- Editable RT baseline, including PUE and IT power allocation.
- Separate power assumptions for GPU logic, HBM, CPU logic, and DDR.
- Five cooling scenarios evaluated using the same electronics power.
- Interactive power-breakdown charts normalized to RT facility electricity.
- Component-level power review and calculation-flow diagram.
- Explanatory popovers and section-level default resets.
- SVG chart export and downloadable HTML snapshots.

## How the Model Works

The controls follow five steps:

1. **PUE and facility overhead:** establish RT facility electricity and separate cooling from non-cooling overhead.
2. **IT power allocation:** divide IT power among accelerators, hosts, network/storage, and other server loads.
3. **Temperature and heat:** specify the shared bath, heat sink, component junction temperatures, and parasitic heat.
4. **Power after cryogenic redesign:** apply remaining-power ratios to each component and configure other loads.
5. **Cooling models:** calculate replacement refrigeration power and compare total electricity with RT operation.

The main relationships are:

- RT facility power = RT IT power × PUE
- RT cooling power = RT facility power − RT IT power − non-cooling facility power
- Redesigned component power = RT component power × remaining-power fraction
- Bath heat = redesigned cold-electronics power + parasitic heat
- Net saving = 1 − cryogenic facility power / RT facility power

All input power budgets use common units. They do not need to sum to 100; the charts normalize results to the RT facility total.

## Cooling Scenarios

| Scenario | Model boundary |
| --- | --- |
| Anhui / Chuzhou | Open-loop nitrogen production, including air separation and liquefaction, with optional exhaust-gas cooling of warm loads. |
| Closed-loop LN₂ | Refrigeration with cold return or internal recuperation, using an editable net Carnot efficiency. |
| 50% Carnot | Refrigeration requiring twice the ideal Carnot work. |
| 100% Carnot | Reversible refrigeration: the ideal minimum work between the bath and heat sink. |
| TSMC “magic” | A previously fitted temperature-dependent scenario curve; treated as hypothetical where it falls below Carnot work. |

The RT cooling allowance is replaced by the selected cooling calculation, rather than added again.

Open-loop exhaust recovery reduces the cooling demand of warm loads. The closed-loop model receives no additional external exhaust-cooling credit.

## Default Chip-Redesign Assumptions

Remaining power is expressed relative to each component’s own RT power, before refrigeration.

| Component | Junction temperature | Power remaining | Basis |
| --- | ---: | ---: | --- |
| GPU logic | 150 K | 24.9% | Transferred approximately 150 K redesigned-logic literature point. |
| HBM | 100 K | 6.2% | Transferred approximately 100 K redesigned-logic assumption. |
| CPU logic | 100 K | 6.2% | Approximately 100 K redesigned-logic literature point. |
| DDR | 77 K | 6.96% | CryoGuard modeled electronic power, with the paper’s cooling contribution removed. |

These are editable scenario assumptions, not demonstrated power reductions for a complete commercial GPU or HBM package.

The default RT allocation is illustrative and should be replaced with measurements or estimates for the system being studied.

## Temperature Controls

Refrigeration calculations use the shared bath temperature as the cold boundary.

Junction temperatures describe the selected operating points. Changing them does not automatically interpolate chip power; remaining-power ratios are separate inputs.

When the bath temperature changes:

- Junctions at the previous bath temperature follow the bath.
- Junctions below the new bath temperature are raised to it.
- Warmer junction settings remain unchanged.

## Assumptions and Limitations

- Comparisons assume equal computational throughput.
- Chip-power reductions and cooling efficiency are independent assumptions.
- Junction temperatures are not calculated from package thermal resistance.
- Nitrogen properties remain manual inputs when bath temperature changes.
- Closed-loop efficiency is an engineering estimate with an editable sensitivity range.
- The closed-loop steady-state model excludes initial cooldown and assumes no nitrogen makeup.
- The TSMC fit is a scenario model, not a verified universal refrigeration law.
- Power ratios derived from literature may not transfer directly to different technologies, workloads, or architectures.

Results are intended for research exploration and sensitivity analysis, rather than as validated predictions for a specific facility.

## Sources and Provenance

The app’s explanatory popovers describe the assumptions, equations, and source boundaries.

The DDR default uses:

**CryoGuard: A Near Refresh-Free Robust DRAM Design for Cryogenic Computing**, ISCA 2021.

https://doi.org/10.1109/ISCA52012.2021.00056

Its modeled result retains 74.1% of RT power including refrigeration. Removing the paper’s cooling factor gives:

**Electronic power remaining = 74.1% / 10.65 ≈ 6.96%.**

GPU and HBM defaults are transferred assumptions. The Anhui/Chuzhou production input and TSMC scenario fit follow the reference data used in this analysis; their provenance and applicability should be reviewed before publication or engineering use.
