# Predict-number-of-visitors-to-restaurants
Predict how many future visitors a restaurant will receive (solution to kaggle challenge 'Recruit Restaurant Visitor Forecasting')

## Problem Define
Use time-series reservation and visitation data to predict the total number of visitors to a restaurant for future dates

## Motivation
> One common predicament is that restaurants need to know how many customers to expect each day to effectively purchase ingredients and schedule staff members. This forecast isn't easy to make because many unpredictable factors affect restaurant attendance, like weather and local competition. It's even harder for newer restaurants with little historical data.




## Data Description
The data comes from two separate sites:
> - Hot Pepper Gourmet (hpg): similar to Yelp, here users can search restaurants and also make a reservation online
> - AirREGI / Restaurant Board (air): similar to Square, a reservation control and cash register system

<details><summary>more details here</summary><p>

air_reserve.csv
>This file contains reservations made in the air system. Note that the reserve_datetime indicates the time when the reservation was created, whereas the visit_datetime is the time in the future where the visit will occur.
> - air_store_id: the restaurant's id in the air system
> - visit_datetime: the time of the reservation
> - reserve_datetime: the time the reservation was made
> - reserve_visitors: the number of visitors for that reservation

hpg_reserve.csv
> This file contains reservations made in the hpg system.
> - hpg_store_id: the restaurant's id in the hpg system
> - visit_datetime: the time of the reservation
> - reserve_datetime: the time the reservation was made
> - reserve_visitors: the number of visitors for that reservation

air_store_info.csv
>This file contains information about select air restaurants. Column names and contents are self-explanatory.
> - air_store_id
> - air_genre_name
> - air_area_name
> - latitude
> - longitude
>Note: latitude and longitude are the latitude and longitude of the area to which the store belongs

hpg_store_info.csv
>This file contains information about select hpg restaurants. Column names and contents are self-explanatory.
> - hpg_store_id
> - hpg_genre_name
> - hpg_area_name
> - latitude
> - longitude
>Note: latitude and longitude are the latitude and longitude of the area to which the store belongs

store_id_relation.csv
>This file allows you to join select restaurants that have both the air and hpg system.
> - hpg_store_id
> - air_store_id

air_visit_data.csv
>This file contains historical visit data for the air restaurants.
> - air_store_id
> - visit_date 
> - visitors: the number of visitors to the restaurant on the date

date_info.csv
>This file gives basic information about the calendar dates in the dataset.
> - calendar_date
> - day_of_week
> - holiday_flg: if this day is a holiday in Japan

</p></details>

## Evaluation
The submissions are evaluated with [root mean squared logarithmic error (RMSLE)](https://www.quora.com/What-is-the-difference-between-an-RMSE-and-RMSLE-logarithmic-error-and-does-a-high-RMSE-imply-low-RMSLE).

## Feature Engineering Tricks
- Holiday trick: mark holidays as weekend and the day before holiday as Friday. 
- Visit info: features of 21,35,63,140,280,350,420 days before are generated separately  
- Reservation info: features of 35,63,140 days before are generated separately 

## Models
3 models are trained with [LightGBM](https://github.com/Microsoft/LightGBM) and then ensemabled.
- Model 1: keep 14 days for testing and slip 30 times
- Model 2: keep 28 days for testing and slip 15 times
- Model 3: keep 42 days for testing and slip 10 times

