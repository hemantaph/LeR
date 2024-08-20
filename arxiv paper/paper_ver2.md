---
title: '***ler*** : LVK (LIGO-Virgo-KAGRA collaboration) Event (compact-binary mergers) Rate calculator and simulator'
tags:
  - Python
  - astrophysics
  - statistics
  - gravitational waves
  - LIGO
authors:
  - name: Phurailatpam Hemantakumar
    orcid: 0000-0000-0000-0000
    # equal-contrib: true
    affiliation: "1"
  - name: Anupreeta More
    # equal-contrib: False
    affiliation: "2,3"
  - name: Harsh Narola
    # equal-contrib: False
    affiliation: "4,5"
  - name: Ng Chung Yin (Leo)
    # equal-contrib: False 
    affiliation: "1"
  - name: Justin Janquart
    # equal-contrib: False 
    affiliation: "4,5"
  - name: Chris Van Den Broeck
    # equal-contrib: False 
    affiliation: "4,5"
  - name: Otto Akseli HANNUKSELA 
    # equal-contrib: False 
    affiliation: "1"
  - name: Neha Singh
    # equal-contrib: False
    affiliation: "6"
  - name: David Keitel
    # equal-contrib: False
    affiliation: "6"
affiliations:
  - name: Department of Physics, The Chinese University of Hong Kong, Shatin, New Territories, Hong Kong
    index: 1
  - name:  The Inter-University Centre for Astronomy and Astrophysics (IUCAA), Post Bag 4, Ganeshkhind, Pune 411007, India
    index: 2
  - name: Kavli Institute for the Physics and Mathematics of the Universe (IPMU), 5-1-5 Kashiwanoha, Kashiwa-shi, Chiba 277-8583, Japan
    index: 3
  - name: Department of Physics, Utrecht University, Princetonplein 1, 3584 CC Utrecht, The Netherlands
    index: 4
  - name: Nikhef – National Institute for Subatomic Physics, Science Park, 1098 XG Amsterdam, The Netherlands
    index: 5
  - name: Departament de F\'isica, Universitat de les Illes Balears, IAC3-IEEC, Crta. Valldemossa km 7.5, E-07122 Palma, Spain
    index: 6
  
date: 19 June 2024
bibliography: paper.bib
---

### Summary

Gravitational waves (GWs) are ripples in the fabric of space and time caused by the acceleration of unevenly distributed mass or masses. Observable GWs are created especially during the violent cosmic events of merging compact binaries, such as 'binary black holes' (BBH), 'binary neutron stars' (BNS), and 'neutron star and black hole pair' (NSBH). GWs emitted by these events can be distorted or magnified by the gravitational fields of massive objects such as galaxies or galaxy clusters, a phenomenon known as gravitational lensing. Profound comprehension of gravitational lensing's impact on GW signals is imperative to their accurate interpretation and the extraction of astrophysical insights therein. For this purpose, statistical modeling of GWs lensing can provide valuable insights into the properties of the lensing objects and GW sources. Such statistics require accurate and efficient means to calculate the detectable lensing rates, which depend on up-to-date modeling and implementation of lens and source properties and their distribution. The outcomes of these computational analyses not only contribute to generating dependable forecasts but also play an important role in validating forthcoming lensing events [arXiv:2306.03827 [gr-qc]](https://arxiv.org/abs/2306.03827).

Obtaining precise outcomes in statistical analyses of this nature necessitates the utilization of large-scale sampling, often numbering in the millions. However, this process is computationally demanding. The ***ler*** framework employs innovative techniques to vectorize and parallelize the workflow efficiently. Additionally, it simplifies calculations through the utilization of interpolation and linear algebra. A further challenge entailed the seamless integration of disparate statistical components in a modular manner that facilitates adaptability, upgradability, and extendability. The ***ler*** framework effectively addresses these complexities, offering user-friendly features that ensure seamless compatibility with other associated software packages.

### Statement of need

***ler*** is a statistical-based Python package whose core function is designed for the computation of detectable rates pertaining to both lensed and unlensed gravitational wave (GW) events. The holistic functionality of the package is built upon leveraging array operations and linear algebra from the *numpy* library, complemented by interpolation methods from *scipy* and Python’s native *multiprocessing* capabilities. The software's efficiency is notably enhanced by the *numba* library's Just-In-Time (*njit*, n: no-python mode) compilation, which dynamically converts Python and NumPy code into machine code, thereby optimizing performance in numerical computations involving extensive loops and array operations. Predominantly, ***ler*** employs the inverse transform sampling method, utilizing interpolated inverse cumulative distribution functions (CDFs). This approach eschews the need for rejection sampling, which often requires numerous loops and the handling of relatively tedious probability density functions (PDFs) of the sampled parameters. The overall design of the package optimizes both speed and functionality while upholding user-friendliness. The ler software has been developed to cater to the requirements of both the LIGO-Virgo-KAGRA Scientific Collaboration and research scholars engaged in astrophysics studies. It is currently used in generating detectable lensing events and GW lensing rates with the available information on current and future detectors. The results will predict the capacity of various detectors to detect and study such lensing events. Statistics generated from ***ler*** will be used in event validation for the ongoing effort to detect lensed GWs. Lastly, ***ler*** was designed with upgradability in mind to include additional statistics as required by the related research.

### Design and Structure

The architecture of the ***ler*** API is deliberately organized such that each distinct functionality holds its own significance in scientific research. Simultaneously, these functionalities seamlessly integrate and can be employed collectively based on specific research requirements. Key features of ***ler*** and its dependencies can be summarized as follows:

- Sampling Gravitational Wave (GW) Source Properties:
    * For the unlensed events, The sampling distribution $(\,R_m^U(z_s)\,)$ for the source's redshift $(\,z_s\,)$ is derived from the merger rate density of compact binaries, which, in turn, is based on the star formation rate. The code is meticulously designed to enable the straightforward integration of future updates or user-specified distributions of these sources.
    * The process of sampling both intrinsic and extrinsic parameters of GW sources, represented by $\theta_i$, utilizes the prior distributions $(P(\theta_i))$ available within the *gwcosmo* and *bilby* Python packages. Before parameterizing the rate calculation, the software allows users the flexibility to manually alter any pertinent parameters as required. The said feature is employed when one doesn't specifically have a prior distribution for a particular parameter.

- Sampling of lens galaxies attributes and source red-shifts:
    * For lensed case, the source redshift $(z_s)$ is sampled under the strong lensing condition $(\text{SL})$, based on the precomputed probability of strong lensing with source at $z_s$ $(\text{optical depth: }P\left(\text{SL}|z_s\right)\,)$. This probability can be recalculated for specified configurations of lens galaxies, leveraging *multiprocessing* and *njit* functionalities for enhanced efficiency.
    * The package utilizes the Elliptical Power Law with external shear (EPL+Shear) model for galaxy parameters $(\theta_L)$ sampling, following [Wierda et. al 2021](https://arxiv.org/abs/2106.06303). Rejection sampling is applied on the above samples on condition that whether the event is strongly lensed or not, $P\left(\text{SL}|z_s,\theta_L\right)$.

- Generation of image properties:
    * Source position $(\beta)$ is sampled from the caustic in the source plane.
    * Sampled lens properties and source position is fed in *Lenstronomy* to generate properties of the images. This is the slowest part of the entire simulation, which ***ler*** tackles through parallelization with multiprocessing.
    * Image properties like magnification $(\mu_i)$ and time delay ($\Delta t_i$) modifies the original source signal strength, changing the signal-to-noise ratio SNR and our ability to detect.

- Calculation of Detectable Merger Rates Per Year:
    * The calculation of rates necessitates integration over simulated events that meet specific detection criteria. This process includes computing SNRs $(\rho)$ for each event or its lensed images, followed by an assessment against a predetermined threshold(s) $(\rho_{th})$.
    * SNR calculations are optimized using [*gwsnr*](https://github.com/hemantaph/gwsnr), leveraging interpolation and multiprocessing for accuracy and speed.
    * Simulated events and rate results, along with input configurations, are systematically archived for easy access and future analysis. Additionally, all interpolators used in the process are preserved for future applications.

# Equations

$\textbf{Detectable Unlensed rates:}$

\begin{equation*}
\begin{split}
R_U = \int & dz_s \frac{dV_c}{dz_s}\frac{R_m(z_s)}{1+z_s}\left\{\Theta[\rho(z_s,\theta)-\rho_{th}] P(\theta) d\theta \right\}
\end{split}
\end{equation*}

* $z_s$: GW source redshift, $\frac{dV_c}{dz_s}$: Differential co-moving volume, $\frac{1}{1+z_s}$: Time dilation correction factor, $R_m(z_s)$: source frame merger rate density, $\theta$: GW source parameters, $P$: probability distribution, $\rho$: SNR, $\rho_{th}$: SNR threshold, $\Theta$: Heaviside function to select detectable events.

$\textbf{Detectable Lensed rates:}$

\begin{equation*}
\begin{split}
R_L = \int & dz_s \frac{dV_c}{dz_s}\tau(z_s)\frac{R_m^L(z_s)}{1+z_s} \,\mathcal{O}_{images}(z_s,\theta,\mu_i,\Delta t_i, \rho_{th}) \, \\ 
& \, P(\theta) P(\theta_L|\text{SL},z_s) P(\beta|\text{SL}) d\theta d\beta d\theta_L dz_s 
\end{split}
\end{equation*}

* $\tau(z_s)$: Optical-depth of strong lensing, $\theta_L$: lens parameters, $\beta$: image properties, $\mu$: image magnification, $\Delta t$: image time delay, $\mathcal{O}$: logical OR operator applied across all $\Theta_i$ of the images, $\text{SL}$: strong lensing condition.

# Acknowledgements

The authors express their sincere appreciation for the significant contributions that have been instrumental in completing this research. Special thanks are extended to the academic advisors for their invaluable guidance and steadfast support. The collaborative efforts and enriching discussions with research colleagues significantly enhanced the study's quality. Acknowledgement is given to the Department of Physics, The Chinese University of Hong Kong, for the Postgraduate Studentship that facilitated this research. Further gratitude is extended to the Netherlands Organisation for Scientific Research (NWO) for their support. The authors also recognize the contributions of individuals who added empirical depth to this work. Appreciation is conveyed for the computational resources provided by the LIGO Laboratory, supported by National Science Foundation Grants No. PHY-0757058 and No. PHY-0823459.

# References