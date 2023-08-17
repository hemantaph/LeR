# LeR
[![DOI](https://zenodo.org/badge/626733473.svg)](https://zenodo.org/badge/latestdoi/626733473) [![PyPI version](https://badge.fury.io/py/ler.svg)](https://badge.fury.io/py/ler) [![DOCS](https://readthedocs.org/projects/ler/badge/?version=latest)](https://ler.readthedocs.io/en/latest/)

`LeR` is a statistical-based python package whose core function is to calculate detectable rates of both lensing and unlensed GW events. This calculation is very much dependent on the other functionality of the package, which can be subdivided into three parts; 1. Sampling of compact-binary source properties, 2. Sampling of lens galaxy characteristics and 3. Solving the lens equation to get image properties of the source. The package as a whole relies on `numpy` array operation and linear algebra, `scipy` interpolation and `multiprocessing` functionality of python to increase speed and functionality without compromising on the ease of use. The API of `LeR` is structured such that each functionality mentioned stands in its own right for scientific research but also can be used together as needed. Key features of `LeR` and its dependencies can be summarized as follows,

- Detectable merger rates: 
    * Calculation not only relies on the properties of simulated events but also on detectability provided by the condition of the GW detectors. For this, the optimal signal-to-noise ratio (SNR) is calculated for each of the simulated events and it can be computationally expensive. This is mitigated because `LeR` relies on [`gwsnr`]{https://github.com/hemantaph/gwsnr/tree/main} for efficient and rapid calculation of SNRs. Due to the prowess of `gwsnr`, rate calculation can also be done both for present and future detectors with customizable sensitivities. 
    * Merger rates of both the simulated unlensed and lensed events can be calculated and compared. 
- Sampling GW sources:
    * Distribution of the source's red-shift is based on the merger rate density of compact binaries, which can be BBH, BNS, primordial black holes (PBHs) etc. The code is designed to accommodate easy updates or additions of such distribution by the users in the future. 
    * Sampling of BBH masses is done using `gwcosmo` following the powerlaw+peak model. Other related properties are sampled from available priors of `bilby`. The user can manually replace any before feeding the parameters in for rate computation.
- Sampling of lens galaxies:
    * Lens distribution follows [(Oguri et al. 2018](https://arxiv.org/abs/1807.02584). It depends on the sampled source red-shifts and also on the optical depth.
    * `LeR` employs the Elliptical Power Law model with the external shear (EPL+Shear) model for sampling other galaxy features, which is available in the `Lenstronomy` package.
    * Rejection sampling is applied on the above samples on condition that whether the event is strongly lensed or not.
- Generation of image properties:
    * Source position is sampled from the caustic in the source plane.
    * Sampled lens properties and source position is fed in `Lenstronomy` to generate properties of the images.
    * Properties like magnification and time delay are essential as it modifies the source signal strength, changing the SNR and detection ability.
    * `LeR` can handle both super-threshold and sub-threshold events in picking detectable events and rate computation.

`LeR` was written to be used by both LIGO scientific collaboration and research students for related works in astrophysics. It is currently used in generating detectable lensing events and GW lensing rates with the available information on current and future detectors. The results will predict the feasibility of various detectors for detecting and studying such lensing events. Statistics generated from `LeR` will be used in event validation of the ongoing effort to detect lensed gravitational waves. Lastly, `LeR` was designed with upgradability in mind to include additional statistics as required by the related research. 

# Installation

Follow the installation instruction at [ler.readthedoc](https://ler.readthedocs.io/en/latest/installation.html)
