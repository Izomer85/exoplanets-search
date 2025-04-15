# Predicting the Most Promising Stars for Habitable Planets

## Executive Summary

This project started with a simple question: rather than just cataloging exoplanets, can we predict which stars are statistically more likely to host habitable-zone planets and make those planets detectable using current human technology?

Our goal wasn’t to determine which stars do or do not host life-supporting planets. Instead, we built a framework to help astronomers prioritize their search by identifying the most promising stars based on available data. This is especially important because many exoplanets may exist beyond the limits of what current telescopes can detect.

## Why This Matters

The universe contains hundreds of billions of stars, and our detection methods are still evolving. Even if the majority of stars have planets in the habitable zone, we may not be able to see them—at least not yet. So instead of spreading our efforts evenly, we aimed to pinpoint the kinds of stars that are statistically more likely to reveal habitable-zone planets through current observational capabilities.

This approach doesn’t exclude any stars from being potentially interesting. It simply provides a data-driven way to focus limited observational resources where they’re most likely to yield results.

## Our Approach

We used real observational data from NASA’s Exoplanet Archive, which includes thousands of confirmed exoplanet-hosting stars. For each of these stars, we looked at whether any known planets orbit within the habitable zone—the range of distances where liquid water could exist.

We gathered a range of stellar attributes for these stars and their systems, including:
- Effective temperature  
- Luminosity  
- Radius  
- Mass  
- Metallicity  
- Age  
- Surface gravity  
- Proper motion  

These variables became the foundation for training a series of binary classification models to distinguish between stars with and without detectable habitable-zone planets.

After completing model training and validation, we applied these models to 500,000 stars from the Gaia DR3 catalog (see Appendix A), none of which intersect with the NASA Exoplanets Archive.

## Machine Learning and Modeling Strategy

We tested several model types:
- Logistic Regression (L1-penalized)  
- Random Forests (shallow and deep)  
- Decision Trees  
- Support Vector Machines  

Our evaluation focused on **maximizing recall**, to ensure we capture as many valid candidates as possible, while enforcing a **minimum precision threshold of 70%**, to reduce false positives.

Importantly, we didn’t rely on a single model. Instead, we used a composite scoring system that combined the outputs of several top-performing models. This provided a more robust signal when identifying high-likelihood candidates.

## Key Findings

Among the 500,000 Gaia stars evaluated, we focused on the top **0.01%** based on both **maximum** and **average** model probabilities. Only **42 stars** appeared in both top lists—our highest-confidence candidates.

These stars tended to be cooler, older, and more stable—matching the profile of stars that already host known habitable-zone planets. They now form a clear, prioritized short list for follow-up observation.

The most promising candidate across both score types is:  
**Gaia DR3 5542267270964254848**

We recommend prioritizing this star for telescope time or further analysis. See Appendix B for the full list.

## Next Steps

We see several opportunities for future improvements:
- Explore deeper ensemble models or neural networks  
- Retrain with upcoming discoveries from missions like PLATO or JWST  
- Incorporate stellar variability, binarity, and flare activity  
- Share top candidates with telescope committees or citizen science projects  

## Conclusion

This project highlights the value of flipping the traditional approach: instead of just cataloging detected exoplanets, we use machine learning to **predict which stars are statistically most likely to host detectable habitable-zone planets**.

With open datasets and data-driven modeling, we can sharpen our search for life in the universe—star by star.

---

## Appendix A: Gaia Data Query

The following query was used to pull Gaia data via the `astroquery` package:

```sql
SELECT TOP 500000
  gs.designation,
  gs.ra,
  gs.dec,
  gs.parallax,
  gs.pmra,
  gs.pmdec,
  gs.phot_g_mean_mag,
  gs.teff_gspphot,
  gs.mh_gspphot,
  gs.logg_gspphot,
  ap.radius_gspphot,
  ap.lum_flame,
  ap.mass_flame,
  ap.age_flame
FROM gaiadr3.gaia_source AS gs
JOIN gaiadr3.astrophysical_parameters AS ap
  ON gs.source_id = ap.source_id
WHERE
  ap.lum_flame IS NOT NULL AND
  ap.mass_flame IS NOT NULL AND
  ap.age_flame IS NOT NULL AND
  ap.radius_gspphot IS NOT NULL AND
  gs.teff_gspphot IS NOT NULL AND
  gs.mh_gspphot IS NOT NULL AND
  gs.phot_g_mean_mag IS NOT NULL AND
  gs.logg_gspphot IS NOT NULL AND
  gs.parallax IS NOT NULL AND
  gs.pmra IS NOT NULL AND
  gs.pmdec IS NOT NULL


## Appendix B: Top Candidates
The top 42 candidate stars—those that appeared in the top 0.01% across both max and average model scoring methods—are listed below:
1. Gaia DR3 5542267270964254848  
2. Gaia DR3 5098973942472679296  
3. Gaia DR3 5086419581269459712  
4. Gaia DR3 4498918632716948352  
5. Gaia DR3 2270771375724754816  
6. Gaia DR3 5917231492323978880  
7. Gaia DR3 5085428886933218304  
8. Gaia DR3 4499403105023922944  
9. Gaia DR3 5902380972893138304  
10. Gaia DR3 5817572475614527232  
11. Gaia DR3 2265144899846776064  
12. Gaia DR3 5917074949359384320  
13. Gaia DR3 2269196668913927808  
14. Gaia DR3 4498902517999299456  
15. Gaia DR3 5253820673315693440  
16. Gaia DR3 5085349584656832896  
17. Gaia DR3 4499356783798651392  
18. Gaia DR3 4471087175915086080  
19. Gaia DR3 4499162449416966272  
20. Gaia DR3 2268964328362753280  
21. Gaia DR3 2267045818011982976  
22. Gaia DR3 5091129579844220032  
23. Gaia DR3 5095607993784297600  
24. Gaia DR3 2266234825107556224  
25. Gaia DR3 5099924843936375296  
26. Gaia DR3 5947520666842892544  
27. Gaia DR3 5080543413172761088  
28. Gaia DR3 2266513894902202368  
29. Gaia DR3 5098702775417593984  
30. Gaia DR3 5081933161508910208  
31. Gaia DR3 4499000615051539584  
32. Gaia DR3 4500148780065654528  
33. Gaia DR3 2267826643066975744  
34. Gaia DR3 5085811379542251136  
35. Gaia DR3 5094098879714455296  
36. Gaia DR3 5086608319313421184  
37. Gaia DR3 2270940872314294784  
38. Gaia DR3 5094530644186718336  
39. Gaia DR3 5094693406267230592  
40. Gaia DR3 2268491504001786624  
41. Gaia DR3 4498929799629565184  
42. Gaia DR3 2269197489251449856
