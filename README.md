!pip install google-play-scraper

from google_play_scraper import app

import pandas as pd

import numpy as np

# Scrape All available reviews
#(IF YOU WANT TO SCRAPE ALL AVAILABLE REVIEWS)

from google_play_scraper import Sort, reviews_all

result = reviews_all(
    'id.co.esb.esb_pos', #input_from_link_of_google_playstore_that_you_want_to_scrape
    sleep_milliseconds=0, 
    lang='id', #languange
    country='id', #country
    sort='relevant', #sort_by
)

#Scrape desired number of reviews
#(IF YOU WANT TO SCRAPE WITH A CERTAIN NUMBER)
#(e.g, you want to scrape 1000 reviews, then you can input , count = 1000 )

from google_play_scraper import Sort, reviews

result, continuation_token = reviews(
    'com.miHoYo.GenshinImpact',
    lang='id', # defaults to 'en'
    country='id', # defaults to 'us'
    sort=Sort.MOST_RELEVANT, # defaults to Sort.MOST_RELEVANT you can use Sort.NEWEST to get newst reviews
    count=100, # defaults to 100
    filter_score_with=None # defaults to None(means all score) Use 1 or 2 or 3 or 4 or 5 to select certain score

df_busu = pd.DataFrame(np.array(result),columns=['review'])

df_busu = df_busu.join(pd.DataFrame(df_busu.pop('review').tolist()))

df_busu.head()

len(df_busu.index) #count the number of data we got

df_busu[['userName', 'score','at', 'content']].head()  #preview userName, rating, date-time, and reviews only

#Run This Code to Sort the Data By Date 

new_df = df_busu[['userName', 'score','at', 'content']]
sorted_df = new_df.sort_values(by='at', ascending=False) #Sort by Newst, change to True if you want to sort by Oldest.
sorted_df.head()

my_df = sorted_df[['userName', 'score','at', 'content']] #get userName, rating, date-time, and reviews only

#SHOW DATA
my_df.head()

#SAVE
my_df.to_csv("file_name.csv", index = False)  #Save the file as CSV 
