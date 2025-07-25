<p align="center">
  <img src="assets/halodance_cat.png" alt="HaloDance Logo" width="300"/>
</p>

# HaloDance
HaloDance is a publicly released suite of N-body simulation results that captures the complex gravitational interaction between the Milky Way (MW) and the Large Magellanic Cloud (LMC) across a broad parameter space. Designed to explore the dynamical response of the MW halo to the infall of the LMC, this simulation grid provides a valuable resource for studying the formation of stellar streams, halo anisotropy, and time-dependent signatures in the Galactic potential.

The simulations systematically vary key parameters of the MW–LMC system (the LMC follows a first-infall orbital scenario)—including the total mass of the MW and LMC, halo concentration, and shape—to reflect the current observational uncertainties and theoretical models. Each simulation tracks the evolution of both the MW and LMC over the last 2 Gyrs, with outputs including full 6D phase-space data of dark matter particles. 

### Key Features

1. Coverage of approximately 2,848 parameter combinations spanning the MW’s NFW halo virial mass $M_{\mathrm{MW}}$, concentration $c$, flattening $q$, and the LMC halo mass $M_{\mathrm{LMC}}$, generated using Latin Hypercube sampling. This includes 1,848 models assuming an isotropic MW halo with $\beta(r) = 0$, and 1,000 models adopting a radially varying anisotropy profile $\beta(r) = -0.15 - 0.2,\alpha(r)$, where $\alpha(r) = \frac{\mathrm{d} \ln \rho(r)}{\mathrm{d} \ln r}$.

2. Snapshots from 101 epochs within the last 2 Gyrs, including present-day configurations

3. Detailed modeling of the LMC-induced perturbations in the MW DM halo

4. Designed for use in stream modeling, halo kinematic studies, and forward modeling of Gaia-era data
  
5. For the simulation details, please refer to Sheng & Ting & Xue 2025 

### Snapshot Access

Due to the large storage requirements of the full N-body snapshot suite (~11 TB total), we have uploaded **6 representative snapshots per simulation** (each with $10^6$ particles, spaced by 0.4 Gyr) to [AWS S3](https://halo-dance.s3.us-east-1.amazonaws.com).

To download a snapshot, use a command like:
```bash
wget https://halo-dance.s3.us-east-1.amazonaws.com/isotropic_MW_halo/Model_413/snapshot_000.hdf5
```
Note: For the anisotropic MW models, change the subdirectory from **isotropic_MW_halo** to **anisotropic_MW_halo**.

### Snapshot Index and Lookback Time

The following table shows the correspondence between snapshot indices and lookback times:
| Snapshot Index | Lookback Time (Gyr) |
|----------------|---------------------|
| 000            | 2.0 (initial)       |
| 020            | 1.6                 |
| 040            | 1.2                 |
| 060            | 0.8                 |
| 080            | 0.4                 |
| 100            | 0.0 (present day)   |

If you are interested in accessing the full N-body snapshot data (including the high-res snapshots with $10^{7}$ particles), please feel free to reach out to Yanjun Sheng at Yanjun.Sheng@anu.edu.au or yanjunsheng1998@gmail.com.

HaloDance invites the community to explore how the Galactic halo "dances" to the gravitational rhythm set by the LMC. 


