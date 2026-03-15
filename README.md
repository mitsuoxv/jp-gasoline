# Gasoline retailers’ and wholesalers’ margins in Japan
Mitsuo Shiota
2026-03-03

- [Weekly update](#weekly-update)
  - [Gasoline retail price (weekly)](#gasoline-retail-price-weekly)
  - [Dubai crude oil price (weekly)](#dubai-crude-oil-price-weekly)
  - [Subsidy](#subsidy)
  - [Margin](#margin)
- [Monthly update](#monthly-update)
  - [Retail prices (monthly)](#retail-prices-monthly)
  - [Wholesale prices](#wholesale-prices)
  - [Dubai crude oil price (monthly)](#dubai-crude-oil-price-monthly)
  - [Subsidy](#subsidy-1)
  - [Margin (monthly)](#margin-monthly)

``` r
library(tidyverse)
library(readxl)
library(tsibble)
library(tidyquant)

theme_set(theme_light())
```

Updated: 2026-03-15

## Weekly update

### Gasoline retail price (weekly)

Agency for National Resources Energy under METI publishes Monday
gasoline prices in its [web
site](https://www.enecho.meti.go.jp/statistics/petroleum_and_lpgas/pl007/results.html#headline1)
every Wednesday, usually. Sometimes, due to holidays, target dates and
publishing dates are delayed.

``` r
retail <- read_excel("data/260311s5.xlsx", 
                     sheet = "レギュラー",
                     col_types = c("text", "date", rep("numeric", 59))) |> 
  select(2:3) |> 
  set_names(c("date", "retail_price")) |> 
  mutate(
    date = as.Date(date)
    ) |> 
  filter(!is.na(date))
```

``` r
retail |> 
  filter(date >= as.Date("2020-01-01")) |> 
  ggplot(aes(date, retail_price)) +
  geom_line() +
  scale_x_date(date_breaks = "12 week", date_labels = "%y %b %d") +
  labs(x = NULL, y = "Yen per litre",
       title = "Gasoline retail price (weekly)",
       caption = "Note: including consumption tax\nSource: Agency for National Resources Energy") +
  theme(axis.text.x = element_text(angle = 90, vjust = 0.5),
        panel.grid.minor.x = element_blank())
```

![Gasoline retail price
(weekly)](README_files/figure-commonmark/retail_price_plot-1.png)

### Dubai crude oil price (weekly)

Agency for National Resources Energy had published Dubai crude oil
prices (weekly average from Tuesday to Monday) from 2022 January up to
2025 May.

Since it stopped publishing Dubai crude oil prices, I give up
calculating weekly average from Tuesday to Monday. Instead, I get Monday
only data from
https://www.investing.com/commodities/dubai-crude-oil-platts-futures-historical-data
and https://www.investing.com/currencies/usd-jpy-historical-data, and
regard them as weekly average from Tuesday to Monday.

I fill missing data in price.

``` r
meti_dubai_weekly <- tribble(
  ~start_date, ~price,
  "2022-01-03", 57.8,
  "2022-01-10", 59.4,
  "2022-01-17", 62.6,
  "2022-01-24", 62.9,
  "2022-01-31", 64.4,
  "2022-02-07", 66.2,
  "2022-02-14", 67.4, # Up to here, Monday
  "2022-02-22", 69.9, # From here, Tuesday
  "2022-03-01", 81.6,
  "2022-03-08", 85.8,
  "2022-03-15", 76.8,
  "2022-03-22", 88,
  "2022-03-29", 81.6,
  "2022-04-05", 79.2,
  "2022-04-12", 83.6,
  "2022-04-19", 85.6,
  "2022-04-26", 85.4,
  "2022-05-03", NA,
  "2022-05-10", 85.5,
  "2022-05-17", 88.2,
  "2022-05-24", 89.2,
  "2022-05-31", 93.3,
  "2022-06-07", 99.1,
  "2022-06-14", 97.9,
  "2022-06-21", 92.3,
  "2022-06-28", 94.9,
  "2022-07-05", 88.3,
  "2022-07-12", 86.3,
  "2022-07-19", 90.3,
  "2022-07-26", 89.8,
  "2022-08-02", 81.4,
  "2022-08-09", 82.0,
  "2022-08-16", 79.6,
  "2022-08-23", 86.0,
  "2022-08-30", 85.3,
  "2022-09-06", 83.0,
  "2022-09-13", 84.2,
  "2022-09-20", 82.4,
  "2022-09-27", 79.4,
  "2022-10-04", 83.5,
  "2022-10-11", 86.3,
  "2022-10-18", 85.1,
  "2022-10-25", 85.3,
  "2022-11-01", 86.5,
  "2022-11-08", 82.7,
  "2022-11-15", 76.5,
  "2022-11-22", 70.7,
  "2022-11-29", 69.8,
  "2022-12-06", 64.6,
  "2022-12-13", 66.4,
  "2022-12-20", 65.7,
"2022-12-27", 66.3,
"2023-01-03", NA,
"2023-01-10", 65.3,
"2023-01-17", 67.9,
"2023-01-24", 68.7,
"2023-01-31", 66.2,
"2023-02-07", 68.6,
"2023-02-14", 70.6,
"2023-02-21", 69.5,
"2023-02-28", 71.2,
"2023-03-07", 70.8,
"2023-03-14", 63.7,
"2023-03-21", 62.3,
"2023-03-28", 66.0,
"2023-04-04", 71.0,
"2023-04-11", 72.6,
"2023-04-18", 70.3,
"2023-04-25", 67.8,
"2023-05-02", NA,
"2023-05-09", 64.4,
"2023-05-16", 64.9,
"2023-05-23", 67.3,
"2023-05-30", 65.3,
"2023-06-06", 66.1,
"2023-06-13", 65.7,
"2023-06-20", 67.6,
"2023-06-27", 67.9,
"2023-07-04", 69.9,
"2023-07-11", 70.6,
"2023-07-18", 70.4,
"2023-07-25", 71.6,
"2023-08-01", 77.7,
"2023-08-08", 79.5,
"2023-08-15", 79.7,
"2023-08-22", 79.8,
"2023-08-29", 80.9,
"2023-09-05", 84.7,
"2023-09-12", 87.0,
"2023-09-19", 88.0,
"2023-09-26", 89.0,
"2023-10-03", 83.8,
"2023-10-10", 84.2,
"2023-10-17", 86.5,
"2023-10-24", 85.5,
"2023-10-31", 83.3,
"2023-11-07", 80.0,
"2023-11-14", 79.5,
"2023-11-21", 78.7,
"2023-11-28", 76.8,
"2023-12-05", 71.3,
"2023-12-12", 69.3,
"2023-12-19", 69.9,
"2023-12-26", 70.4,
"2024-01-02", NA,
"2024-01-09", 71.2,
"2024-01-16", 72.7,
"2024-01-23", 75.7,
"2024-01-30", 74.3,
"2024-02-06", 74.3,
"2024-02-13", 76.7,
"2024-02-20", 77.1,
"2024-02-27", 77.5,
"2024-03-05", 77.4,
"2024-03-12", 78.0,
"2024-03-19", 80.5,
"2024-03-26", 81.3,
"2024-04-02", 85.9,
"2024-04-09", 87.1,
"2024-04-16", 87.1,
"2024-04-23", 87.1,
"2024-04-30", 85.4,
"2024-05-07", 82.5,
"2024-05-14", 83.0,
"2024-05-21", 82.9,
"2024-05-28", 83.4,
"2024-06-04", 78.3,
"2024-06-11", 81.3,
"2024-06-18", 83.1,
"2024-06-25", 84.9,
"2024-07-02", 88.5,
"2024-07-09", 86.9,
"2024-07-16", 84.2,
"2024-07-23", 80.4,
"2024-07-30", 74.9,
"2024-08-06", 70.8,
"2024-08-13", 74.0,
"2024-08-20", 70.9,
"2024-08-27", 71.1,
"2024-09-03", 67.6,
"2024-09-10", 64.7,
"2024-09-17", 66.7,
"2024-09-24", 66.7,
"2024-10-01", 69.2,
"2024-10-08", 73.1,
"2024-10-15", 70.2,
"2024-10-22", 71.4,
"2024-10-29", 69.9,
"2024-11-05", 71.5,
"2024-11-12", 69.6,
"2024-11-19", 71.3,
"2024-11-26", 69.6,
"2024-12-03", 68.4,
"2024-12-10", 70.3,
"2024-12-17", 71.9,
"2024-12-24", 73.6,
"2024-12-31", NA,
"2025-01-07", 77.0,
"2025-01-14", 81.9,
"2025-01-21", 80.8,
"2025-01-28", 78.1,
"2025-02-04", 74.9,
"2025-02-11", 74.9,
"2025-02-18", 75.0,
"2025-02-25", 72.8,
"2025-03-04", 66.8,
"2025-03-11", 66.8,
"2025-03-18", 69.0,
"2025-03-25", 71.1,
"2025-04-01", 68.4,
"2025-04-08", 60.2,
"2025-04-15", 61.0,
"2025-04-22", 61.7,
"2025-04-29", NA,
"2025-05-06", 57.6,
"2025-05-13", 60.0,
"2025-05-20", 58.7,
"2025-05-27", 58.2,
"2025-06-03", 59.9, # 06-09(Mon) 65.88 * 144.61 / 159
"2025-06-10", 63.1, # 06-16(Mon) 69.34 * 144.74 / 159
"2025-06-17", 68.7, # +5.6 = 175 + 13.4 - 182.8
"2025-06-24", 62.8, # 06-30(Mon) 69.27 * 144.04 / 159
"2025-07-01", 64.6, # 07-07(Mon) 70.33 * 146.05 / 159
"2025-07-08", 67.7, # +3.1 = 175 + 11.3 - 183.2; 07-14(Mon) 65.1 = 70.06 * 147.71 / 159
"2025-07-15", 68.0, # +0.3 = 175 + 10.2 - 184.9; 07-21(Mon) 65.2 = 70.32 * 147.39 / 159
"2025-07-22", 66.0, # 07-28(Mon) 70.59 * 148.55 / 159
"2025-07-29", 69.0, # +3.0 = 175 + 12.2 - 184.2; 08-04(Mon) 65.3 = 70.64 * 147.09 / 159
"2025-08-05", 64.3, # 08-11(Mon) 69.03 * 148.14 / 159
"2025-08-12", 64.1, # 08-18(Mon) 68.94 * 147.88 / 159
"2025-08-19", 65.4, # +1.3 = 175 + 10.5 - 184.2; 08-25(Mon) 64.7 = 69.64 * 147.79 / 159
"2025-08-26", 64.2, # 09-01(Mon) 69.39 * 147.18 / 159
"2025-09-02", 64.0, # 09-08(Mon) 69.03 * 147.52 / 159
"2025-09-09", 65.6, # 09-15(Mon) 70.74 * 147.41 / 159
"2025-09-16", 64.5, # 09-22(Mon) 69.74 * 147.72 / 159
"2025-09-23", 65.4, # 09-29(Mon) 69.95 * 148.60 / 159
"2025-09-30", 62.0, # 10-06(Mon) 65.59 * 150.36 / 159
"2025-10-07", 61.5, # 10-13(Mon) 64.18 * 152.28 / 159
"2025-10-14", 59.9, # 10-20(Mon) 63.15 * 150.75 / 159
"2025-10-21", 62.8, # 10-27(Mon) 65.36 * 152.88 / 159
"2025-10-28", 64.3, # 11-03(Mon) 66.30 * 154.22 / 159
"2025-11-04", 63.4, # 11-10(Mon) 65.37 * 154.15 / 159
"2025-11-11", 63.5, # 11-17(Mon) 65.00 * 155.26 / 159
"2025-11-18", 63.7, # 11-24(Mon) 64.50 * 156.91 / 159
"2025-11-25", 62.4, # 12-01(Mon) 63.80 * 155.45 / 159
"2025-12-02", 62.1, # 12-08(Mon) 63.35 * 155.93 / 159
"2025-12-09", 60.2, # 12-15(Mon) 61.69 * 155.22 / 159
"2025-12-16", 61.4, # 12-22(Mon) 62.12 * 157.07 / 159
"2025-12-23", 60.9, # 12-29(Mon) 62.02 * 156.06 / 159
"2025-12-30", 59.3, # 01-05(Mon) 60.31 * 156.39 / 159
"2026-01-06", 60.8, # 01-12(Mon) 61.17 * 158.16 / 159
"2026-01-13", 61.3, # 01-19(Mon) 61.66 * 158.12 / 159
"2026-01-20", 59.7, # 01-26(Mon) 61.58 * 154.17 / 159
"2026-01-27", 63.6, # 02-02(Mon) 65.02 * 155.62 / 159
"2026-02-03", 66.5, # 02-09(Mon) 67.79 * 155.89 / 159
"2026-02-10", 64.7, # 02-16(Mon) 67.05 * 153.51 / 159
"2026-02-17", 66.5, # 02-23(Mon) 68.38 * 154.64 / 159
"2026-02-24", 75.7, # 03-02(Mon) 76.53 * 157.37 / 159
"2026-03-03", 106.7 # 03-09(Mon) 107.55 * 157.67 / 159
) |> 
  fill(price, .direction = "down")
```

### Subsidy

Every week, on Wednesday, Agency for National Resources Energy announced
next Thursday-Wednesday subsidy based on actual Tuesday-Monday average
Dubai crude oil price, and Monday gasoline retail price.

In the code below, start_date in meti_subsidy is the same start_date in
meti_dubai_weekly. It means ‘Tuesday’, not ‘Thursday’.

I fill missing data in subsidy.

``` r
meti_subsidy <- tribble(
  ~start_date, ~subsidy,
  "2022-01-17", 3.4,
  "2022-01-24", 3.7,
  "2022-01-31", 5,
  "2022-02-07", 5,
  "2022-02-14", 5, # Up to here, Monday
  "2022-02-22", 5, # From here, Tuesday
  "2022-03-01", 17.7,
  "2022-03-08", 25,
  "2022-03-15", 18.6,
  "2022-03-22", 25,
  "2022-03-29", 20.7,
  "2022-04-05", 20.3,
  "2022-04-12", 25,
  "2022-04-19", 31.8,
  "2022-04-26", 34.7,
  "2022-05-03", NA,
  "2022-05-10", 36.1,
  "2022-05-17", 37.3,
  "2022-05-24", 36.7,
  "2022-05-31", 38.8,
  "2022-06-07", 41.4,
  "2022-06-14", 40.5,
  "2022-06-21", 38.4,
  "2022-06-28", 40.8,
  "2022-07-05", 36.9,
  "2022-07-12", 36.6,
  "2022-07-19", 39.0,
  "2022-07-26", 37.7,
  "2022-08-02", 31.4,
  "2022-08-09", 33.8,
  "2022-08-16", 32.4,
  "2022-08-23", 37.1,
  "2022-08-30", 36.5,
  "2022-09-06", 35.6,
  "2022-09-13", 36.7,
  "2022-09-20", 35.7,
  "2022-09-27", 33.8,
  "2022-10-04", 36.8,
  "2022-10-11", 37.8,
  "2022-10-18", 36.4,
  "2022-10-25", 36.3,
  "2022-11-01", 36.3,
  "2022-11-08", 32.3,
  "2022-11-15", 25.7,
  "2022-11-22", 19.5,
  "2022-11-29", 18.7,
  "2022-12-06", 13.7,
  "2022-12-13", 15.6,
  "2022-12-20", 14.8,
  "2022-12-27", 15.6,
  "2023-01-03", NA,
  "2023-01-10", 14.8,
  "2023-01-17", 17.5,
  "2023-01-24", 18.4,
  "2023-01-31", 15.5,
  "2023-02-07", 17.3,
  "2023-02-14", 18.7,
  "2023-02-21", 17.0,
  "2023-02-28", 18.1,
  "2023-03-07", 17.1,
  "2023-03-14", 9.5,
  "2023-03-21", 8.1,
  "2023-03-28", 11.9,
  "2023-04-04", 17.2,
  "2023-04-11", 19.0,
  "2023-04-18", 16.8,
  "2023-04-25", 14.1,
  "2023-05-02", NA,
  "2023-05-09", 10.5,
  "2023-05-16", 11.1,
  "2023-05-23", 12.5,
  "2023-05-30", 10.0,
  "2023-06-06", 9.6,
  "2023-06-13", 9.0,
  "2023-06-20", 9.7,
  "2023-06-27", 10.1,
  "2023-07-04", 10.4,
  "2023-07-11", 10.2,
  "2023-07-18", 8.4,
  "2023-07-25", 9.1,
  "2023-08-01", 12.0,
  "2023-08-08", 12.1,
  "2023-08-15", 10.0,
  "2023-08-22", 9.7,
  "2023-08-29", 17.4,
  "2023-09-05", 26.1,
  "2023-09-12", 30.5,
  "2023-09-19", 32.1,
  "2023-09-26", 37.6,
  "2023-10-03", 34.5,
  "2023-10-10", 34.8,
  "2023-10-17", 35.7,
  "2023-10-24", 33.3,
  "2023-10-31", 29.7,
  "2023-11-07", 25.1,
  "2023-11-14", 23.5,
  "2023-11-21", 21.9,
  "2023-11-28", 19.9,
  "2023-12-05", 14.7,
  "2023-12-12", 13.0,
  "2023-12-19", 13.8,
  "2023-12-26", 15.0,
  "2024-01-02", NA,
  "2024-01-09", 16.3,
  "2024-01-16", 18.2,
  "2024-01-23", 21.4,
  "2024-01-30", 19.8,
  "2024-02-06", 19.4,
  "2024-02-13", 21.3,
  "2024-02-20", 21.6,
  "2024-02-27", 21.7,
  "2024-03-05", 21.1,
  "2024-03-12", 21.2,
  "2024-03-19", 23.3,
  "2024-03-26", 23.9,
  "2024-04-02", 28.7,
  "2024-04-09", 30.0,
  "2024-04-16", 30.2,
  "2024-04-23", 30.1,
  "2024-04-30", 28.3,
  "2024-05-07", 25.1,
  "2024-05-14", 25.6,
  "2024-05-21", 25.7,
  "2024-05-28", 26.2,
  "2024-06-04", 21.1,
  "2024-06-11", 24.0,
  "2024-06-18", 25.8,
  "2024-06-25", 28.4,
  "2024-07-02", 33.4,
  "2024-07-09", 32.9,
  "2024-07-16", 30.8,
  "2024-07-23", 27.1,
  "2024-07-30", 21.4,
  "2024-08-06", 17.1,
  "2024-08-13", 20.0,
  "2024-08-20", 16.6,
  "2024-08-27", 16.4,
  "2024-09-03", 12.6,
  "2024-09-10", 9.7,
  "2024-09-17", 11.6,
  "2024-09-24", 11.6,
  "2024-10-01", 14.3,
  "2024-10-08", 18.3,
  "2024-10-15", 15.5,
  "2024-10-22", 16.7,
  "2024-10-29", 14.9,
  "2024-11-05", 16.4,
  "2024-11-12", 14.5,
  "2024-11-19", 16.3,
  "2024-11-26", 15.2,
  "2024-12-03", 14.9,
  "2024-12-10", 12.7,
  "2024-12-17", 15.0,
  "2024-12-24", 17.4,
  "2024-12-31", NA,
  "2025-01-07", 16.5,
  "2025-01-14", 21.5,
  "2025-01-21", 20.5,
  "2025-01-28", 17.4,
  "2025-02-04", 13.7,
  "2025-02-11", 13.1,
  "2025-02-18", 12.5,
  "2025-02-25", 9.4,
  "2025-03-04", 2.5,
  "2025-03-11", 2.1,
  "2025-03-18", 3.8,
  "2025-03-25", 5.8,
  "2025-04-01", 4.4,
  "2025-04-08", 0.0,
  "2025-04-15", 0.9,
  "2025-04-22", 1.1,
  "2025-04-29", NA,
  "2025-05-06", 0.0,
  "2025-05-13", 7.4,
  "2025-05-20", 8.4,
  "2025-05-27", 9.4,
  "2025-06-03", 10.0,
  "2025-06-10", 10.0,
  "2025-06-17", 13.4,
  "2025-06-24", 10.0,
  "2025-07-01", 10.0,
  "2025-07-08", 11.3,
  "2025-07-15", 10.2,
  "2025-07-22", 10.0,
  "2025-07-29", 12.2,
  "2025-08-05", 10.0,
  "2025-08-12", 10.0,
  "2025-08-19", 10.5,
  "2025-08-26", 10.0,
  "2025-09-02", 10.0,
  "2025-09-09", 10.0,
  "2025-09-16", 10.0,
  "2025-09-23", 10.0,
  "2025-09-30", 10.0,
  "2025-10-07", 10.0,
  "2025-10-14", 10.0,
  "2025-10-21", 10.0,
  "2025-10-28", 10.0,
  "2025-11-04", 15.0,
  "2025-11-11", 15.0,
  "2025-11-18", 20.0,
  "2025-11-25", 20.0,
  "2025-12-02", 25.1,
  "2025-12-09", 25.1,
  "2025-12-16", 25.1, 
  "2025-12-23", 0, # 2026/1/1(Thu)-7(Wed) subsidy
  "2025-12-30", 0,
  "2026-01-06", 0, 
"2026-01-13", 0, 
"2026-01-20", 0, 
"2026-01-27", 0, 
"2026-02-03", 0, 
"2026-02-10", 0, 
"2026-02-17", 0, 
"2026-02-24", 0, 
"2026-03-03", 0 
) |> 
  fill(subsidy, .direction = "down")
```

### Margin

I exclude consumption tax from retail price.

``` r
retail <- retail |> 
  mutate(
    consumption_tax_rate = case_when(
      date < as.Date("2004-04-01") ~ 0,
      date < as.Date("2014-04-01") ~ .05,
      date < as.Date("2019-10-01") ~ .08,
      .default = .1),
    retail_price = retail_price  / (1 + consumption_tax_rate)
  ) |> 
  select(date, retail_price)
```

I convert date to week, and combine retail, meti_dubai_weekly and
meti_subsidy. I fill missing data in retail_price.

``` r
retail_weekly <- retail |> 
  mutate(week = yearweek(date)) |> 
  select(week, retail_price)

meti_dubai_weekly <- meti_dubai_weekly |> 
  mutate(week = yearweek(start_date)) |> 
  select(week, dubai = price)
  
meti_subsidy_weekly <- meti_subsidy |> 
  mutate(week = yearweek(start_date)) |> 
  select(week, subsidy)

combo_weekly <- meti_dubai_weekly |> 
  left_join(retail_weekly, by = "week") |> 
  left_join(meti_subsidy_weekly, by = "week") |> 
  fill(retail_price, .direction = "down") |> 
  select(week, retail_price, dubai, subsidy)
```

As retail price on Monday reflects previous Tuesday-Monday average Dubai
crude oil price and Thursday-Wednesday subsidy, I take 2 weeks lag of
dubai and subsidy.

``` r
combo_weekly <- combo_weekly |> 
  mutate(
    across(dubai:subsidy, \(x) lag(x, n = 2)),
    gas_tax = if_else(week < yearweek("2026 W02"), 53.8, 28.7),
    subsidy = if_else(is.na(subsidy), 0, subsidy),
    margin =retail_price - (dubai + gas_tax - subsidy)
  )
```

Plot.

``` r
combo_weekly |> 
  filter(!is.na(margin)) |> 
  mutate(week = as.Date(week)) |> 
  ggplot(aes(week, margin)) +
  geom_line() +
  scale_x_date(date_breaks = "12 week", date_labels = "%y %b %d") +
  labs(x = NULL, y = "Yen per litre",
       title = "Margin (weekly) = retail price - (Dubai crude oil price (FOB) +\ngasoline tax - subsidy)",
       caption = "Note: excluding consumption tax\nSource: Agency for National Resources Energy, and Investing.com") +
  theme(axis.text.x = element_text(angle = 90, vjust = 0.5),
        panel.grid.minor.x = element_blank())
```

![Margin
(weekly)](README_files/figure-commonmark/margin_plot_weekly-1.png)

## Monthly update

### Retail prices (monthly)

``` r
retail_monthly <- retail |> 
  mutate(month = yearmonth(date)) |> 
  summarize(retail_price = mean(retail_price), .by = month) |> 
  filter(month >= yearmonth("2003-01"))
```

### Wholesale prices

Published wholesale prices are monthly, and do not include consumption
tax. Published retail prices are weekly, and include consumption tax. I
transform published retail prices into monthly retail prices which do
not include consumption tax.

``` r
wholesale <- read_excel("data/260227o5.xlsx",
                        sheet = "レギュラー",
                        col_types = c("text", "date", rep("numeric", 55))
                        ) |> 
  select(2:3) |> 
  set_names(c("date", "price")) |> 
  filter(!is.na(date))

wholesale <- wholesale  |>  
  mutate(month = yearmonth(date)) |> 
  select(month, wholesale_price = price) |> 
  filter(month >= yearmonth("2003-01"))
```

### Dubai crude oil price (monthly)

I get monthly Dubai crude oil prices in USD / barrel and exchange rates
in JPY / USD from [FRED by St. Louis FRB](https://fred.stlouisfed.org/),
and transform them into JPY / litre using 1 barrel = 158.987 litre.

``` r
dubai <- tq_get(c("POILDUBUSDM", "EXJPUS"), get = "economic.data", from = "2003-01-01")  |>
  pivot_wider(names_from = symbol, values_from = price) |>
  mutate(
    dubai_oil_price = POILDUBUSDM * EXJPUS / 158.987,
    month = yearmonth(date)
    ) |>
  select(month, dubai_oil_price)
```

### Subsidy

``` r
meti_subsidy_monthly <- combo_weekly |> 
  mutate(month = yearmonth(week)) |>
  summarize(subsidy = mean(subsidy), .by = month) 
```

### Margin (monthly)

Combine.

``` r
combo_monthly <- wholesale |> 
  left_join(retail_monthly, by = "month") |> 
  left_join(dubai, by = "month") |> 
  left_join(meti_subsidy_monthly, by = "month") |> 
  mutate(
    gas_tax = if_else(month < yearmonth("2026-01"), 53.8, 28.7),
    subsidy = if_else(is.na(subsidy), 0, subsidy),
    wholesalers_margin = wholesale_price - (dubai_oil_price + gas_tax - subsidy),
    retailers_margin = retail_price - wholesale_price
  )
```

Plot.

``` r
combo_monthly |> 
  filter(month >= yearmonth("2010 Jan")) |> 
  pivot_longer(wholesalers_margin:retailers_margin) |> 
  ggplot(aes(month, value)) +
  geom_line(aes(color = name), show.legend = FALSE) +
  annotate("text", x = as.Date("2015-01-01"), y = 5,
           label = "Retailers' margin", hjust = -0.2) +
  annotate("text", x = as.Date("2015-01-01"), y = 30,
           label = "Wholesalers' margin", hjust = -0.2) +
  scale_x_yearmonth(date_breaks = "1 year", date_labels = "%y") +
  scale_y_continuous(limits = c(0, 40), expand = expansion(add = c(0, 0))) +
  labs(x = NULL, y = "Yen per litre",
       title = "Retailers' margin = retail price - wholesale price\nWholesalers' margin = wholesale price - (Dubai crude oil price (FOB) +\ngasoline tax - subsidy)",
       caption = "Note: excluding consumption tax\nSource: Agency for National Resources Energy, and FRED")
```

![Margin
(monthly)](README_files/figure-commonmark/margin_plot_monthly-1.png)

END
