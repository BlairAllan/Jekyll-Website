---
title: "Capital Bikeshare Data Analytics"
header:
  teaser:
---

An Exploratory Analysis report completed using R:

# Introduction

Capital Bikeshare (CaBi) is a bicycle-sharing system which operates in 
Washinton, DC. The system consists of classic and electric bicycles which users
unlock via their app or at kiosks. Users are charged for unlocking the bikes and
on a per minute basis. The company also offers a membership scheme and
day passes.

This report looks at analysing trip data to help uncover trends to provide
an insight into riders behaviours and identify impactful variables which could
be used to assist in business decisions or help identify and explain scenarios
such as the effect of COVID-19 on CaBi.

# Data

The data used throughout this report was acquired via CaBi's website where they
publish quarterly trip data. This includes when the trip occurred, the start and
end destination and if they were a member or a "casual" rider. We obtained this
data via two separate .csv files, one including all the trip data between
31/12/2018 - 31/12/2020, while the other contained information regarding the
latitude and longitude of the bike stations and the number of available docks.

After obtaining the data, several variables were cleaned and modified due to
their being either missing data points or due to input error.

### Steps taken to clean data:

1.  Variables renamed and transformed into snake case format
2.  Start date and End dates were identified to be swapped at various points
    -   Switched all applicable start/end datetimes so that end datetime was
        always the oldest of the two.
3.  Various data points appeared to be due to either a false start or users
    trying to re-dock or some errors when compiling the data.
    -   Trips that had a duration of less than 1 minute were removed.
    -   Trips that started and ended at the same station less than 5 minutes
        were removed.
4.  Membership variable was tidied to only having two levels.

### Join

We then combined the rides data with the bike share location data, which was
used to obtain the missing GPS coordinates of the bike stations.

### Exclusions

We have decided to exclude any trips which were under 1 minute as this is most
likely due to an error rather than it being an actual trip. We have also decided
to exclude any trips that start and end at the same station which is under 5 
minutes. The reason for this is that its unlikely a real trip that started and
ended in the same location did not last more than 5 minutes. We have also
decided to remove any trips that lasted over 24 hours as these are most likely
either errors or bikes that have been stolen. Additionally, CaBi states on their
website that any bikes missing for longer than 24 hours will be fined heavily
alongside extra time fees.





# Research Questions:

1.  What trends can be identified when looking at the frequency of trips across 
    2019-2021?

    -   Are bike rentals seasonal?

    -   How did COVID-19 impact the number of trips in 2020?

2.  Are there any identifiable trends between casual and member riders?

    -   Do members ride for a longer duration?

    -   Do casuals ride for shorter distances?

3.  What time is the busiest regarding number of rides? Is there an identifiable
    "rush hour" for cyclist?

    -   Alongside specific hours that are busy, are there specifically 
        busier days?
    -   Was there a decline in commuters during 2020 and did the "rush hours" 
        disappear?

4.  Is there a difference in who and when electric bikes are used?

    -   Are they more popular amongst members?

    -   Are they more popular across the weekend?

5.  Which station would be best for a marketing campaign?

    -   Which is the most popular station with members and casuals?

    -   Are there routes which are noticeably popular? has that changed over 
        the two years?

# Analysis:


## 1. What trends can be identified when looking at the frequency of trips?

Initially, we wanted to identify if there was any noticeable trends when 
plotting the frequency against time. We also made the assumption that we would 
see distinct seasonal demand, as when the weather gets colder and wetter less 
people would be willing to cycle.

### Are bike rentals seasonal?

<img src="/assets/r-figs/figure-html/question_1_1-1.png" width="600" />



In the above graph, as expected we see in 2019 an extremely clear case of 
seasonal demand. With the 2019 summer showing the peak number of rides, with it 
being the hottest and driest season. However, when looking at the year 2020 we 
don't see anything that resembles 2019. In 2020, we don't see any reversal of 
trend in spring unlike 2019 and we don't see any distinct peak 
during the summer.

(Seasons determined by events DC, 2020)

### What was the impact of COVID-19?


<img src="/assets/r-figs/figure-html/question_1_2-1.png" width="600" />

On the 23rd of March 2020, Washington D.C announced the initial Stay-At-Home 
Order across the city. Weeks prior to this announcement the USA and the world 
had declared COVID-19 a public health emergency resulting to people reducing 
their movement and a large number of people begin to work from home to avoid 
catching the virus.

### How did this effect CaBi's Bicycles?

Well in the above graph we can see from the months after the Stay-At-Home Order,
unlike in 2019, there is not a huge influx of member trips as you enter the 
warmer seasons (March to July). Instead what we see is a stagnation in member 
trips and a much large peak of casual riders compared to 2019.

What we can extract from this is that, due to there being more people staying 
home there was less tripped that were occurred during 2020. We can also predict 
that people did not want to pay for their annual or monthly membership and 
simply used a single trip or 24-Hour Pass instead. Which explains why during 
the hotter months of 2020 there was a increase in casual riders compared to 
2019 but the number of member trips had decreased by over 50% compared to 
2019's summer peak.

## 2. Are there any identifiable trends between casual and member riders?

Riders fall into one of two categories, those who have a reoccurring 
membership (Annual Member, 30-Day Member or Day Key Member) or those who are 
casual riders(Single Trip, 24-Hour Pass, 3-Day Pass or 5-Day Pass). This makes 
it an easy way to categories riders and get a sense of customer behaviour as do 
they think its worthwhile paying for an monthly/annual subscription or are 
people under the impression that it is cheaper and as convenient to pay for 
day passes.



Rider Category  |  Avg Ride Duration (min) |  Avg Ride Distance (km) |  Ride Frequency
--------------- | ------------------------ | ----------------------- | ---------------
casual          |                 40.00877 |                1.936255 |          617100
member          |                 17.12995 |                1.832611 |         2144767

Just based off the table summary we can identify two things from this data, 
one being that across the two years there were far more rides carried out by 
members than casual riders.

### Differences in duration and distance travelled?

Below we have two box-plots which provide the range of duration and distance 
between member and casual riders. Looking at the duration graph, interestingly 
members as noted in the above table, take on average much shorter trips and very
obviously try to stay within their free ride limit (30 minutes). Whereas when 
casual riders take out a single trip or 24 hour day pass its obvious that 
they're much more likely to complete a trip that's longer than 30 minutes as 
they aren't restricted and maybe even feel entitled to getting the most out of 
their rental.

When looking at the distance comparison, interestingly although casual averagely
complete trips that are longer, they don't go any further than members when 
looking at star to end stations. An explanation to this is that, they most 
likely stop on the way else where before ending at a similarly distant station 
due to them not being restricted by a ride limit.

<img src="/assets/r-figs/capital_bikeshare/question_2_1-1.png" width="600" />

## 3. What time is the busiest regarding number of rides?

The time of day with the most number of rides starting can be used to identify 
peak ride times and help understand when more bikes need to be available or 
when a good time would be to run a promotion or try create a promotion to 
spread the rides more evenly across the day to improve accessibility.

<img src="/assets/r-figs/capital_bikeshare/question_3_1-1.png" width="600" />


The above graph is a heat map of the frequency of rides throughout the two years
and provides us with a good idea of a "rush hour" pattern. First noticeable area
if that between 6AM - 9AM which appears to be the morning peak. The much more
larger peak time comes in the afternoon where from 3PM to 7PM across the entire
week is extremely popular time for riders to start their trip.

We can also extract that on Wednesday to Sunday, 4PM to 5PM was consistently the
busiest hour for the number of rides started throughout the two year period.

### Alongside specific hours that are busy, are there specifically busier days?

When plotting the frequency of rides against the day of the week we produce the
below graph. We can see that in 2019, the most popular day was on a Wednesday
followed by the Friday. Whereas in 2020, Saturday saw the highest number of
rides followed by Sunday.

The reason of to why there's such a decline in rides in 2020 compared to 2019
can be explained due to the COVID-19 pandemic. However, this graph does
demonstrate an interesting trend of which is due to the pandemic and potentially
explained by people working from home more frequently. Bike rentals in 2020
appear to be more of a leisurely activity and mode of transport compared to
2019. This is due to the most popular days being on the weekend compared to 2019
where they were the least busiest.

<img src="/assets/r-figs/capital_bikeshare/question_3_2-1.png" width="600" />

### Did the "rush hours" disappear in 2020?

When looking at the below heat map, we can see a noticeable difference to the
previous heat map above. The first clear difference is the lack of the 6AM - 9AM
rush hours Monday to Friday. There was also comparatively less rides later-on in
the day past 8PM whereas it was observed when including 2019 for there to be a
lot of rides during the night, especially on a Friday or Saturday. There was
also very few rides between midnight and 6AM during the weekdays, whereas during
2019 there was a substantial number of trips being started around midnight
and at 5AM.

<img src="/assets/r-figs/capital_bikeshare/question_3_3-1.png" width="600" />

## 4. Is there a difference in who and when electric bikes are used?

CaBi brought back their "e-bikes" range back in 2020. They relaunched with 1,500
electric bikes and they provide an alternative to their traditional bikes and
includes additional features such as pedal assistance and improved braking and
handle bars. The electric bikes cost the same amount to unlock but cost
\$0.15/min compared to the classic bike at \$0.05/min.

The 1500 electric bikes make up 30% of CaBi's bike fleet , so comparing how
frequently they are used amongst customers provide a great insight if people are
willing to pay the additional fees for the premium features. The graph below 
shows the frequency of rides throughout 2020 across the two types of bikes.
While starting off in July making up only 6.7% of rides, over the next two
months the number of customers choosing the electric bikes over the classic
increased to a peak of 21.3% in September. This is a promising result as
although ride numbers fell steeply from September onwards the percentage of
customers using and paying more for the electric bikes was fairly stable being
slightly above 20% of all rides. Suggesting that there is a strong demand for
the features that come with an electric bike and while its difficult to predict,
if they are able to maintain 20% of all rides its most likely that CaBi will see
this product launch a success especially when ride frequency picks up post
COVID-19 and into the warmer months.

<img src="/assets/r-figs/capital_bikeshare/question_4_1-1.png" width="600" />

#### Was there specific times where a type of bike was more popular?

Below is a density graph, plotting time of day against the type of bike. What we
can extract from this graph is that as seen previously, during the weekdays
there are two distinct peaks, one in the morning (around 8AM) and a larger
afternoon peak at 6PM. Although these two peaks are most noticeable in the
members group. When looking at the different type of bikes, electric bikes
appear to be more in demand during the weekdays in both casual and member
groups. We can also see demand is far more concentrated on the weekdays compared
to the weekends based off the weekend peaks being much broader and the weekday
peaks being slightly rightly skewed, especially in the casual group.

<img src="/assets/r-figs/capital_bikeshare/question_4_2-1.png" width="600" />


## 5. Which station would be best for a future marketing campaign?

One essential aspect of a marketing campaign is to make sure the most potential
customers are exposed to the campaign. So we had a look at which station is the
busiest and maybe the best place to convince more people to become members or be
locations to target as more people return to work post COVID-19.

The graph below consist of the top 10 stations based off trips starting from
there. In 2019, ***Columbus Circle*** was by far the most popular station,
especially with members. While,it ranked 8th with the ***Lincoln Memorial*** 
ranking 1st in 2020.

Interestingly this graph further supports a previous suggestion that individuals
switched from paying an annual membership to the casual passes, with the below
graph showing stations such as ***15th & P St NW***,
***New Hampshire Ave & T St NW*** and ***14th & Irving St NW*** of where
majority of rides during 2019 were via a member while during 2020 there was an
increase in casual riders starting from those stations although there was a
general decline in trips made.

One thing to take away from this, if its predicted that people will go back to
commuting to work in the future, similar to how it was in 2019 than
***Columbus Circle*** station could be a great region for a promotional campaign
to encourage users to return to using CaBi bike's.

<img src="/assets/r-figs/capital_bikeshare/question_5_1-1.png" width="600" />


### Are there routes which are noticeably popular?

Alongside looking at the most popular stations, looking at specific in-demand
routes can also be used to target a promotional campaign but also ensure there
are enough bikes supplied to prevent loss of potential riders.

The below graph consists of the top 10 routes (start station and end station)
that CaBi riders took in 2019 and 2020. Interestingly, the top route in 2019
(***Smithsonian-National Mall*** to ***Smithsonian- National Mall***) and 2020
(***Jefferson Dr & 14th St SW*** to ***Jefferson Dr & 14th St SW***) are trips
where the starting station is the end station too. Which it comes to no surprise
that majority of those routes are made by casual riders, who we estimate use a
24hr pass and simply cycle to various locations but keep the bike with
themselves for the day until returning to the same starting station.

Interestingly, while we were aware of ***Columbus Circle*** station being
popular, particularly in 2019, the graph below also helps us identify
***6th & H St NE*** a station that is fairly popular with members, especially
in 2019. With the route too ***6th & H St NE*** and back to
***Columbus Circle*** both being in the top 10, could be used as a great
promotional tool to encourage more riders to sign up as members as the city
recovers from COVID-19 and during the up coming spring and summer months.

<img src="/assets/r-figs/capital_bikeshare/question_5_2-1.png" width="600" />


# Conclusion

Throughout this report we have identified several key characteristics between 
users of the CaBi's bikes. This includes, members restrict their rental duration
based off their free ride restrictions, with very few member rides going across
30 minutes. We identified that bike rental was clearly seasonal during 2019,
with the hotter months showing more rides however due to the COVID-19 pandemic,
2020 did not follow any of these previously observed trends. We did uncover a 
trend due to the pandemic which was that there was less trips completed by 
members and a rise in casual users. We saw this shift between 2019 and 2020 in
regards to how the busiest days shifted from Wednesday in 2019 to Saturday 
in 2020.

These differences between 2019 to 2020, suggest a change in customer behaviour
with bike rental moving away from being popular for those commuting too it 
being more popular on the weekends and away from standard rush hours suggesting
the rentals are for more for leisure, with this idea being supported with a 
decrease in member trips while casual trips were nearly double of 2019 when 
comparing months.

We were also able to identify stations and areas of interest for a potential 
media campaign to recoup the members lost during the pandemic. Alongside 
identifying the most popular stations and routes, we uncovered an interesting 
characteristic of casual riders who frequently return to their starting station,
this could be due to them being tourist or because that station links to other 
public transport links, however additional information regarding who rented the
bike could provide interesting results and potential marketing opportunities 
amongst tourist.

The data from 2019 was pre-COVID and some of those habits and trends will 
return however a large shift in how people work and the number of people who 
work from home may have significantly impacted the
number of people using CaBi bikes as their form of their work commute. This 
impact has already been considered as in the last 6 months CaBi has increased 
their memberships free ride limit from 30 minutes to 45 while increasing the 
prices for casual rider passes, with the largest fee hike seeing the 1 hour 
pass starting at \$2.50 now compared to the original \$2.00 starting price, 
alongside an increase in cost per minute.

Further analysis would be helpful to get an insight of the profitability of 
this bikeshare system, is it more profitable that there are more casual riders 
who are paying more per minute of use but are less frequent or is this sudden 
change in demand due to COVID-19 a serious revenue problem?

Another interesting aspect would be to look at the effects of large events, 
such as the 4th of July and other various events such as sporting or elections 
in regards to popularity to use bikes to travel through busy streets which 
could provide additional marketing opportunities that could differentiate CaBi 
from their competitors such as Uber.