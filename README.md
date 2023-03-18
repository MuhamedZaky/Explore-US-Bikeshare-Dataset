# Explore-US-Bikeshare-Dataset
A python code to explore a US bikeshare dataset


import time
import pandas as pd
import numpy as np

CITY_DATA = { 'chicago': 'chicago.csv',
              'new york city': 'new_york_city.csv',
              'washington': 'washington.csv' }

def get_filters():
    """
    Asks user to specify a city, month, and day to analyze.

    Returns:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    """
    print('Hello! Let\'s explore some US bikeshare data!')
    # TO DO: get user input for city (chicago, new york city, washington). HINT: Use a while loop to handle invalid inputs
    city = input("choose one city from: (chicago, new york city, washington)\n: ").lower().strip()
    while city not in CITY_DATA:
    	print("Wrong input, try again")
    	city = input("choose one city from: ( chicago, new york city, washington) : ").lower().strip()
    
    # TO DO: get user input for month (all, january, february, ... , june)
    months = ["all", "january","february", "march", "april", "may", "june"]
    month = input("Pick a month from (all, january, february, march, april, may, june): ").lower().strip()
    while month not in months:
    	print("Wrong input, try again")
    	month = input("Pick a month from: (all, january, february, march, april, may, june): ").lower()
    # TO DO: get user input for day of week (all, monday, tuesday, ... sunday)
    days = ["all", "monday", "tuesday", "wednesday", "thrusday", "friday", "saturday", "sunday"]
    day = input("What day do you want to view it\'s data?: (all, monday, tuesday, wednesday, thrusday, friday, saturday, sunday) ").lower().strip()
    while day not in days:
    	print("Wrong input, try again")
    	day = input("Wich day do you want to view it\'s data?: ").lower().strip()

    print('-'*40)
    return city, month, day
    
    
def load_data(city, month, day):
    """
    Loads data for the specified city and filters by month and day if applicable.

    Args:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    Returns:
        df - Pandas DataFrame containing city data filtered by month and day
    """
    #reading choosen city csv file and preparing choosen filters(month,day) data to display 
    df = pd.read_csv(CITY_DATA[city])
    df["Start Time"] = pd.to_datetime(df["Start Time"])
    df["month"] = df["Start Time"].dt.month
    df["day"] = df["Start Time"].dt.weekday_name
    df["Start hour"] = df["Start Time"].dt.hour
    
    if month != "all":
    	months = ["all", "january","february", "march", "april", "may", "june"]
    	month = months.index(month)+1
   
    if day != "all":
    	df = df[df["day"] == day.title()]


    return df


def time_stats(df):
    """Displays statistics on the most frequent times of travel."""

    print('\nCalculating The Most Frequent Times of Travel...\n')
    start_time = time.time()

    # TO DO: display the most common month
    print(f"The most common month:  {df['month'].mode()[0]}")


    # TO DO: display the most common day of week
    print(f"The most common day: {df['day'].mode()[0]}")


    # TO DO: display the most common start hour
    print(f"The most common start hour: {df['Start hour'].mode()[0]}")


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)
    
    
def station_stats(df):
    """Displays statistics on the most popular stations and trip."""

    print('\nCalculating The Most Popular Stations and Trip...\n')
    start_time = time.time()

    # TO DO: display most commonly used start station
    print(f"The most commonly start station: {df['Start Station'].mode()[0]}")


    # TO DO: display most commonly used end station
    print(f"The most commonly end station: {df['End Station'].mode()[0]}")


    # TO DO: display most frequent combination of start station and end station trip
    df['trip'] = (df['Start Station']+ "," +df['End Station'])
    print(f"The most common trip: {df['trip'].mode()[0]}")


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)
    
    
def trip_duration_stats(df):
    """Displays statistics on the total and average trip duration."""
    
    print('\nCalculating Trip Duration...\n')
    start_time = time.time()

    # TO DO: display total travel time
    print(f"The total travel time: {df['Trip Duration'].sum().round()}")
    
    # TO DO: display mean travel time
    print(f"The average travel time: {df['Trip Duration'].mean().round()}")


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def user_stats(df,city):
    """Displays statistics on bikeshare users."""
    print('\nCalculating User Stats...\n')
    start_time = time.time()

    # TO DO: Display counts of user types
    print(df["User Type"].value_counts().to_frame())


    # TO DO: Display counts of gender
    if city != "washington":
    	        print(df['Gender'].value_counts().to_frame())
    	        # TO DO: Display earliest, most recent, and most common year of birth
    	        
    	        print(f"The earliest year of birth: {df['Birth Year'].min()}")
    	        print(f"The most recent year of birth: {df['Birth Year'].max()}")
    	        print(f"The most common year of birth: {df['Birth Year'].mode()[0]}")
    else:
    	print("Regarding the gender and the year of birth there\'s no data available for Washington")
    


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def display_raw_data(df):
	"""Asking user if he wants to check raw data.
	"""
	print("\nDisplaying raw data...\n")
	#taking user's input
	count = 0
	user_answer = input("If you want to check 5 rows of raw data type: yes, if you don\'t type: no: ").lower().strip()
	if user_answer not in ["yes", "no"]:
		print("Invalid input, choose yes or no")
		user_answer = input("If you want to check 5 rows of raw data type (yes), if you don\'t type (no) : ").lower().strip()
	elif user_answer != "yes":
		print("Thank you")
	else:
					while count + 5 < df.shape[0]:
						print(df.iloc[count : count+5])
						count += 5
						user_answer = input("If you want to check 5 rows of raw data type (yes), if you don\'t type (no) : ").lower()
						if user_answer != "Yes":
						 	print("Thank you")
						 	break
		
		
		
def main():
    while True:
        city, month, day = get_filters()
        df = load_data(city, month, day)

        time_stats(df)
        station_stats(df)
        trip_duration_stats(df)
        user_stats(df,city)
        display_raw_data(df)

        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower().strip() != 'yes':
            break


if __name__ == "__main__":
	main()
