# data-512-homework_2
## Goal

The goal of this project is to explore wikipedia articles for politicians across different countries. We're looking both at the raw number of articles and their quality using the ORES classifier to assign a predicted rating. 

## License and Data sourcing
The license to the source data is the Wikimedia Foundation Terms of Use: https://foundation.wikimedia.org/wiki/Policy:Terms_of_Use The ToU is applicable to this project since we are using a free and open license and the results cause no harm to anyone.

The API documentation is linked here: https://doc.wikimedia.org/generated-data-platform/aqs/analytics-api/reference/page-views.html

I also used sample code from Dr. David McDonald which is licensed through CC-BY, linked here: https://creativecommons.org/licenses/by/4.0/

Article quality predictions were sourced through the ORES API, documentation linked here: https://www.mediawiki.org/wiki/ORES

The list of article titles was crawled from this Wikipedia page - Politicians by nationality: https://en.wikipedia.org/wiki/Category:Politicians_by_nationality . 
The results are provided in: data/politicians_by_country_AUG.2024.csv

Population data came from the Population Reference Bureau: https://www.prb.org/international/indicator/population/table/
The results are provided in: data/population_by_country_AUG.2024.csv

## Data produced
My notebook creates two intermediate files:
1. results/politician_revid.csv - This CSV contains a list of politicians and their last revision ID for their Wikipedia page
2. results/prediction_df_final.csv - This CSV containins a list of politicians and their predicted ORES article score

The data schema for these two files are given below with a sample:
politician_revid
```python
title	lastrevid
Adolfo Chávez	1248979046
Aji Raden Sayid Mohammad	1243385610
Ana Paula Martins	1243011550
```

prediction_df_final
```python
article_title	best_guess_rating
Ahmadullah Wasiq	Start
Ali Masykur Musa	Start
Ali Pasha of Gusinje	GA
```

My notebook creates two final output files:
1. results/wp_countries-no_match.txt - This CSV contains a list of countries that have no population information
2. results/wp_politifcians_by_country.csv - This CSV information on politicians, their predicted ORES scores, the country and region information including population statistics.

The data schema for these two files are given below with a sample:
wp_countries-no_match.txt
```python
Guinea-Bissau
Korean
Korea, South
```

wp_politicians_by_country.csv
```python
article_title	country	revision_id	population	region	article_quality
Majah Ha Adrif	Afghanistan	1233202991	42.4	SOUTH ASIA	Start
Haroon al-Afghani	Afghanistan	1230459615	42.4	SOUTH ASIA	B
Tayyab Agha	Afghanistan	1225661708	42.4	SOUTH ASIA	Start
```

## Research Implications

Reflections:

Throughout this assignment, I felt I learned a lot about the time and effort needed to collect data, process it, and then run analysis. It seemed writing functions and documentation to collect data and then process it in a reproducible, clear way took MUCH longer than the time and effort it took to write and document the functions necessary to run the analysis portion. I would guess this reflects how most projects are - in that the planning, gathering of information, and deciding what exactly to analyze requires much more attention than the actual analysis itself. I also learned small lessons, some that I thought were interesting - and a little silly. For example, the ORES API seemed to fail for 10-20 random articles. Yet re-running the ORES API on those 10-20 articles would produce scores. Therefore, simply running two passes of the ORES API seemed to be the best solution rather than digging into what exactly was failing. 

Overall though, throughout this project I kept getting reminded of the phrase “garbage in garbage out”. The population data being represented in millions meant that some countries seemed to have “0” population, when in fact they just had under a million. Some countries weren’t documented in the population data and couldn’t be used. Countries had a disproportionate (and inaccurate) number of politician articles. For example, China seemed to only have ~20 politicians despite the fact that a cursory look at wikipedia shows over 200. Therefore, it's difficult to glean any meaningful insight from the results. 

1.I expected to see a strong bias towards a higher number of articles and high quality articles for politicians from English speaking countries. Because we used the English version of Wikipedia, contributors that wrote English well would likely contribute more to politicians they had more familiarity with (like politicians from their own countries). Additionally, I also expected to see far more politicians from highly populated countries and regions. The logic there is that there’s just more people that would be familiar with a politician and a higher chance that one of those people would write a wikipedia page for them. 

5. I definitely think that using this data could lead to biased or misleading results. There are two main limitations of the data. One is that, currently, the data is incomplete - it doesn’t have a full catalog of all politicians per country and it doesn’t represent all countries. But even if it was complete, because we are only documenting English written wikipedia pages, there’s going to be bias towards politicians that are well known in English speaking communities. Finally, there are certain countries that may use their own versions of Wikipedia or a site similar to that. Therefore, Wikipedia may not provide an unbiased insight into politicians for every country.

6. I think there’s a bunch of situations where using this data could be appropriate and useful. This data could be used with sentiment analysis to answer the questions - “How does the wikipedia community feel about certain politicians?”. Or “Which politicians have wikipedia pages that seem to be the most objective?”. Effectively, picking a question that implicitly states that wikipedia brings its own biases will allow the results to be significant.
