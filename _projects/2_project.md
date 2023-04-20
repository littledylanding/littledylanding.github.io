---
layout: page
title: Dynamic Parameterization in Pairs Trading
description: the Chinese University of Hong Kong, Hong Kong
img: assets/img/Dynamic Parameterization in Pairs Trading.jpg
importance: 2
category: Quantitative Finance
---
Part I: Motivation 

Traditional cointegration-based pairs trading faces the problem of dichotomous view on pairs relationship and static parameters, exposing it to market risk and structural changes in pairs relationship. Inspired by Prof. Law (2018) and Mr. Li Yiyun’s (2021) work in addressing these issues, this project would like to build a dynamic pairs trading strategy based on their proposals. Moreover, I'm interested in strengthening the performance through effective portfolio allocation. Referring the dynamic strategy as benchmark, various smart beta schemes based on conditional covariance matrix were implemented on top of it. 

Part II: Statistical background 

Here are several essential statistical analyses leveraged on throughout the implementation process of the trading strategy. 

2.1 Kalman Filter:

This project applied Kalman Filter to dynamically update two important figures, one is the long-short ratio (beta), another is the ASR threshold. First, treat alpha and beta as state variables by transforming the problem into the following linear state space format. Then define absolute standardized residual (ASR) and calculate the conditional variance of forecast error. Additionally, the paper (2021) proposes alternative method to adjust ASR threshold through state variance.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/equation1.jpg" title="equation1" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/equation2.jpg" title="equation1" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

2.2 Stochastic discount factor (SDF):

One way to formulate dynamic ADF threshold is by solving a system of SDF equations. This project has further imposed an upper and lower bound of the threshold to assure it will fall into a reasonable range.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/equation3.jpg" title="equation1" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

2.3 Dynamic conditional correlation (DCC):

Note that the unconditional correlation between each pair is likely to be zero given the pairs are market neutral, and is most likely to capture the white noises if they turn out to be non-zero. However, under conditions like financial announcements or policy changes, the covariance could be non-zero and captured by DCC proposed by Engle (2002). The dynamic dependence of correlation is governed by two parameters, which are calculated through maximum-likelihood estimation.

2.4 Diversity Index:

Diversity index (DI) is a passive but dynamic portfolio. DI bears a relationship to its underlying index. If portfolio's diversity doesn’t decrease and large companies in the original portfolio don’t have a higher yield than small companies, then DI would have a higher long-term return after a certain period. Since I'm testing on market index and necessary conditions for DI are not violated, then it’s logical to apply this method. 

Part III: Strategy 

3.1 Data collection and cleaning 

TPX100 is chosen as the trading universe, which is liquid enough for our analysis. Five years of data is used for back-testing, starting from 8-Jan-2016 to 25-Dec-2020. Moreover, I used 52-weeks data for Kalman Filter initialization. Assume zero transaction cost, which does not violate the "all else equal” assumption in comparing the performance between the benchmark and proposed smart beta allocation.

3.2 Pair selection process 

First, collect all possible pairs under TPX100 grouped by GICS (Global Industry Classification Standard) level II. Next, apply Kalman Filter to calculate the cointegration coefficients for all possible pairwise combinations. I would only include a pair into the potential group if all of the following criteria are fulfilled:

A)Cointegration check: the ADF t-stat is smaller than the ADF threshold, which is dynamically updatedusing SDF discussed in part II.

B)Deviation check: ASR is larger than or equal to the ASR threshold, which is dynamically updated usingKalman Filter.

C)The prices of both stocks in the pair are available in previous 52 weeks.

Lastly, within the potential group, we calculate their PS, select the top 20 pairs (if available) as our trading group based on PS. PS is defined as:
This allows us to test cointegration (via ADF) and deviation (via ASR) using one variable, which provides more concrete information compared to traditional pairs trading method. 

3.3 Portfolio Allocation 

The benchmark allocation is equal weighted initially, and do not manually adjust weight in the future rebalancing. This project proposed three different allocation schemes to improve the allocation: Global minimum variance (GMV), Maximum diversification ratio (MDR) and Diversity Index (DI). We used DCC to calculate the conditional covariance matrix used in the proposed allocation schemes. 

3.4 Rebalancing 

Rebalance on a weekly basis by first re-calculating the ADF and ASR thresholds. I will only close a position if any of the previous conditions in the selection process is violated. I then add new pairs into our portfolio based on the selection scheme in 3.2 and keep the number of pairs within 20. Finally, reallocate the portfolio based on 3.3. 

Part IV: Empirical finding 

4.1 Dynamic Thresholds 

The strategy involves two dynamically changing thresholds, ADF and ASR threshold. It's observed that the threshold levels were low in volatile markets (for instance, in 2008 and 2020), indicating the ability to adjust according to overall market conditions.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/ADF.jpg" title="equation1" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
The graph below is an illustration of how ASR thresholds evolved for a pair. The thresholds would adjust upward when standard deviation of residual increases, suggesting more uncertainty in residual prediction. This allows the strategy to be more conservative when the pairs relationship becomes volatile.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/ASR.jpg" title="equation1" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
4.2 Back-test Results

Both DI and MDR schemes outperformed the benchmark and DI schemes achieved the highest return of 26%. As for drawdown, although the 3 smart beta schemes have reduced certain level of risk, they do not always improve performance relative to benchmark.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/NAV.jpg" title="equation1" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/drawdown.jpg" title="equation1" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
Both MDR and DI outperformed the Benchmark scenario, with higher annualized return and lower maximum drawdown. In terms of risk-return tradeoff, DI and MDR also exceeded the benchmark strategy with higher ratios.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/metric_proj2.jpg" title="equation1" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
Part V: Conclusion 

This project proposed and verified that by incorporating smart beta, including GMV, MDR and DI as alternative asset allocation strategies, the risk-adjusted performance can be further enhanced compared with original pairs trading strategy.

References:

Engle, R. (2002). Dynamic Conditional Correlation: A Simple Class of Multivariate Generalized Autoregressive Conditional Heteroskedasticity Models. Journal of Business & Economic Statistics, 20(3), 339–350. http://www.jstor.org/stable/1392121

Ferguson, R., & Fernholz, B. R. (1998). A New Kind of Index Fund that Beats its Index. The Journal of Performance Measurement, Winter 1998-1999, 35–48. https://papers.ssrn.com/sol3/papers.cfm? abstract_id=2335413

Law, K. F., Li, W. K., & Yu, P. L. H. (2018). A single-stage approach for Cointegration-based pairs trading. Finance Research Letters, 26, 177–184. https://doi.org/10.1016/j.frl.2017.12.011

Li, Y., & Law, K. K. F. (2021). Systematic risk in pairs trading and dynamic parameterization. Economics Letters, 202, 109842. https://doi.org/10.1016/j.econlet.2021.109842