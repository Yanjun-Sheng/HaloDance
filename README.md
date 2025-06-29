# HaloDance
HaloDance is a publicly released suite of N-body simulation results that captures the complex gravitational interaction between the Milky Way (MW) and the Large Magellanic Cloud (LMC) across a broad parameter space. Designed to explore the dynamical response of the MW halo to the infall of the LMC, this simulation grid provides a valuable resource for studying the formation of stellar streams, halo anisotropy, and time-dependent signatures in the Galactic potential.

The simulations systematically vary key parameters of the MW–LMC system (the LMC follows a first-infall orbital scenario)—including the total mass of the MW and LMC, halo concentration, and shape—to reflect the current observational uncertainties and theoretical models. Each simulation tracks the evolution of both the MW and LMC over the last 2 Gyrs, with outputs including full 6D phase-space data of dark matter particles. 

Key Features:

1. Coverage of about 3,000 parameter combinations using Latin Hypercube sampling (2,000 isotropic MW halo with $\beta(r)=0$ and 1,000 anisotropic MW halo with a radially varying anisotropy profile $\beta(r)=-0.15-0.2 \alpha(r); \alpha(r)=\frac{\mathrm{d} \ln \rho(r)}{d \ln r}$)

2. Snapshots from 101 epochs within the last 2 Gyrs, including present-day configurations

3. Detailed modeling of the LMC-induced perturbations in the MW DM halo

4. Designed for use in stream modeling, halo kinematic studies, and forward modeling of Gaia-era data
  
5. For the simulation details, please refer to Sheng & Ting & Xue 2025 

Due to the storage demands of the full N-body snapshots (approximately 5 TB in total), this GitHub repository provides only the Basis Function Expansion (BFE) representations of each snapshot, which capture the global phase-space structure of the halo in a compact and flexible format.

If you are interested in accessing the full N-body snapshot data, please feel free to reach out to Yanjun Sheng at Yanjun.Sheng@anu.edu.au or yanjunsheng1998@gmail.com.

HaloDance invites the community to explore how the Galactic halo "dances" to the gravitational rhythm set by the LMC. 


