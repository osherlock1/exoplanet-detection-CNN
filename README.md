# Overview
This was the final project for Dr. Meghan Chiovaro's ELE391 Introduction to ML Class at The University of Rhode Island.  This project includes a full pipeline from sourcing raw lightcurve data from NASA's Kepler and K2 Missions, data processing, and training a CNN-based exoplanet classifier.

# Why Use ML for Exoplanet Detection?

The Kepler missions have been instrumental in advancing our understanding of planetary systems outside of our solar system.  The first mission, from 2009 May 2 to 2013 May 11, NASA’s Kepler Space Telescope recorded data from over 200,000 stars.  This new high-quality data allowed scientists and astronomers to increase the number of known confirmed exoplanets.  However, with Kepler’s second mission, K2 recorded an additional 200,000 stars, and the new Transiting Exoplanet Survey Satellite (TESS), launched in 2018, recording tens of millions of stars, it is becoming increasingly impractical for researchers to vet through every light curve to confirm new planets.  To overcome this problem, astronomers began utilizing machine learning’s ability to find complex patterns in data to find the light curves with the highest probability of being caused by an exoplanet.  In our project, we looked to develop a machine-learning algorithm to quickly search through large light curve data sets and find the probability of each light curve being a planet to reduce human workload.


# How Exoplanets are Detected!

Most confirmed exoplanets to date have been discovered using the **transit method**, where astronomers measure how a star's brightness changes over time. This data of the star's brightness is known as a **light curve**.

By continuously monitoring thousands of stars and analyzing their light curves, we can identify these characteristic dips.  Astronomers then anaylize these lightcurves for signs of exoplanets.  Typical signs of exoplanets are:

- **Sharp, symmetric dips** in brightness
- **Periodic dips**, indicating the planets orbit
- **Consistent depth and duration**, this is tied to the planet's size and orbit

![Light Curve gif](assets/lightcurve_gif.gif)

 Manually identifying these dips in millions of light curves is extremely time-consuming.  This is why machine learning has become a popular way for automatic exoplanet detection.

# Data

K2 Data: https://lweb.cfa.harvard.edu/~avanderb/k2.html

Kepler Data: https://archive.stsci.edu/kepler/publiclightcurves.html

# Data Preparation

To prepare the raw Kepler/K2 light curves for input into a machine learning model, we applied several preprocessing steps to enhance the signal of potential transits and make the data suitable for training:

### 1. Phase Folding

Most exoplanet transits are periodic events. For example, if a planet orbits a star every ~10 days, we expect to see a dip in brightness every ~10 days.  
To align these periodic events, we **phase fold** each light curve by folding the time axis over a known or candidate orbital period.  
This results in a plot where multiple transits line up on top of each other, making the dip more prominent.


### 2. Binning

After folding, we **bin** the data into fixed-length segments (e.g., 200 points per curve).  
This step smooths out noise and creates a uniform input size for the CNN, which expects fixed-size 1D vectors.

We typically use **equal-width binning**, where the folded phase curve is divided into equal intervals, and the median or average flux in each bin is computed.

### 3. Normalization

Each light curve is **normalized** using standard scaling to have zero mean and unit variance.  
This prevents the model from learning irrelevant differences in baseline brightness or flux units, and accelerates convergence during training.

### 4. Outlier Removal

For robustness, we optionally remove NaNs, flagged data points, and outliers outside of a plausible flux range to account for instrument error and random particle phenomenon.  

---

These steps transform raw, irregular light curves into clean, aligned, and compact representations that emphasize transit features—ideal for feeding into a CNN-based classifier.

# Why a 1D CNN?

Although light‐curve data is sequential, a 1D convolutional neural network offers several advantages for exoplanet transit detection:

 **Local Pattern Extraction**  
   - Transits manifest as brief, localized dips in flux.  
   - Convolutions act as sliding “dip detectors” that can learn those short‐duration features directly.

 **Parameter Efficiency & Regularization**  
   - A small number of filters can cover a wide range of local patterns, reducing the total learnable parameters.  
   - Combined with pooling and dropout, CNNs often generalize better on moderate‑sized datasets.

 **Proven Success in the Literature**  
   - The **Astronet** model (Shallue & Vanderburg, 2018) demonstrated state‑of‑the‑art Kepler transit classification using a 1D CNN.  
   - Subsequent works (e.g., [Ansdell et al. 2018], [Pearson et al. 2019]) have built on this design, confirming its robustness.


# Results

After training, we evaluated the best model on the test set (160 samples). The key performance metrics are:

### Classification Report

| Class       | Precision | Recall | F1‑Score | Support |
|-------------|-----------|--------|----------|---------|
| No Transit  | 0.99      | 0.95   | 0.97     | 85      |
| Transit     | 0.95      | 0.99   | 0.97     | 75      |
| **Accuracy**    | —         | —      | **0.97**   | 160     |
| Macro Avg   | 0.97      | 0.97   | 0.97     | 160     |
| Weighted Avg| 0.97      | 0.97   | 0.97     | 160     |

- **Overall accuracy**: 97%  
- Both classes achieve an F1‑score of 0.97, indicating a strong balance between precision and recall.

