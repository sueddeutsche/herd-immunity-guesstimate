Herd Immunity Guesstimate
================
@cendt
27 11 2020

This little project tries to make an educated guess on the time window
to reach herd immunity (and partial immunity) against Sars-Cov-2 in
Germany through vaccination. The results were published on Süddeutsche
Zeitung on November 27th, 2020 and can be found
[here](https://sz.de/1.5130292). All sources are listed at the end.

### Methods

We use public government information on planned vaccination programs to
estimate the population share of immune persons over time. Thresholds
for partial and full herd immunity are obtained from epidemiological
publications.

Vaccination programs are managed on state level. We use the example of
Baden-Württemberg for our estimate, as they published very detailed
information on their program. Other states are examined for plausibilty
checks, but there is no detailed data on vaccination plans available for
some states.

### General assumptions

Persons who were already infected with Sars-Cov-2 are immune and do not
need to be vaccinated. We use data from KIT forecast hub \[1\] to
estimate how many people will be infected towards the end of 2020.

We further assume that 50% of infections are discovered and included in
official figures. This is in the range of several scientific estimates,
but the true number might be lower or higher. \[2\] \[3\] \[4\]

We assume a basic reproduction number of 3.5, in accordance to
Rober-Koch-Institut \[5\]. With Test-Trace-Isolate and some other basic
NPIs, R can be reduced to 2, and with the ban of mass events, to 1.5
\[6\].

We assume that vaccins will be 90% effective, will stop vaccinated
people from spreading the virus and will require two shots to work.

The thresholds for Herd Immunity can be calculated from R using a simple
formula \[7\]

``` r
underreporting <- 2
bw_infected <- 194922 * underreporting
nrw_infected <- 353531 * underreporting
R_0 <- 3.5
R_1 <- 2.0
R_2 <- 1.5
vaccine_efficency <- .9
vaccine_shots <- 2
bw_pop <- 11100394
nrw_pop <- 17947221
full_herd_immunity <- 1 - 1/R_0
tti_herd_immunity <- 1 - 1/R_1
partial_herd_immunity <- 1 - 1/R_2
```

### Vaccination programs

The state of Baden-Württemberg plans to run 8-12 Zentrale Impfzentren
(ZIZ) with a daily capacity of 1500 shots each from mid-December 2020,
and 44 Kreis-Impfzentren (KIZ) with a daily capacity of 750 shots each
from mid-January 2021. Both are supposed to run 7 days/week. \[8\]

The state of North Rhine-Westfalia plans one shot per 10 inhabitants per
month at the beginning and increase capacity over time. We assume the
process to start efficently from mid-January 2021 as well. \[9\]

``` r
ziz_monthly <- 10 * 1500 * 30 / vaccine_shots * vaccine_efficency
kiz_monthly <- 44 * 750 * 30 / vaccine_shots * vaccine_efficency
nrw_monthly <- .1 * nrw_pop / vaccine_shots * vaccine_efficency
```

### Results

We use the estimates of monthly capacities to predict how many months it
will take to reach partial and full herd immunity.

Time till herd immunity, from mid-January 2021, in months:

Full herd immunity in Ba-Wü

``` r
round((bw_pop * full_herd_immunity - bw_infected - ziz_monthly) / (ziz_monthly + kiz_monthly),1)
```

    ## [1] 11.3

Herd immunity in Ba-Wü with TTI still in place

``` r
round((bw_pop * tti_herd_immunity - bw_infected - ziz_monthly) / (ziz_monthly + kiz_monthly),1)
```

    ## [1] 7.7

Partial herd immunity in Ba-Wü with mass gatherings still banned

``` r
round((bw_pop * partial_herd_immunity - bw_infected - ziz_monthly) / (ziz_monthly + kiz_monthly),1)
```

    ## [1] 4.8

![](https://datawrapper.dwcdn.net/jdxn1/full.png)

Full herd immunity in NRW with low initial capacities

``` r
round((nrw_pop * full_herd_immunity - nrw_infected) / nrw_monthly,1)
```

    ## [1] 15

### Discussion

If we assume that capacities in NRW can grow during 2021, our results
for Ba-Wü and NRW seem to be similar enough to use them as a rough
estimate for Germany in total. But of course, we do not know. This whole
thing is a very rough estimate with lots of uncertainties.

Also, the sucess of vaccination program depends on enough willing people
to get a shot or two. Avaiable opinion polling suggests there might be
enough volunteers in Germany, but some dont-know-yets remain to be
convinced. \[10\]

### Contact

Christian Endt, [@c\_endt](https://twitter.com/c_endt), c.endt \[ät\]
sz.de

### Sources

1.  <https://kitmetricslab.github.io/forecasthub/forecast>

2.  <https://arxiv.org/abs/2011.11413>

3.  <https://covid19.dunkelzifferradar.de/>

4.  <https://twitter.com/C_Althaus/status/1332076519095754752?s=20>

5.  <https://www.rki.de/DE/Content/InfAZ/N/Neuartiges_Coronavirus/Steckbrief.html#doc13776792bodyText4>

6.  <https://projekte.sueddeutsche.de/artikel/wissen/coronavirus-was-kommt-nach-dem-lockdown-e862488/>

7.  A. Kucharski: The Rules of Contagion, p. 57

8.  <https://www.baden-wuerttemberg.de/de/service/presse/pressemitteilung/pid/baden-wuerttemberg-bereitet-sich-vor/>

9.  <https://www.ruhrnachrichten.de/nachrichten/impfzentren-in-nrw-ab-dezember-massenimpfung-erst-mitte-2021-impfstrassen-geplant-1577948.html>

10. <https://www.zdf.de/nachrichten/politik/politbarometer-teil-shutdown-100.html?slide=20201126-0228-47-1003>
