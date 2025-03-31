Gasoline prices in Japan
================
Mitsuo Shiota
2021-11-17

- [Summary](#summary)
- [Get data](#get-data)
  - [Gasoline prices](#gasoline-prices)
  - [Imported crude oil price](#imported-crude-oil-price)
  - [Subsidy](#subsidy)
  - [Dubai crude oil price (monthly)](#dubai-crude-oil-price-monthly)
- [Plot retail, wholesale gas and crude oil
  prices](#plot-retail-wholesale-gas-and-crude-oil-prices)
- [Plot price differences](#plot-price-differences)
  - [Retailers’ margin](#retailers-margin)
  - [Wholesalers’ margin over imported crude oil price one month ahead
    plus tax minus
    subsidy](#wholesalers-margin-over-imported-crude-oil-price-one-month-ahead-plus-tax-minus-subsidy)
  - [Difference between CIF and FOB](#difference-between-cif-and-fob)
- [Dubai crude oil price (weekly)](#dubai-crude-oil-price-weekly)
- [Change of METI’s effects calculation from March 14,
  2022](#change-of-metis-effects-calculation-from-march-14-2022)
- [Wholesalers reported record profits for FY
  2021](#wholesalers-reported-record-profits-for-fy-2021)
- [Wholesalers’ margin over Dubai crude oil
  price](#wholesalers-margin-over-dubai-crude-oil-price)

Updated: 2025-03-31

## Summary

[Nikkei
reported](https://www.nikkei.com/article/DGXZQOUA169350W1A111C2000000/)
on November 17, 2021 that Ministry of Economy, Trade and Industry (METI)
is considering a subsidy to gasoline wholesalers to suppress gasoline
retail prices when they rise to higher than 170 yen per litre. At the
end of the article, Nikkei showed a skeptical view saying, “As the
number of retailers has decreased, the retailers may not reduce retail
prices even if wholesale prices fall.” So I study the relationship among
retail and wholesale prices of gasoline and imported crude oil prices in
Japan.

I find the current surge in retail prices of gasoline is mainly due to
the surge in imported crude oil prices, and partly due to the reduced
competition among retailers and wholesalers, who are getting more
margins.

To give a subsidy to wholesalers may incentivize them to raise their
margins even more. This subsidy idea is contrary to that of the Biden
Administration, which has begun to investigate oil companies.

## Get data

### Gasoline prices

Agency for National Resources Energy under METI publishes gasoline
prices in its [web
site](https://www.enecho.meti.go.jp/statistics/petroleum_and_lpgas/pl007/results.html#headline1).
Although the original retail price data include consumption tax since
April 1, 2004, I exclude consumption tax all over the period.

![](README_files/figure-gfm/retail_weekly-1.png)<!-- -->

### Imported crude oil price

I get imported crude oil price data from customs statistics via
[e-Stat](https://www.e-stat.go.jp/stat-search/files?page=1&layout=datalist&toukei=00350300&bunya_l=16&tstat=000001013141&cycle=1&tclass1=000001013192&tclass2=000001013194&tclass3val=0).

### Subsidy

[METI publishes](https://nenryo-gekihenkanwa.jp/pdf/result_rev8.pdf)
Dubai crude oil prices, counterfactual gasoline prices without
subsidies, actual gasoline prieces and effects of subsidies, which are
differences between counterfacual and actual, all in the units of yen /
litre every week.

![](README_files/figure-gfm/meti_vs_crude-1.png)<!-- -->

### Dubai crude oil price (monthly)

I get monthly Dubai crude oil prices in USD / barrel and exchange rates
in JPY / USD from [FRED by St. Louis FRB](https://fred.stlouisfed.org/),
and transform them into JPY / litre using 1 barrel = 158.987 litre.

## Plot retail, wholesale gas and crude oil prices

Yes, both retail and wholesale prices are rising, mainly due to rising
imported crude oil prices.

![](README_files/figure-gfm/plot1-1.png)<!-- -->

Correlations are 0.9386 if wholesales lead Japan customs by 1 month,
0.942 if wholesales is concurrent with Japan customs, and 0.9162 if
wholesales lag Japan customs by 1 month. The highest correlation is if
wholesales is concurrent with Japan customs. However, METI gives subsidy
assuming that wholesales lead Japan customs by 1 month. So I regard
imported crude oil price one month ahead, plus tax and minus subsidy as
wholesalers’ cost.

![](README_files/figure-gfm/wholesale_vs_crude_plot-1.png)<!-- -->

## Plot price differences

### Retailers’ margin

Differences between retail and wholesale prices have been increasing
since 2016. The average difference was 12 yen per litre from 2000 Jul to
2015 Dec, and is 21 in 2025 Feb. This may reflect the reduced
competition among retailers. You can see the number of retailers has
constantly decreased since around 1995 in the last page of [this
material
(Japanese)](https://www.enecho.meti.go.jp/category/resources_and_fuel/distribution/hinnkakuhou/data/2021_07_30_01.pdf)
from Agency for National Resources Energy.

![](README_files/figure-gfm/retailers_margin_plot-1.png)<!-- -->

| Month    | Margin | Usual margin | Excess margin |
|:---------|-------:|-------------:|--------------:|
| 2022 Feb |   17.5 |         17.2 |           0.4 |
| 2022 Mar |   18.7 |         17.2 |           1.6 |
| 2022 Apr |   20.0 |         17.2 |           2.8 |
| 2022 May |   21.1 |         17.2 |           3.9 |
| 2022 Jun |   17.7 |         17.2 |           0.5 |
| 2022 Jul |   22.3 |         17.2 |           5.1 |
| 2022 Aug |   20.9 |         17.2 |           3.7 |
| 2022 Sep |   20.1 |         17.2 |           2.9 |
| 2022 Oct |   19.6 |         17.2 |           2.4 |
| 2022 Nov |   19.1 |         17.2 |           1.9 |
| 2022 Dec |   18.7 |         17.2 |           1.5 |
| 2023 Jan |   19.0 |         17.2 |           1.8 |
| 2023 Feb |   19.5 |         17.2 |           2.4 |
| 2023 Mar |   18.1 |         17.2 |           1.0 |
| 2023 Apr |   18.1 |         17.2 |           0.9 |
| 2023 May |   19.1 |         17.2 |           1.9 |
| 2023 Jun |   16.7 |         17.2 |          -0.4 |
| 2023 Jul |   16.9 |         17.2 |          -0.3 |
| 2023 Aug |   17.4 |         17.2 |           0.3 |
| 2023 Sep |   23.0 |         17.2 |           5.8 |
| 2023 Oct |   25.3 |         17.2 |           8.1 |
| 2023 Nov |   21.1 |         17.2 |           3.9 |
| 2023 Dec |   19.4 |         17.2 |           2.2 |
| 2024 Jan |   19.7 |         17.2 |           2.5 |
| 2024 Feb |   19.5 |         17.2 |           2.4 |
| 2024 Mar |   19.2 |         17.2 |           2.0 |
| 2024 Apr |   18.6 |         17.2 |           1.5 |
| 2024 May |   18.8 |         17.2 |           1.6 |
| 2024 Jun |   17.9 |         17.2 |           0.7 |
| 2024 Jul |   19.6 |         17.2 |           2.5 |
| 2024 Aug |   20.8 |         17.2 |           3.6 |
| 2024 Sep |   19.5 |         17.2 |           2.4 |
| 2024 Oct |   19.3 |         17.2 |           2.1 |
| 2024 Nov |   19.2 |         17.2 |           2.1 |
| 2024 Dec |   19.6 |         17.2 |           2.4 |
| 2025 Jan |   21.2 |         17.2 |           4.1 |
| 2025 Feb |   21.2 |         17.2 |           4.0 |

Retailers’ excess margin (yen / liter)

The average retailers’ margin in 2021 was 17.2. If I subtract it from
retailers’ margin, I get excess margin above.

### Wholesalers’ margin over imported crude oil price one month ahead plus tax minus subsidy

Differences between wholesale price and imported crude oil price one
month ahead plus gasoline tax minus subsidy have also been increasing.
This may reflect the reduced competition among wholesalers, who have got
monopolistic power by consolidation.

![](README_files/figure-gfm/wholesalers_margin_plot-1.png)<!-- -->

| Month    | Margin | Usual margin | Excess margin |
|:---------|-------:|-------------:|--------------:|
| 2022 Feb |   22.6 |         19.8 |           2.8 |
| 2022 Mar |   19.2 |         19.8 |          -0.6 |
| 2022 Apr |   19.1 |         19.8 |          -0.7 |
| 2022 May |   18.3 |         19.8 |          -1.5 |
| 2022 Jun |   25.1 |         19.8 |           5.2 |
| 2022 Jul |   22.8 |         19.8 |           3.0 |
| 2022 Aug |   16.7 |         19.8 |          -3.1 |
| 2022 Sep |   20.1 |         19.8 |           0.3 |
| 2022 Oct |   24.0 |         19.8 |           4.1 |
| 2022 Nov |   29.8 |         19.8 |          10.0 |
| 2022 Dec |   23.8 |         19.8 |           4.0 |
| 2023 Jan |   23.7 |         19.8 |           3.9 |
| 2023 Feb |   23.9 |         19.8 |           4.1 |
| 2023 Mar |   26.4 |         19.8 |           6.5 |
| 2023 Apr |   21.4 |         19.8 |           1.6 |
| 2023 May |   21.3 |         19.8 |           1.4 |
| 2023 Jun |   22.0 |         19.8 |           2.1 |
| 2023 Jul |   23.9 |         19.8 |           4.1 |
| 2023 Aug |   26.0 |         19.8 |           6.2 |
| 2023 Sep |   24.1 |         19.8 |           4.2 |
| 2023 Oct |   26.6 |         19.8 |           6.8 |
| 2023 Nov |   27.4 |         19.8 |           7.6 |
| 2023 Dec |   25.6 |         19.8 |           5.7 |
| 2024 Jan |   23.5 |         19.8 |           3.6 |
| 2024 Feb |   27.7 |         19.8 |           7.8 |
| 2024 Mar |   25.0 |         19.8 |           5.2 |
| 2024 Apr |   26.8 |         19.8 |           6.9 |
| 2024 May |   26.9 |         19.8 |           7.1 |
| 2024 Jun |   23.0 |         19.8 |           3.2 |
| 2024 Jul |   33.8 |         19.8 |          13.9 |
| 2024 Aug |   30.3 |         19.8 |          10.5 |
| 2024 Sep |   25.2 |         19.8 |           5.3 |
| 2024 Oct |   25.6 |         19.8 |           5.8 |
| 2024 Nov |   28.1 |         19.8 |           8.2 |
| 2024 Dec |   26.5 |         19.8 |           6.6 |
| 2025 Jan |   31.2 |         19.8 |          11.4 |

Wholesalers’ excess margin (yen / liter)

The average wholesalers’ margin in 2021 was 19.8. If I subtract it from
retailers’ margin, I get excess margin above.

### Difference between CIF and FOB

Difference between CIF and FOB has widened since 2022, probably because
Russian invasion of Ukraine and depreciated yen have raised insurance
and transport fees.

![](README_files/figure-gfm/cif_fob_plot-1.png)<!-- -->

## Dubai crude oil price (weekly)

I get daily data from
[oilprice.com](https://oilprice.com/jp/%E5%8E%9F%E6%B2%B9%E4%BE%A1%E6%A0%BC%E3%83%81%E3%83%A3%E3%83%BC%E3%83%88).

Weekly prices are not so different between oilprice.com and METI.

![](README_files/figure-gfm/dubai_chart-1.png)<!-- -->

So I create weekly prices data by combining oilprice.com in 2021 and
METI in 2022.

METI calculates subsidy effects on retail prices by assuming that retail
prices will reflect subsidies 2 weeks later. I have no reason to
disagree.

METI also assumes that there is 2 weeks lag between Dubai crude oil
prices and retail prices (0 week from Dubai to wholesalers and 2 weeks
from wholesalers to retailers). I disagree. In the physical terms, it is
impossible to carry oil from Dubai to retailers in Japan in 2 weeks. In
the accounting terms, all major wholesalers evaluate stocks by average
method, not by LIFO (Last In First Out). In the past observations from
the monthly data, wholesale prices usually lag Dubai crude oil prices by
1 month.

If we assume 3 weeks lag instead, subsidy effects are smaller. When
effects are smaller than subsidies, subsidies are not fully reflected in
retail prices, and wholesalers and/or retailers are getting benefits
from subsidies.

![](README_files/figure-gfm/actual_counterfactual-1.png)<!-- -->

![](README_files/figure-gfm/subsidy_effect-1.png)<!-- -->

Above calculation assumes there is a constant margin between Dubai crude
oil prices and retail prices. Lag 0 to 5 assumes margins 107.3, 107.6,
110.8, 112.4, 118.5, 118.5 respectively.

Lag 2 assumes 110.8. Let us assume this margin of 110.8 over all lags
between Dubai crude oil prices and retail prices.

![](README_files/figure-gfm/actual_counterfactual2-1.png)<!-- -->

![](README_files/figure-gfm/subsidy_effect2-1.png)<!-- -->

As I assume fixed margin based on lag 2 assumption, lag 2 does not
change from the previous plots. In lags 3 to 5, there are less effects
of subsidies than the previous plots.

From this analysis, it is clear that the assumption on lag and normal
margin between Dubai crude oil prices and retail gasoline prices is very
important to calculate the effects by subsidies. METI assumes lag of 2
weeks from Dubai to retailers (0 week from Dubai to wholesalers and 2
weeks from wholesalers to retailers), and normal margin 110.8 yen per
litre between Dubai and retail prices. However, the monthly data
analysis in the previous chapter suggests that there usually is one
month lag from Dubai to retailers, and that 110.8 yen per litre gap
between Dubai and retail prices is historically large. I hope I will
find out whether METI’s assumption is reasonable or not, while I update
this page.

## Change of METI’s effects calculation from March 14, 2022

METI changed calculation method from March 14, 2022. The new method is:

1.  Reset the last week counterfactual retail price without subsidy to
    the last week actual retail price plus subsidy.

2.  Add Dubai crude oil price change from the last week to this week to
    it, and you will get this week counterfactual retail price without
    subsidy.

I once thought this is a minor change, as METI basically continues to
assume that retail prices will reflect subsidies 2 weeks later, and that
there is 2 weeks lag between Dubai crude oil prices and retail prices.
No, this is not a minor change. The new method makes effects closer to
subsidies inherently, and allows wholesalers and/or retailers to take as
much money as they like.

I tweet about this [here
(English)](https://twitter.com/mitsuoxv/status/1536998279916924929?s=20&t=arc-0ZS59Qpugt1CcXDGVA)
and [here
(Japanese)](https://twitter.com/mitsuoxv/status/1537004102697136128?s=20&t=arc-0ZS59Qpugt1CcXDGVA).

    ## Warning: Removed 3 rows containing missing values or values outside the scale range
    ## (`geom_col()`).

![](README_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

## Wholesalers reported record profits for FY 2021

I tweeted about this
[here](https://twitter.com/mitsuoxv/status/1519833594046283781).

## Wholesalers’ margin over Dubai crude oil price

Although I disagree to the METI assumptions, first, I assume them to
check whether wholesalers are acting as METI expected. So I assume that
Dubai crude oil price and subsidy will be reflected to wholesale gas
price 10 days (2 weeks) later, and that the usual margin of wholesalers
is the same as that of 2022 Jan, which is higher than average but within
the historical range. Then, I calculate how much the actual wholesale
gas price exceeds the expected price if subsidy is fully reflected. From
2022 Jan to 2022 Apr, the excesses are small.

| Month    | Actual | Dubai crude | Subsidy | Gas tax | Usual margin | Expected | Excess |
|:---------|-------:|------------:|--------:|--------:|-------------:|---------:|-------:|
| 2022 Jan |  134.4 |        56.9 |     0.0 |    53.8 |         23.7 |    134.4 |    0.0 |
| 2022 Feb |  138.7 |        65.2 |     4.7 |    53.8 |         23.7 |    138.0 |    0.7 |
| 2022 Mar |  140.0 |        78.5 |    16.6 |    53.8 |         23.7 |    139.4 |    0.6 |
| 2022 Apr |  137.8 |        83.1 |    22.8 |    53.8 |         23.7 |    137.8 |    0.0 |
| 2022 May |  133.1 |        86.0 |    34.9 |    53.8 |         23.7 |    128.6 |    4.5 |
| 2022 Jun |  139.1 |        94.9 |    39.4 |    53.8 |         23.7 |    133.0 |    6.1 |
| 2022 Jul |  134.1 |        90.4 |    38.2 |    53.8 |         23.7 |    129.8 |    4.3 |
| 2022 Aug |  133.2 |        84.6 |    34.9 |    53.8 |         23.7 |    127.3 |    5.9 |
| 2022 Sep |  134.2 |        84.6 |    36.5 |    53.8 |         23.7 |    125.7 |    8.5 |
| 2022 Oct |  134.1 |        83.3 |    36.1 |    53.8 |         23.7 |    124.7 |    9.4 |
| 2022 Nov |  133.4 |        82.8 |    32.6 |    53.8 |         23.7 |    127.6 |    5.8 |
| 2022 Dec |  134.1 |        67.9 |    16.9 |    53.8 |         23.7 |    128.5 |    5.6 |
| 2023 Jan |  133.9 |        66.3 |    15.7 |    53.8 |         23.7 |    128.1 |    5.8 |
| 2023 Feb |  132.7 |        68.5 |    17.5 |    53.8 |         23.7 |    128.6 |    4.1 |
| 2023 Mar |  134.2 |        68.8 |    15.4 |    53.8 |         23.7 |    130.9 |    3.3 |
| 2023 Apr |  134.8 |        68.0 |    14.1 |    53.8 |         23.7 |    131.4 |    3.4 |
| 2023 May |  133.7 |        67.0 |    13.3 |    53.8 |         23.7 |    131.2 |    2.5 |
| 2023 Jun |  137.6 |        66.1 |    10.3 |    53.8 |         23.7 |    133.3 |    4.3 |
| 2023 Jul |  141.5 |        69.3 |     9.8 |    53.8 |         23.7 |    137.0 |    4.5 |
| 2023 Aug |  148.8 |        77.1 |    10.8 |    53.8 |         23.7 |    143.8 |    5.0 |
| 2023 Sep |  143.8 |        83.1 |    20.9 |    53.8 |         23.7 |    139.7 |    4.1 |
| 2023 Oct |  134.3 |        86.3 |    34.9 |    53.8 |         23.7 |    128.9 |    5.4 |
| 2023 Nov |  136.8 |        82.1 |    27.9 |    53.8 |         23.7 |    131.7 |    5.1 |
| 2023 Dec |  139.7 |        74.0 |    17.4 |    53.8 |         23.7 |    134.2 |    5.5 |
| 2024 Jan |  139.6 |        70.9 |    15.7 |    53.8 |         23.7 |    132.8 |    6.8 |
| 2024 Feb |  139.1 |        75.2 |    20.5 |    53.8 |         23.7 |    132.3 |    6.8 |
| 2024 Mar |  139.3 |        77.5 |    21.4 |    53.8 |         23.7 |    133.6 |    5.7 |
| 2024 Apr |  140.3 |        84.4 |    27.2 |    53.8 |         23.7 |    134.7 |    5.6 |
| 2024 May |  140.1 |        84.5 |    27.3 |    53.8 |         23.7 |    134.7 |    5.4 |
| 2024 Jun |  141.0 |        81.5 |    24.2 |    53.8 |         23.7 |    134.7 |    6.3 |
| 2024 Jul |  140.0 |        85.5 |    30.3 |    53.8 |         23.7 |    132.8 |    7.2 |
| 2024 Aug |  137.9 |        75.0 |    21.4 |    53.8 |         23.7 |    131.1 |    6.8 |
| 2024 Sep |  139.2 |        68.2 |    13.4 |    53.8 |         23.7 |    132.3 |    6.9 |
| 2024 Oct |  139.7 |        69.8 |    14.9 |    53.8 |         23.7 |    132.4 |    7.3 |
| 2024 Nov |  139.6 |        70.6 |    15.6 |    53.8 |         23.7 |    132.5 |    7.1 |
| 2024 Dec |  141.2 |        70.3 |    14.8 |    53.8 |         23.7 |    133.0 |    8.2 |
| 2025 Jan |  145.0 |        76.5 |    18.2 |    53.8 |         23.7 |    135.8 |    9.2 |
| 2025 Feb |  146.5 |        77.2 |    16.2 |    53.8 |         23.7 |    138.5 |    8.0 |

Wholesale gas price (yen / liter): 2 weeks lag of subsidy, and 2 weeks
lag of Dubai

Next, I change the assumption on lag of Dubai price from 10 days (2
weeks) based on LIFO to one month (5 weeks) based on the average method
of inventory evaluation actually adopted by wholesalers. In Feb and Mar,
when Dubai price was rising, excesses were large enough to show most
subsidy was taken by wholesalers. In Apr, when Dubai in plateau, excess
was small.

| Month    | Actual | Dubai crude | Subsidy | Gas tax | Usual margin | Expected | Excess |
|:---------|-------:|------------:|--------:|--------:|-------------:|---------:|-------:|
| 2022 Feb |  138.7 |        60.7 |     4.7 |    53.8 |         23.7 |    133.5 |    5.2 |
| 2022 Mar |  140.0 |        67.0 |    16.6 |    53.8 |         23.7 |    127.9 |   12.1 |
| 2022 Apr |  137.8 |        83.0 |    22.8 |    53.8 |         23.7 |    137.8 |    0.0 |
| 2022 May |  133.1 |        83.1 |    34.9 |    53.8 |         23.7 |    125.7 |    7.4 |
| 2022 Jun |  139.1 |        87.1 |    39.4 |    53.8 |         23.7 |    125.2 |   13.9 |
| 2022 Jul |  134.1 |        95.7 |    38.2 |    53.8 |         23.7 |    135.0 |   -0.9 |
| 2022 Aug |  133.2 |        89.9 |    34.9 |    53.8 |         23.7 |    132.6 |    0.6 |
| 2022 Sep |  134.2 |        82.2 |    36.5 |    53.8 |         23.7 |    123.3 |   10.9 |
| 2022 Oct |  134.1 |        82.9 |    36.1 |    53.8 |         23.7 |    124.3 |    9.8 |
| 2022 Nov |  133.4 |        85.0 |    32.6 |    53.8 |         23.7 |    129.9 |    3.5 |
| 2022 Dec |  134.1 |        79.1 |    16.9 |    53.8 |         23.7 |    139.7 |   -5.6 |
| 2023 Jan |  133.9 |        66.6 |    15.7 |    53.8 |         23.7 |    128.4 |    5.5 |
| 2023 Feb |  132.7 |        67.0 |    17.5 |    53.8 |         23.7 |    127.1 |    5.6 |
| 2023 Mar |  134.2 |        68.7 |    15.4 |    53.8 |         23.7 |    130.8 |    3.4 |
| 2023 Apr |  134.8 |        67.0 |    14.1 |    53.8 |         23.7 |    130.4 |    4.4 |
| 2023 May |  133.7 |        69.5 |    13.3 |    53.8 |         23.7 |    133.7 |    0.0 |
| 2023 Jun |  137.6 |        66.1 |    10.3 |    53.8 |         23.7 |    133.3 |    4.3 |
| 2023 Jul |  141.5 |        66.5 |     9.8 |    53.8 |         23.7 |    134.3 |    7.2 |
| 2023 Aug |  148.8 |        70.6 |    10.8 |    53.8 |         23.7 |    137.3 |   11.5 |
| 2023 Sep |  143.8 |        79.2 |    20.9 |    53.8 |         23.7 |    135.7 |    8.1 |
| 2023 Oct |  134.3 |        85.9 |    34.9 |    53.8 |         23.7 |    128.5 |    5.8 |
| 2023 Nov |  136.8 |        85.0 |    27.9 |    53.8 |         23.7 |    134.6 |    2.2 |
| 2023 Dec |  139.7 |        80.4 |    17.4 |    53.8 |         23.7 |    140.5 |   -0.8 |
| 2024 Jan |  139.6 |        71.5 |    15.7 |    53.8 |         23.7 |    133.4 |    6.2 |
| 2024 Feb |  139.1 |        72.5 |    20.5 |    53.8 |         23.7 |    129.5 |    9.6 |
| 2024 Mar |  139.3 |        75.6 |    21.4 |    53.8 |         23.7 |    131.7 |    7.6 |
| 2024 Apr |  140.3 |        78.9 |    27.2 |    53.8 |         23.7 |    129.2 |   11.1 |
| 2024 May |  140.1 |        86.8 |    27.3 |    53.8 |         23.7 |    137.0 |    3.1 |
| 2024 Jun |  141.0 |        83.4 |    24.2 |    53.8 |         23.7 |    136.7 |    4.3 |
| 2024 Jul |  140.0 |        82.2 |    30.3 |    53.8 |         23.7 |    129.4 |   10.6 |
| 2024 Aug |  137.9 |        85.0 |    21.4 |    53.8 |         23.7 |    141.1 |   -3.2 |
| 2024 Sep |  139.2 |        72.3 |    13.4 |    53.8 |         23.7 |    136.5 |    2.7 |
| 2024 Oct |  139.7 |        66.4 |    14.9 |    53.8 |         23.7 |    129.0 |   10.7 |
| 2024 Nov |  139.6 |        71.0 |    15.6 |    53.8 |         23.7 |    132.8 |    6.8 |
| 2024 Dec |  141.2 |        70.4 |    14.8 |    53.8 |         23.7 |    133.1 |    8.1 |
| 2025 Jan |  145.0 |        71.0 |    18.2 |    53.8 |         23.7 |    130.3 |   14.7 |
| 2025 Feb |  146.5 |        78.3 |    16.2 |    53.8 |         23.7 |    139.6 |    6.9 |

Wholesale gas price (yen / liter): 2 weeks lag of subsidy, and 5 weeks
lag of Dubai

EOL
