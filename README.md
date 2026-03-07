Gasoline retailers’ and wholesalers’ margins in Japan
================
Mitsuo Shiota
2026-03-03

- [Get data](#get-data)
  - [Gasoline wholesale and retail
    prices](#gasoline-wholesale-and-retail-prices)
  - [Dubai crude oil price (monthly)](#dubai-crude-oil-price-monthly)
  - [Subsidy](#subsidy)
  - [Combine](#combine)
- [Plot](#plot)

Updated: 2026-03-07

## Get data

### Gasoline wholesale and retail prices

Agency for National Resources Energy under METI publishes gasoline
prices in its [web
site](https://www.enecho.meti.go.jp/statistics/petroleum_and_lpgas/pl007/results.html#headline1).

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

retail <- read_excel("data/260226s5.xlsx", 
                     sheet = "レギュラー",
                     col_types = c("text", "date", rep("numeric", 59))) |> 
  select(2:3) |> 
  set_names(c("date", "price")) |> 
  mutate(
    date = as.Date(date)
    ) |> 
  filter(!is.na(date))

retail <- retail |> 
  mutate(month = yearmonth(date)) |> 
  summarize(retail_price = mean(price), .by = month) |> 
  filter(month >= yearmonth("2003-01"))

retail <- retail |> 
  mutate(
    consumption_tax_rate = case_when(
      month < yearmonth("2004-04") ~ 0,
      month < yearmonth("2014-04") ~ .05,
      month < yearmonth("2019-10") ~ .08,
      .default = .1),
    retail_price = retail_price  / (1 + consumption_tax_rate)
  ) |> 
  select(month, retail_price)
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
meti_subsidy <- tribble(
  ~start_date, ~subsidy,
  "2022-01-17", 3.4,
  "2022-01-24", 3.7,
  "2022-01-31", 5,
  "2022-02-07", 5,
  "2022-02-14", 5,
  "2022-02-22", 5,
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
  "2025-12-23", 25.1,
  "2025-12-30", 25.1
)

meti_subsidy <- meti_subsidy|>
  filter(!is.na(subsidy)) |>
  mutate(month = yearmonth(start_date)) |>
  summarize(subsidy = mean(subsidy), .by = month)
```

### Combine

``` r
combo <- wholesale |> 
  left_join(retail, by = "month") |> 
  left_join(dubai, by = "month") |> 
  left_join(meti_subsidy, by = "month") |> 
  mutate(
    gas_tax = if_else(month < yearmonth("2026-01"), 53.8, 28.7),
    subsidy = if_else(is.na(subsidy), 0, subsidy),
    wholesalers_margin = wholesale_price - (dubai_oil_price + gas_tax - subsidy),
    retailers_margin = retail_price - wholesale_price
  )
```

## Plot

``` r
combo |> 
  filter(month >= yearmonth("2010 Jan")) |> 
  pivot_longer(wholesalers_margin:retailers_margin) |> 
  ggplot(aes(month, value)) +
  geom_rect(xmin = as.Date("2022-01-01"), xmax = as.Date("2025-12-01"), 
            ymin = -Inf, ymax = Inf, fill = "gray90", alpha = 0.3) +
  geom_line(aes(color = name), show.legend = FALSE) +
  annotate("text", x = as.Date("2022-01-01"), y = 10,
           label = "Subsidy period", hjust = -0.2) +
  annotate("text", x = as.Date("2015-01-01"), y = 5,
           label = "Retailers' margin", hjust = -0.2) +
  annotate("text", x = as.Date("2015-01-01"), y = 30,
           label = "Wholesalers' margin", hjust = -0.2) +
  scale_y_continuous(limits = c(0, 40), expand = expansion(add = c(0, 0))) +
  labs(x = NULL, y = "Yen per litre",
       title = "Retailers' margin = retail price - wholesale price\nWholesalers' margin = wholesale price - (Dubai crude oil price (FOB) +\ngasoline tax - subsidy)",
       caption = "Note: excluding consumption tax\nSource: Agency for National Resources Energy, etc.")
```

![](README_files/figure-gfm/margin_plot-1.png)<!-- -->

END
