# tiktok-billboard

## Searching for patterns of success on TikTok Billboard Top 50 chart

### About
Analyzing inaugural 4 weeks of TikTok BillBoard Top 50. Softwares utilized: Google Sheets, R.

### Overview
Starting on Thursday of the week of September 16, 2023, Billboard weekly began ranking the top 50 songs on TikTok songs in the United States—**the Billboard TikTok Top 50**. This is the first time Billboard began tracking these metrics on TikTok in the US, which opens up new possibilities for examining what the most popular and influential songs are—especially amongst Gen Z—and any other points of commonality.

### Industry understanding
There now exists the possibility to look at weekly realtime data and track various data points to seek out potential correlations and patterns for **songwriters**, **producers**, and **music industry professionals**. For the **songwriter**, does the number of words matter to popularity? Does your country of origin matter? For the **producer**, what keys are most popular? What genre? What tempo? And for the **music industry professional**, which songs are taking off—brand new ones, last year’s crop, or decades’ old classics?

### Data understanding
The chart for the [TikTok Billboard Top 50](https://www.billboard.com/charts/tiktok-billboard-top-50/) provided the base info (such as ranking, song title, artist name, and previous week’s ranking). For more detailed analysis, querying each song through [SongBPM.com](https://songbpm.com/) was useful for tempo, duration, and key center metadata; genre info came from [Apple Music](https://music.apple.com/); and Google queries encompassed the artist country-of-origin sourcing. Each of these sources were consulted to comprise the full datasets within Google Sheets ([see the document here](https://docs.google.com/spreadsheets/d/1FY9F-yOZIdBuBr7ebJiZcdXXqN_IJwJQ9X11inbrrNw/edit?usp=sharing)).

I began with an intro sheet tab, then each of the four weeks of the Billboard chart are divided up into their own sheet tabs (duplicated to include a version for each week with and without added analysis formulas) followed by two sheet tabs combining the songs and the rankings (respectively).

It would seem that TikTok partners with Billboard to share data, and as Billboard describes it, this is their process: “A weekly ranking of the most popular songs on TikTok in the US based on creations, video views and user engagement” ([Billboard](https://www.billboard.com/charts/tiktok-billboard-top-50/)). Therefore, based on that definition including “creations,” it would seem there might be potential for bias within the weekly dataset on Billboard’s end, as they are the gatekeepers of the particulars behind the rankings. However, basing remaining popularity off video views and user engagement—combined with Billboard’s history ranking other genres within music for decades—makes this weekly ranking a new, valuable dataset for many songwriters, producers, and industry professionals in the music industry looking to glean deeper insights into market trends.

There are no elements of the songs in this dataset that would infringe on composition or recording copyrights of said songs. The data from Billboard is publicly available, as is other data such as artist country of origin and song metadata.

The data will allow me to look at various subsets (top 5 of a given week, top 50 across 4 weeks, etc.) to compare and contrast various values with the song’s given rank or popularity.

Only **two main problems** arose from the data, being very different types of problems yet both with solutions. **First** was that the key center info was not always accurate from SongBPM.com. So utilizing my bachelor’s degree in music, I listened for and personally corrected each song’s key center within my dataset.

**Second** was that, although each song was ranked each week, there did not exist a cumulative measurement of popularity—it would seem the longer a song was on the chart, the greater its current-state popularity would be because of familiarity. So I created a new datapoint called ‘_impact score_’ which is simply a given song’s ranking inverted by 50 multiplied by the number of weeks it has been on the chart. Similarly, ‘_total impact score_’ takes each song’s weekly impact score and sums them together.

For example, from October 7, “Sky” by Playboi Carti debuted on the chart at number 1. So 50 x 1 (1 inverted by 50 {= 50} _times_ 1 week on chart) equals an impact score of 50. But “Paint the Town Red” by Doja Cat ranking at number 2 received an impact score of 196 because of how long the song had been on the chart (2 inverted by 50 {= 49} _times_ 4 weeks on chart).

At first, I used Google Sheets to clean, organize, and create formulas to find initial patterns in the data, especially within each week’s chart. If this were instead a dataset of every Billboard Chart from every week ever, a tool like SQL would have been far more appropriate.

I then imported the combined song and ranking datasets into R in order to take a deeper look into some of the themes and patterns taking place across the entire 4-week span and to create visualizations of these findings.

Because I was the compiler and the one who entered the data into Google Sheets, there was perhaps less to clean with the data than if I were to have scraped data from a table online. I edited the song name to be case-accurate from its original Billboard form (as verified on Apple Music); in some cases where a song (especially a soundtrack) had multiple artists, ‘et. al.’ replaces any listed artists past two. As aforementioned, I personally reverified key centers. I renamed columns to be consistent and descriptive. Finally, I ensured the data itself did not repeat or had not translated something inaccurately; I did this by sorting the data by various parameters to look for gaps and used conditional formatting to look for duplicates.

Within the spreadsheets, column data starts with the _ranking_, then moves onto _song_, _artist_, _country_, _last week’s position_, _peak position_, _weeks on chart_, _key center_, _duration_, _BPM_, _genre_, and _original release date_.

I needed to add a few extra columns of info for each song in order to make some formulas work for the purposes of analysis. These include the _inverted song rankings_ (50 = 1, 1 = 50, etc.) in order to multiply by weeks-on-chart for the _impact score_; the _number of words in each title_; and a column simply listing that week’s _chart date_ in order to subtract days from via its original release date for a total of days elapsed from release date to chart date.

Many of the comparisons seemed non-conclusive in their correlation. I started by analyzing in Google Sheets running functions like MIN, MAX, and AVERAGE and dividing across cells to create the various measurements in the dataset.

Unsurprisingly, song duration played no factor into ranking or impact score, as only snippets of each song are taken, thus equalizing this outcome.

Perhaps more surprisingly was that there seemed to be no correlation between the novelty of a song and how often or high it charted; I broke up every song into 5 categories using COUNTIFS functions—looking at original song release date to chart date: New (less than 30 days), Fresh (30-90 days), Familiar (90-365 days), Old (1-10 years), and Classic (10+ years). I created some pi chart visualizations to compare each week’s Top 50 as well as Top 5, and there was a pretty even distribution. Although Fresh songs added up to the most common across 4 weeks at 26.3% (33.3% Top 5), songs over a year old (Old and Classic combined) still accounted for 37.1% of trending songs (and 26.6% of the Top 5—over a quarter!).

<img width="1015" alt="GRAPHnovelty" src="https://github.com/i-am-nate/tiktok-billboard/assets/112446964/c4276985-607b-4ff3-8b82-33ec7cf140a4">

I was able to use additional functions such as COUNTUNIQUE to find, for instance, that Hip-hop/rap was the most popular genre across each of the 4 weeks; I used COUNTIF to discover, in a given week, 72%-94% of artists were American. However, if I were to find even more insights, I needed to look to R.

### Evaluation
After importing the combined song and rank csvs into R and running some more calculations, I created visualizations to look at 5 more properties.

**First**, the genre question. Here I combined some genres together (like Rap into Hip-hop/rap, Latino Urbano into Latino, etc.) before analyzing. We knew that Hip-hop/rap was the most popular genre, and the bar graphs bear that out. Now we also discover that, all competing for the next most popular genres are Pop, R&B/soul, and Alternative (with Soundtracks a respectable pace behind these and Jazz being the least popular genre represented).

![GRAPHgenres_16-9](https://github.com/i-am-nate/tiktok-billboard/assets/112446964/10f68a41-8186-41ce-8f70-752c8167f3f3)

**Second**, what countries are artists from? Although across the 4 weeks there are 13 countries represented, the United States is by far the highest ranked country (as the bar graph illustrates in grand fashion).

![GRAPHcountry_16-9](https://github.com/i-am-nate/tiktok-billboard/assets/112446964/67016147-893c-4e5e-a39f-b42d241861d6)

**Third**, does the key of the song matter? After giving R a custom order to accurately look across the keys in each of the 4 weeks, we can see a few key centers which are close to the top of the circle of fifths represent the largest share of songs (as we see from the larger circles of C, D, A, and F—though noticeably scarce is G). However, these are certainly not a majority of the Top 50. In fact, not only are all 12 diatonic key centers represented across all 4 weeks, but—due to microtonal keys—there are a full 17 key centers represented. The 5 microtonal keys represented (A½♭, B¼♭, B¼♯, C¼♯, and F♯♯ ) almost all contain a parenthetical title tag of “(Sped Up)” or something similar.

![GRAPHkey_16-9](https://github.com/i-am-nate/tiktok-billboard/assets/112446964/5f5b8146-11e6-44e5-b09a-c6d493d04b59)

**Fourth**, how about the tempo of the song? Here, comparing each song’s total impact score to its BPM, we discover a slight bell curve upwards toward songs around 85 beats per minute and again a slope up for songs approaching 170. There is a bit of a dip in the middle around 125 beats per minute. Genres and their average tempos certainly play a role here, with Hip-hop/rap and R&B/soul contributing to the upward tick for the down-tempo songs and Pop, Dance, and Electronic for the up-tempo.

![GRAPHbpm2_16-9](https://github.com/i-am-nate/tiktok-billboard/assets/112446964/49774b96-df24-4b5f-94db-3184dea3d14c)

Finally **fifth**, do the number of words in the song title contribute either way to a song’s impact? Measuring a song’s total impact score compared to its title’s word count did not find significant correlation. At most, we find an average difference of +60 impact points comparing songs with 2-word titles to ones with 4 words, but each category of word count had many songs +200-300 impact points above average. There was also a sharp declining slope for songs approaching 5 and certainly 6+ words; however, the sample size for songs with this many words was far fewer than songs with 5 or fewer words in the title.

![GRAPHwords2_16-9](https://github.com/i-am-nate/tiktok-billboard/assets/112446964/f1b3e614-c81b-4a60-9276-c04a4d1a554c)

### Conclusion
Surely there are more insights to be pulled from the data gathered within this dataset, and tracking this brand new chart further into the future will undoubtedly reveal even more insights, patterns, and correlations. But to wrap up all these findings, what insights have we gleaned for the songwriter, producer, or music industry professional looking to climb the TikTok Billboard chart?

For the **songwriter**: song duration can be long or short, perhaps keep your song title under 5 words, and ultimately the key center doesn’t matter so much (although especially C, D, and A seem to have the most representation). But being from the US is certainly a helpful quality, as is writing for popular genres such as Hip-hop/rap, R&B/soul, or Pop.

For the **producer**, those same genres are still the most popular right now; slower or faster tempo tends to perform better than mid-tempo; and non-diatonic key centers are certainly charting in large part thanks to “(Sped Up)” remixes.

For the **music industry professional**, perhaps the biggest insight to glean is that many songs that are trending are over 10 years old, and even more are at least a year old. Although songs released within the past 90 days still chart overall the most, perhaps just as much attention should be given toward not just brand new songs but also to ones at least a year old.
