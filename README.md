# PyRADS
PyRADS is the Python line-by-line RADiation model for planetary atmosphereS. PyRADS is a radiation code that can provide line-by-line spectral resolution, yet is written in Python and so is flexible enough to be useful in teaching.

For Earth-like atmospheres, PyRADS currently uses HITRAN 2016 line lists (http://hitran.org/) and the MTCKD continuum model (http://rtweb.aer.com/continuum_frame.html).

Currently, PyRADS is only valid for longwave calculations (no scattering).

References:

(1) Koll & Cronin, 2018, https://doi.org/10.1073/pnas.1809868115.

# Installation
1) Download to your own computer.

2) Manually compile the MTCKD model:
- cd $PyRADS/DATA/MT_CKD_continuum/cntnm.H2O_N2/build
- (on a Mac) make -f make_cntnm osxGNUdbl
- (on a Mac if you are using gfortran installed with conda) make -f make_cntnm osxGNUCONDAdbl

3) Manually install the pyDISORT wrapper, which solves
  the radiative transfer equations with scattering (=in the shortwave).
  Steps marked with (x) use conda to create a separate environment, and are optional.
  
- (x) conda create --clone base --name base_w_pyDISORT
- (x) conda activate base_w_pyDISORT
- cd $PyRADS/pyDISORT-master
- python setup.py install

  To test whether pyDISORT was successfully installed:
- cd $PyRADS/pyDISORT-master/test
- python test_disort.py
- python test_Rayleigh.py

  If the installation failed, the test scripts will return "ImportError: No module named disort".
  If the installation worked, the test scripts will print a large slew of output.
  If using a separate conda environment, subsequently remember to load that environment before running pyDISORT.

4) Run test scripts

To compute outgoing longwave radiation (OLR) in W/m2 for a given surface temperature:
- cd $PyRADS/Test01.olr
- python compute_olr_h2o.py

To compute OLRs for a set of surface temperatures and save the resulting output to txt:
- cd $PyRADS/Test02.runaway
- python compute_olr_h2o.01.100RH.py

To compute SW fluxes in W/m2 for a given surface temperature (over a *limited* part of the solar spectrum) and save the resulting output to txt file:
- cd $PyRADS/Test03.sw
- python compute_sw_h2o.py 1000. 1100. 0.01 300. .

NOTE: running pyDISORT over the entire solar+thermal spectrum at line-by-line resolution is computationally very costly. The brute-force approach here is feasible with sufficient computational resources, e.g., splitting the solar spectrum over many parallel processors.

NOTE: resolution in test scripts was chosen for relative speed, not accuracy. For research-grade output and model intercomparisons, vertical and spectral resolution need to be increased. For some reference values, see Methods in Koll & Cronin (2018).


# Requirements
Python 2 or 3 with numpy and scipy.

For the MTCKD continuum model: gmake and gfortran.

For the pyDISORT radiation solver: see https://github.com/chanGimeno/pyDISORT.

# Acknowledgements
PyRADS makes use of HITRAN 2016 line lists (http://hitran.org/), AER's MTCKD continuum model (http://rtweb.aer.com/continuum_frame.html), and the PyTran script published by Ray Pierrehumbert as part of the courseware for "Principles of Planetary Climates" (https://geosci.uchicago.edu/~rtp1/PrinciplesPlanetaryClimate/). Brian Rose (http://www.atmos.albany.edu/facstaff/brose/) has helped improve the code.
