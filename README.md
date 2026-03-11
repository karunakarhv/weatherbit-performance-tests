# Qantas Loyalty - WeatherBIT API - Performance Testing

## Prerequisites

| Tool               | Version | Install                                                  |
|--------------------|---------|----------------------------------------------------------|
| Java JDK           | 11+     | `brew install openjdk@11`                                |
| Maven              | 5.6.3+  | [Download](https://jmeter.apache.org/download_jmeter.cgi)                                             |
| Weatherbit API key | free    | [Sign up here](https://www.weatherbit.io/account/create) |

---

## 1. Overview

This document describes how to approach performance testing the Weatherbit `/current`
weather endpoint using Apache JMeter. It covers test plan structure, key metrics to
capture, and how to interpret results.

---
## 2. Thread Group Configuration

| Parameter            | Baseline | Soak Test | Spike Test |
|----------------------|----------|-----------|------------|
| Number of Threads    | 10       | 20        | 100 (burst)|
| Ramp-up Period (s)   | 30       | 60        | 5          |
| Loop Count           | 5        | Infinite  | 1          |
| Duration (s)         | —        | 3600      | 60         |

## 3. CSV Data Set Config
 * Injects City/Country Pairs from CSV
 * Filename: cities.csv
 * Variable names: city, country

## 4. Assertions in JMeter

```
1. Response Code Assertion:  HTTP 200
2. JSON Path Assertion:      $.count >= 1
3. JSON Path Assertion:      $.data[0].temp exists
4. Duration Assertion:       <= 2000 ms
```

## 5. Run in Commandline
```bash
jmeter -n \
  -t PerformanceTesting.jmx \
  -l results/results.jtl \
  -e -f -o results/html-report \
  -Japi.key=xxx
```