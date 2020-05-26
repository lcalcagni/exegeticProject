---
layout: default
title: Exploring Trundler
nav_order: 1

---

# Exploring Trundler
{: .fs-9 }

Trundler is a data product which gathers retail price data.

The Swagger documentation for the API is [here](https://api.trundler.dev/ ) and there is also an [R package](https://github.com/datawookie/trundler) to make your life easier and free of JSONs.

The intent of this document is to become familiar with the API and to look through the data and trying to find something interesting.

{: .fs-6 .fw-300 }


---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Getting familiar with the API using Python
Since I work mostly in Python, in this section I will explore the API using Python and the ``requests`` package.

[Go to this section]({% link docs/Getting_familiar.md %})
<br>
<br>  


## US vs NZ: retail prices comparison during COVID-19
There is a retailer located both in the USA and in New Zealand. Since these are two countries with such a different evolution of the number of COVID-19 infected people, I want to evaluate if there is some variability in the price of the products sold by the retailer that are shared in both countries, and if it has a correlation with the number of COVID-19 cases per day in each country.

[Go to this section]({% link docs/USvsNz.md %})
<br>
<br>  

## New functionalities
Many of the interesting conclusions that can be drawn from this data set are associated with:

1. Product price variability over time.
2. Product availability over time.
3. Product offer prices variability over time.

For people who don’t have access to the entire database, some new functionalities for the detection of variability of those variables could be useful and are proposed in this section.


[Go to this section]({% link docs/Newfunctionalities.md %})
<br>
<br>  

## Variability detection
Using the new functionalities I will search for variabilities in the dataset to detect insights that Trundler may reveal.
