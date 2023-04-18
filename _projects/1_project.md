---
layout: page
title: American Options Pricing Using Monte Carlo Method
description: Cornell University 
img: assets/img/American Options Pricing Using Monte Carlo Method.png   
importance: 1
category: Quantitative Finance
---

The pricing of American options is usually limited to the numerical analysis approach. Three types of numerical methods are commonly used for option pricing, including the binomial method, finite-difference method, and Monte Carlo simulation method. The Monte Carlo simulation methods have a significant advantage in dealing with higher dimensional cases and path-dependent option pricing problems.

This project aims to investigate three Monte Carlo simulation methods for pricing American options with a single asset underlying. This project will compare the approaches of Longstaff and Schwartz (2001) and Rogers (2002) with that of Broadie and Glasserman (1997). This project implements each method with the algorithm to generate numerical results and find that Longstaff-Schwartz's and Rogers' methods perform better regarding the accuracy and computational time.

Among the three methods, Roger's and Longstaff-Schwartz’s methods initially tend to give the point estimate. In comparison, the Broadie-Glasserman approach leads us to have a boundary for the option price. After converting the upper and lower edge for the American option price to a point estimator and comparing it with the first two methods, we found that Roger's approach and the Longstaff-Schwartz method have better accuracy. 

For Rogers’ method, the results obtained when the option is in the money are very close to the true values with good confidence intervals. When the option is out of the money, we obtain wider confidence interval. 

Apart from the price estimator, we find that the computational time is less for Roger's and Longstaff-Schwartz methods. Since the number of nodes in the tree increases for the Broadie-Glasserman method, the time steps will increase exponentially. It is time-consuming or even unavailable to estimate the derivative price when many exercising opportunities exist.
