<p align="center">
  <img src="assets/halodance_cat.png" alt="HaloDance Logo" width="300"/>
</p>

# HaloDance

HaloDance is a public suite of N-body simulations of the Milky Way (MW) and the Large Magellanic Cloud (LMC).
The suite shows how the MW dark-matter halo responds to the infall of the LMC, over a wide range of model parameters.
Use it to study stellar streams, halo anisotropy, and time-dependent signatures in the Galactic potential.

The LMC is on its first infall in all models. Each simulation follows the MW and the LMC for the last 2 Gyr (Section 3).
The outputs give the full 6D phase-space data of the particles.

## 1. Key features

1. About 2,848 parameter combinations, from Latin Hypercube sampling of four parameters: the virial mass of the MW NFW halo, $M_{\mathrm{MW}}$; its concentration, $c$; its flattening, $q$; and the mass of the LMC halo, $M_{\mathrm{LMC}}$.
   - 1,848 models have an isotropic MW halo, $\beta(r) = 0$.
   - 1,000 models have a radially varying anisotropy, $\beta(r) = -0.15 - 0.2\,\alpha(r)$, with $\alpha(r) = \mathrm{d}\ln\rho(r)/\mathrm{d}\ln r$.
2. Snapshots at 101 times in the last 2 Gyr, including the present day.
3. The LMC-induced perturbations of the MW dark-matter halo.
4. Use cases: stream modelling, halo kinematics, and forward modelling of Gaia-era data.
5. For the simulation details, refer to Sheng, Ting & Xue (2025).

## 2. Snapshot access

The full suite needs about 11 TB. Thus we release **6 snapshots of each simulation** (each with about $10^6$ particles, 0.4 Gyr apart) on [AWS S3](https://halo-dance.s3.us-east-1.amazonaws.com).

To download a snapshot, use a command like this:

```bash
wget https://halo-dance.s3.us-east-1.amazonaws.com/isotropic_MW_halo/Model_413/snapshot_000.hdf5
```

For the anisotropic MW models, change the subdirectory from **isotropic_MW_halo** to **anisotropic_MW_halo**.

The number in the S3 folder name `Model_<N>` is the `index` of the model (Section 4): 0 to 1,847 for the isotropic models and 0 to 999 for the anisotropic models.
For example, `isotropic_MW_halo/Model_413` is the isotropic model with `index` = 413.

For the full N-body snapshots (also the high-resolution snapshots with about $10^7$ particles), contact Yanjun Sheng at Yanjun.Sheng@anu.edu.au or yanjunsheng1998@gmail.com.

## 3. Snapshot index and time

| Snapshot index | Code time | Lookback time (Gyr) |
|---|---|---|
| 000 | 0.0 | 2.0 (initial) |
| 020 | 0.4 | 1.6 |
| 040 | 0.8 | 1.2 |
| 060 | 1.2 | 0.8 |
| 080 | 1.6 | 0.4 |
| 100 | 2.0 | 0.0 (present day) |

The simulations use the GADGET-4 units 1 kpc, $10^{10}\,M_\odot$ and 1 km/s.
The code time unit is 1 kpc / (1 km/s) = 0.978 Gyr. Thus one code unit is about 1 Gyr, and the run covers code time 0 to 2.0 (1.96 Gyr).
The tables in this folder give the time in code units (columns `time_Gyr` and `lookback_Gyr`).

## 4. Definitions

| Term | Meaning |
|---|---|
| `index` | The model number in this release: the S3 folder `Model_<index>` and the row of the model in all tables (0, 1, 2, ...). Use `index` to find a model. |
| `model_id` | The absolute index of the model in the full set of simulation runs (0 to 4,999), before the selection of the released models. It is given for reference only; it is not the S3 folder number. |
| $N_{\mathrm{mw}}$ | The MW ID limit of a model (`mw_id_limit`): the largest particle ID of the MW. It is different for each model. |
| Simulation frame | The coordinates as the snapshot stores them. The origin is not at the MW. |
| MW-centred frame | The frame with its origin at the MW disk centre and at rest with the inner MW (Section 6). |

## 5. Particle types and IDs

Each snapshot has three particle groups. Each group has the datasets `Coordinates`, `Velocities`, `Masses`, `Potential` and `ParticleIDs`.

| Group | Content |
|---|---|
| `PartType1` | Dark matter of the MW halo **and** of the LMC halo, in one group. |
| `PartType2` | MW disk. |
| `PartType3` | MW bulge. |

To separate the MW halo from the LMC halo in `PartType1`, compare the particle ID with the MW ID limit $N_{\mathrm{mw}}$ of the model (column `mw_id_limit` in `<suite>_sim_snapshot_info.csv`):

- If `ParticleIDs` $\le N_{\mathrm{mw}}$, the particle is MW halo dark matter.
- If `ParticleIDs` $> N_{\mathrm{mw}}$, the particle is LMC halo dark matter.

$N_{\mathrm{mw}}$ is different for each model (573,000 to 2,061,000). The particle IDs do not change between snapshots.
Section 8 shows an example.

## 6. Position and velocity of the MW disk and the LMC

For each model and each released snapshot, this folder gives the centre and the velocity of the MW disk and of the LMC.

### 6.1 Method

| Quantity | Calculation |
|---|---|
| MW disk centre | Shrinking sphere on the disk particles (`PartType2`). Start at the disk barycentre with the largest particle radius. Make the radius 5% smaller at each step. Stop at 10 kpc. |
| MW velocity | Mass-weighted mean velocity of the MW halo (`PartType1`, ID $\le N_{\mathrm{mw}}$), disk and bulge particles within $0.1\,R_{200}$ of the MW disk centre ($R_{200}$ of the MW). |
| LMC centre | The same shrinking sphere on the LMC particles (`PartType1`, ID $> N_{\mathrm{mw}}$). |
| LMC velocity | Mass-weighted mean velocity of the LMC particles within $0.1\,R_{200}$ of the LMC centre ($R_{200}$ of the LMC). |

$R_{200}$ is the virial radius of the initial MW or LMC model.
The calculation uses float64 arithmetic. It is the same as the method of the released `*_sim_info.txt` files.

### 6.2 Frame and units

- Positions are in kpc and velocities in km/s, in the simulation frame.
- The relative quantities are LMC minus MW.
- To move a particle to the MW-centred frame, use:

  ```
  x_mw_frame = x - mw_disk_centre
  v_mw_frame = v - mw_velocity
  ```

- This frame has no rotation and no Sun offset. The axes are the simulation axes.

### 6.3 Checks

| Comparison | Largest difference |
|---|---|
| With the orbits measured at all 101 snapshots | 0.036 (isotropic), 0.034 (anisotropic) kpc or km/s |
| With the released present-day LMC offsets (`*_sim_info.txt`) | 0.052 (isotropic), 0.020 (anisotropic) |
| With the released initial LMC offsets | 0.029 (isotropic), 0.011 (anisotropic) |

## 7. Files in this folder

| File | Content |
|---|---|
| `isotropic_sim_snapshot_info.csv`, `anisotropic_sim_snapshot_info.csv` | One row for each model and released snapshot: MW ID limit (Section 5), centres and velocities (Section 6). |
| `halodance_sim_info.h5` | All the values above, as arrays. |
| `isotropic_sim_info.txt`, `anisotropic_sim_info.txt` | The released summary: present-day and initial LMC offset from the MW, and the four parameters. |

### 7.1 Snapshot CSV columns

| Columns | Content |
|---|---|
| `index`, `model_id` | Model (Section 4). |
| `mw_id_limit` | $N_{\mathrm{mw}}$: split of `PartType1` into MW halo and LMC halo (Section 5). |
| `mw_halo_count`, `lmc_halo_count` | Number of MW halo and LMC halo particles in `PartType1`. |
| `snapshot`, `time_Gyr`, `lookback_Gyr` | Snapshot index and code time (Section 3). |
| `M_mw_1e12Msun`, `M_lmc_1e10Msun`, `concentration`, `flattening` | Model parameters. |
| `mw_x`, `mw_y`, `mw_z`, `mw_vx`, `mw_vy`, `mw_vz` | MW disk centre (kpc) and MW velocity (km/s). |
| `lmc_x` ... `lmc_vz` | LMC centre and velocity. |
| `rel_x` ... `rel_vz` | LMC minus MW. |

### 7.2 HDF5 structure

The file has one group for each suite: `isotropic` and `anisotropic`.

| Dataset | Shape | Content |
|---|---|---|
| `index`, `model_id` | (models) | Model (Section 4). |
| `parameters` | (models, 4) | $M_{\mathrm{MW}}$ ($10^{12}\,M_\odot$), $M_{\mathrm{LMC}}$ ($10^{10}\,M_\odot$), $c$, $q$ (names in the attribute `names`). |
| `snapshot`, `time_Gyr`, `lookback_Gyr` | (6) | Snapshot index and code time. |
| `mw_disk_center_pos`, `mw_velocity` | (models, 6, 3) | MW disk centre and MW velocity. |
| `lmc_center_pos`, `lmc_velocity` | (models, 6, 3) | LMC centre and LMC velocity. |
| `lmc_relative_pos`, `lmc_relative_vel` | (models, 6, 3) | LMC minus MW. |
| `particle_ids/mw_id_limit` | (models) | $N_{\mathrm{mw}}$. |
| `particle_ids/mw_halo_count`, `particle_ids/lmc_halo_count` | (models) | Number of MW halo and LMC halo particles in `PartType1`. |

## 8. Example (Python)

```python
import h5py
import numpy as np
import pandas as pd

index, snap = 413, 100                    # model index (S3 folder Model_413) and snapshot
info = pd.read_csv("isotropic_sim_snapshot_info.csv")
row = info[(info["index"] == index) & (info.snapshot == snap)].iloc[0]

n_mw = int(row["mw_id_limit"])
x_mw = row[["mw_x", "mw_y", "mw_z"]].to_numpy(float)
v_mw = row[["mw_vx", "mw_vy", "mw_vz"]].to_numpy(float)

# Downloaded from isotropic_MW_halo/Model_413/snapshot_100.hdf5 (Section 2)
with h5py.File(f"Model_{index}/snapshot_{snap:03d}.hdf5", "r") as f:
    dm = f["PartType1"]
    ids = dm["ParticleIDs"][:]
    pos = dm["Coordinates"][:].astype(float) - x_mw   # MW-centred frame
    vel = dm["Velocities"][:].astype(float) - v_mw

mw_halo = ids <= n_mw     # MW halo dark matter
lmc_halo = ids > n_mw     # LMC halo dark matter
```
