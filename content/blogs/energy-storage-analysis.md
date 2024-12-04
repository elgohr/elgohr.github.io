---
title: "Analysis: Maximize Energy Savings with Solar, Storage, and Smart Contracts"
date: 2024-11-02
draft: false
tags:
  - HEMS
  - Fronius
  - PV
  - Smart Contract
description: "Howto maximize energy savings"
toc: true
---

Before starting the project I created a rough analysis whether the effort could be worth it.  
In the end the analysis was also influenced by the fact that I wanted to play around with the new tech 😉  

## Where to start?
I took the decision to start looking at three main factors:

1) How much energy do we use and when?
2) How much energy do we generate?
3) Would the times match dynamic energy prices?

In this way I created an application that is querying the Fronius API for logging energy usage to a local database.  
I took a MariaDB as it was the most easy to install on my home setup (otherwise I would have favoured a timeseries database).  
The plan was to learn from the past and create something like a moving window for a typical day (energy consumption per hour).  
It would have to be a moving windows, as I would expect energy usage in winter to be different than in sommer (more light...).  

For predicting the energy generation I had a look at the API of [forecast.solar](https://api.forecast.solar), which turned out to create good results.  

Afterwards I realized that the energy price is set two days in advance. I wasn't expecting this, as energy is traded on an exchange and I was expecting something like a stock market. But this makes my life much easier. In this way I created a logger for the [EPEX-Spot market](https://github.com/elgohr/EPEX-DE-History) to compare this data later.

As you can see above, two out of three values are estimations.  
In this way the challenge would be to make the most educated guess possible.

In the end I create a simulation that would provide the optimal schedule of loading/unloading for a day.

No simulation was unloading. This could be explained easily, because I would get 0.08 Euro per kWh.
In comparison I would pay around 0.19 Euro as the energy price and additional 0.21 Euro taxes and service fee (not including energy provider price).
