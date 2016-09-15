FRIDA: FRI-based DOA Estimation for Arbitrary Array Layout
==================================

This repository contains all the code to reproduce the results of the paper
[*FRIDA: FRI-based DOA Estimation for Arbitrary Array Layout*](http://lcav.epfl.ch).

*FRIDA* is a new algorithm for direction of arrival (DOA) estimation
for acoustic sources. This repository contains a python implementation
of the algorithm, as well as five conventional methods: MUSIC, SRP-PHAT, CSSM,
WAVES, and TOPS (in the `doa` folder).

A number of scripts were written to evaluate the performance of FRIDA and the
other algorithms in different scenarios. Monte-Carlo simulations were used to
study the noise robustness and the minimum angle of separation for close source
resolution (`figure_doa_separation.py`, `figure_doa_synthetic.py`).  A number
of experiment on recorded data were done and the scripts for processing this
data are also available (`figure_doa_experiment.py`,
`figure_doa_9_mics_10_src.py`).

We are available for any question or request relating to either the code
or the theory behind it. Just ask!

Abstract
--------

In this paper we present FRIDA --- an algorithm for estimating directions of
arrival of multiple wideband sound sources. FRIDA combines multi-band
information coherently and achieves state-of-the-art resolution at extremely
low signal-to-noise ratios. It works for arbitrary array layouts, but unlike
the various steered response power and subspace methods, it does not require a
grid search. FRIDA leverages recent advances in sampling signals with a finite
rate of innovation. It is based on the insight that for any array layout, the
entries of the spatial covariance matrix can be linearly transformed into a
uniformly sampled sum of sinusoids.

Authors
-------

Hanjie Pan, Robin Scheibler, Eric Bezzam, and Martin Vetterli are with 
Audiovisual Communications Laboratory ([LCAV](http://lcav.epfl.ch)) at 
[EPFL](http://www.epfl.ch).

Ivan Dokmanić is with Institut Langevin, CNRS, EsPCI Paris, PSL Research University.

<img src="http://lcav.epfl.ch/files/content/sites/lcav/files/images/Home/LCAV_anim_200.gif">

#### Contact

[Robin Scheibler](mailto:robin[dot]scheibler[at]epfl[dot]ch) <br>
EPFL-IC-LCAV <br>
BC Building <br>
Station 14 <br>
1015 Lausanne

Recreate the figures and sound samples
--------------------------------------

In a terminal, run the following script.

    ./make_all_figures.sh

Alternatively, start an ipython cluster

    ipcluster start -n <number_workers>
    
and then type in the following commands in an ipython shell.

    # Simulation with different SNR values
    %run figure_doa_synthetic.py -f <filename>
    %run figure_doa_synthetic_plot.py -f <filename>

    # Simulation of closely spaced sources
    %run figure_doa_separation.py -f <filename>
    %run figure_doa_separation_plot.py -f <filename>

    # Experiment on speech recordings
    %run figure_doa_experiment.py -f <filename>
    %run figure_doa_experiment_plot.py -f <filename>

    # Experiment with 10 loudspeakers and 9 microphones
    %run figure_doa_9_mics_10_src.py -o <filename>
    %run figure_doa_9_mics_10_src_plot.py -f <filename>

The data is saved in the `data` folder and the figures generated are collected in `figures`.

Data used in the paper
----------------------

The output from the simulation and processing that
was used for the figures in the paper is stored in
the repository in the following files.

    # Simulation with different SNR values
    data/20160911-035215_doa_synthetic.npz
    data/20160911-161112_doa_synthetic.npz
    data/20160911-175127_doa_synthetic.npz
    data/20160911-192530_doa_synthetic.npz
    data/20160911-225325_doa_synthetic.npz

    # Simulation of closely spaced sources
    data/20160910-192848_doa_separation.npz

    # Experiment on speech recordings
    data/20160909-203344_doa_experiment.npz

    # Experiment with 10 loudspeakers and 9 microphones
    data/20160913-011415_doa_9_mics_10_src.npz

Recorded Data
-------------

The recorded samples are stored in the `recordings` folder.
Detailed description and instructions are provided along the data.

Overview of results
-------------------

We implemented for comparison five algorithms: incoherent MUSIC, SRP-PHAT, CSSM, WAVES, and TOPS.

### Influence of Noise (Fig. 1A)

We compare the robustness to noise of the different algorithms when a single source is present.

<img src="https://dl.dropboxusercontent.com/u/78009186/images/FRIDA/experiment_snr_synthetic.png" height="300">

### Resolving power (Fig. 1B)

We study the resolution power of the different algorithms. How close can two sources become
before the algorithm breaks down.

<img src="https://dl.dropboxusercontent.com/u/78009186/images/FRIDA/experiment_minimum_separation.png" height="300">

### Experiment on speech data (Fig. 2C)

We record signals from 8 loudspeakers with 1, 2, or 3 sources active simultaneously. We use
the algorithm to reconstruct the DOA and plot the statistics of the error.

<img src="https://dl.dropboxusercontent.com/u/78009186/images/FRIDA/experiment_error_box.png" height="300">

### Experiment with more sources than microphone (Fig. 2D)

FRIDA can identifies DOA of more sources than it uses microphones. We demonstrate
this by playing 10 loudspeakers simultaneously and recovering all DOA with only
9 microphones.

<img src="https://dl.dropboxusercontent.com/u/78009186/images/FRIDA/experiment_9_mics_10_src.png" height="400">


Dependencies
------------

The script `system_install.sh` was used to install all the required software on a blank UBUNTU Xenial server.

* A working distribution of [Python 2.7](https://www.python.org/downloads/).
* [Numpy](http://www.numpy.org/), [Scipy](http://www.scipy.org/)
* We use the distribution [anaconda](https://store.continuum.io/cshop/anaconda/) to simplify the setup of the environment.
* Computations are very heavy and we use the
  [MKL](https://store.continuum.io/cshop/mkl-optimizations/) extension of
  Anaconda to speed things up. There is a [free license](https://store.continuum.io/cshop/academicanaconda) for academics.
* We used ipyparallel and joblib for parallel computations.
* [matplotlib](http://matplotlib.org) and [seaborn](https://stanford.edu/~mwaskom/software/seaborn/index.html#) for plotting the results.

The pyroomacoustics is used for STFT, fractionnal delay filters, microphone arrays generation, and some more.

    pip install git+https://github.com/LCAV/pyroomacoustics

List of standard packages needed

    numpy, scipy, pandas, ipyparallel, seaborn, zmq, joblib

In addition the two following libraries are used for resample and processing of wav files

    scikits.audiolab, sickits.samplerate

They require install of shared libraries

    # Ubuntu code
    apt-get install libsndfile1 libsndfile1-dev libsamplerate0 libsamplerate0-dev  # Ubuntu

    # OS X install
    brew install libsndfile
    brew install libsamplerate



Systems Tested
--------------

###Linux

| Machine | ICCLUSTER EPFL                  |
|---------|---------------------------------|
| System  | Ubuntu 16.04.5                  |
| CPU     | Intel Xeon E5-2680 v3 (Haswell) |
| RAM     | 64 GB                           |

###OS X

| Machine | MacBook Pro Retina 15-inch, Early 2013 |
|---------|----------------------------------------|
| System  | OS X Maverick 10.11.6                  |
| CPU     | Intel Core i7                          |
| RAM     | 16 GB                                  |

    System Info:
    ------------
    Darwin 15.6.0 Darwin Kernel Version 15.6.0: Mon Aug 29 20:21:34 PDT 2016; root:xnu-3248.60.11~1/RELEASE_X86_64 x86_64

    Python Info:
    ------------
    Python 2.7.11 :: Anaconda custom (x86_64)

    Python Packages Info (conda)
    ----------------------------
    # packages in environment at /Users/scheibler/anaconda:
    accelerate                2.0.2              np110py27_p0  
    accelerate_cudalib        2.0                           0  
    anaconda                  custom                   py27_0  
    ipyparallel               5.0.1                    py27_0  
    ipython                   4.2.0                    py27_0  
    ipython-notebook          4.0.4                    py27_0  
    ipython-qtconsole         4.0.1                    py27_0  
    ipython_genutils          0.1.0                    py27_0  
    joblib                    0.9.4                    py27_0  
    mkl                       11.3.3                        0  
    mkl-rt                    11.1                         p0  
    mkl-service               1.1.2                    py27_2  
    mklfft                    2.1                np110py27_p0  
    numpy                     1.11.0                    <pip>
    numpy                     1.11.1                   py27_0  
    numpydoc                  0.5                       <pip>
    pandas                    0.18.1              np111py27_0  
    pyzmq                     15.2.0                   py27_1  
    scikits.audiolab          0.11.0                    <pip>
    scikits.samplerate        0.3.3                     <pip>
    scipy                     0.17.0                    <pip>
    scipy                     0.18.0              np111py27_0  
    seaborn                   0.7.1                    py27_0  
    seaborn                   0.7.1                     <pip>

License
-------

Copyright (c) 2016, Hanjie Pan, Robin Scheibler, Eric Bezzam, Ivan Dokmanić, Martin Vetterli

All the code in this repository is under a CC-BY-SA 4.0 license.

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/80x15.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">FRIDA</span> by <a xmlns:cc="http://creativecommons.org/ns#" href="http://lcav.epfl.ch" property="cc:attributionName" rel="cc:attributionURL">LCAV-EPFL</a> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.<br />Based on a work at <a xmlns:dct="http://purl.org/dc/terms/" href="http://go.epfl.ch/FRIDA" rel="dct:source">http://go.epfl.ch/FRIDA</a>.
