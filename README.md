In today’s digital age, streaming platforms have revolutionized the way we consume entertainment. Among these platforms, Netflix stands out as a global leader, offering a vast array of content to millions of subscribers worldwide. But have you ever wondered how Netflix usage varies from country to country?
full article - https://medium.com/@klphprabodini/global-streaming-insights-a-country-by-country-analysis-of-netflix-movies-and-tv-shows-92f9f9b35b5b

In this article, I’ll dive into a comprehensive country-by-country analysis of Netflix using Tableau, a powerful data visualization tool.

Project Overview

In this project, I will explore various aspects of Netflix data, focusing on the following key areas:

1.Top Genres

2. Weighted Rating (WR) for Popularity — introducing WR

WR = v/(v+m) * R + m/(v+m) * C

R = average rating of the show/movie ,v = number of votes for the show/movie , m = minimum votes required to be listed (1000) , C = the mean vote across the dataset

3. Monthly Weighted Rating (WR) and Vote Count — For added Month On Netflix

4.Releasing Shows by Year

5.Top Ratings

6.Top Director — Based On WR

This is the final outcome of this project: a simple dashboard for country-wise Netflix data.


Netflix Dashboard — Country Based ( Netflix — Country Dashboard | Tableau Public
Data Sources

I downloaded the Netflix titles Excel file from the Tableau site and added IMDb popularity, vote count, and original language using web scraping. The Excel file contains the following fields: duration_minutes, duration_seasons, type, title, date_added, release_year, ratings, description, show_id, and includes. Additionally, there are separate sheets for director, country, cast, and listed_in (category).

I used Jupyter Notebook and the Python library called imdb.py to scrape this data and add it to the Excel file.

Due to some issues, I couldn’t scrape all the data into the Excel file; I was only able to scrape approximately half of it. Additionally, some TV shows and movies don’t have one or more of the previously mentioned three fields (IMDb popularity, vote count, and original language).

Therefore, I removed all incomplete data and focused solely on the completed data for my analytics and visualizations And I joined the separate sheets in Tableau using Tableau connections.

Data analytics and Visualization

For data analytics, I primarily used weighted rating. Initially, I created calculated fields to calculate the weighted rating, with the weighting based solely on the country. To achieve this, I utilized Tableau’s Level of Detail (LOD) functions as follows:

// Replace 1000  with your minimum vote threshold if different
( [Vote Count] / ([Vote Count] + 1000) ) * [Popularity] + 
( 1000 / ([Vote Count] + 1000) ) * {FIXED [Country]: AVG([Popularity])}

To determine the best director for each country, I also obtained the weighted rating.
{FIXED [Director]: AVG([Weighted Rating (WR) by Country])}

I introduced summations for both movies and TV shows based on their original country.
{FIXED [Country], [Type]: COUNTD(IF [Type] = "Movie" THEN [Show Id] END)}
{FIXED [Country], [Type]: COUNTD(IF [Type] = "TV Show" THEN [Show Id] END)}
For the ratings bar chart, I used a text file named “radial bar chart values” that I found online. I then joined this file with my dataset and introduced angles (radials) , X , Y for it.

This is the Complete Path to Get This -https://medium.com/@klphprabodini/radial-progress-indicator-d8482f72800b

In analyzing the relationship between added month, weighted rating (WR), and vote count, I created displays where voting counts and ratings somewhat depended on the months of addition or exhibited a relationship with them.
To achieve this, I created separate graphs for weighted rating (WR) and vote count, and then combined them using dual axes.

To determine the top 7 shows in each country (including both TV shows and movies), I utilized weighted rating (WR) and created a calculated field named “Top 7” using the following criteria:
IF INDEX() <= 7 THEN "Top 7" ELSE "Others" END

To determine the best director, I utilized the weighted rating (WR) for each director and ranked them accordingly.
{FIXED [Director]: AVG([Weighted Rating (WR) by Country])}
RANK_UNIQUE([Average Weighted Rating], 'desc')

Finally, I combined all Tableau visualizations into one dashboard and added a filter for country selection. Each detail varies based on the selected country.
