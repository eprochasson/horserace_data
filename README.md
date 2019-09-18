# Hong Kong Jockey Club and Singapore TurfClub Horse Race Data

## Foreword

Gambling is bad, m'kay.

This repository provides horse race data for the Hong Kong Jockey Club and the Singpore Turf Club. The data was obtained by scraping their respective public websites, and
comes with no guarantee of correctness whatsoever.

A particularly cool thing is that we also provides historical odds for a period of time for HKJC race. Being able to predict what would be the final odds for a 
given horse on a given race is _extremely_ valuable, but historical data are, as far as we know, not publicly available. We thus wrote a scraper, that ran for 2 seasons, that
 probed the odds at regular interval up to the race start. This allows for cool time series analysis that can't be done with historical data available on the public websites.

That dataset is provided as a set of compressed CSV files, that can easily be reloaded to a database of your choice, a pandas dataframe, or even Excel if you don't know any better.
The HKJC website is just a little less crappy that the TurfClub one, in general HK data contains more information than their Singaporean counterpart.


## Dataset

The schemas are not explicitely described because they should be quite straightforward and because I'm lazy.

### [`horses`](data/horses.csv.gz)

List of all the horses (some retired) for HKJC and SGTC that ran a race, up to 2018-07-01.

### [`performances`](data/performances.csv.gz)

Each row of this table is the result for a single horse in a single race, with their position, final odds (for first place -- more explicit dividends can be found in the `all_dividends` table for HK races). 
This is the main source of information for the statistics you want. Note that some races found in the performance table do NOT have their counterpart in the `races` table.

This contains historical results from 1979 up to 2018-06-27 for Hong Kong, and 2002-03-08 to 2018-04-24 for Singapore.

### [`races`](data/races.csv.gz)

List of all the races ran between 2016-09-28 and 2018-06-27 for Hong Kong and 2016-09-25 to 2018-04-24 for Singapore. Note that some races not found in this table still have 
available performances in the `performances` table.

### [`all_dividends`](data/all_dividends.csv.gz)

Each row of this table contains the JSON-encoded dividend results (which can be used to infer the final odds) for each race ran in Hong Kong between 2016-09-28 and 2018-06-27.

### [`sectional_times`](data/sectional_times.csv.gz)

Each row contains the sectional times for races ran between 2008-06-05 and 2018-06-27. That's basically, for a given horse in a given race, what was their placing and time at given section of the track. 

### [`live_odds`](data/live_odds.csv.gz)

Live odds evolution for Hong Kong race ran between 2016-09-27 and 2018-06-27. HKJC is a "pari-mutuel" system where odds for a given horse / bet evolve up to the start of the race.
This dataset was collected by poking for the odds at various interval before a race (with the interval getting smaller as the race was getting closer, since that's when the odds tend to vary the most). As far
as we can tell, this kind of information can not be found in historical dataset, and can only be collected in real-time.

## FAQ

### Where's the scraper?

I initially planned on releasing the scrapers for the data, but few things stood in my way:

- the data was obtained through scraping, and the websites changed. It happened many times while the scrapers were live, from 2016 to 2018, but the current version 
drifted too far from the latest working version. Scraping is inherently brittle and the code rots at a very fast pace. I am working on releasing a version that work but don't hold your breath.
- the code was designed for a much larger purpose (probe for new race, pull and store results in a database) and requires a lot of cleanup to specifically isolate the scraping bits.

### Why the discrepancy in the dates? Why some information available and some not? Why is the schema so dirty?

The websites offer various historical information in various format. It's a hot mess, really. Sometimes we get all we need, sometimes we need dirty hacks to get the rest, 
sometime the information is not available at all ¯\\\_(ツ)\_/¯. This is also why there are some fields that are sometimes available, sometimes empty.

We tried to stick to whatever information was available from a given source. A sane developer would not have organize the data like that, but that's how we found it.

## See also

We played with this data for fun, and talked about it in [this talk](https://docs.google.com/presentation/d/1fXiIy6UGYA_I4yI_y_l6bIZ1KAL2aKbsfSqEs4t05b4/edit?usp=sharing). The morale of it is
that you should probably not expect to make bank out of this dataset, but it's really cool for data analysis fun.

The scraping was also supporting a really cool Android app aptly named _Horsemaster_, but it was a bit of a commercial failure and was pulled out for various reasons.
