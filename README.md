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
