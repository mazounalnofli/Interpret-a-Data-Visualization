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
    city = input('please choose a city name from: (chicago, new york city, washington) ').lower()
    while city not in (CITY_DATA.keys()):
        print('please ensure to enter the correct city name')
        city = input('please choose a city name from: (chicago, new york city, washington) ').lower()
    

    # TO DO: get user input for month (all, january, february, ... , june)
    months= ['january', 'february', 'march', 'april', 'may', 'june', 'all']
    while True:
        month = input('please choose a month name: (all, january, february, march, april, may, june) ').lower()
        if month in months :
            break
        else:
            print('please choose a vaild month name')


    # TO DO: get user input for day of week (all, monday, tuesday, ... sunday)
    days = ['saturday', 'sunday', 'monday', 'tuesday', 'wednesday', 'thursday', 'friday', 'all']
    while True:
        day = input('please choose a day of the week: (all, saturday, sunday, monday, tuesday, wednesday, thursday, friday) ').lower()
        if day in days :
            break
        else:
            print('please enter a vaild day name')


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
    df = pd.read_csv(CITY_DATA[city])
    
    df['Start Time'] = pd.to_datetime(df['Start Time'])
    
    df['month'] = df['Start Time'].dt.month
    df['day_of_week'] = df['Start Time'].dt.day
    df['start hour'] = df['Start Time'].dt.hour
    
    if month != 'all':
        months = ['january', 'february', 'march', 'april', 'may', 'june']
        month = months.index(month) + 1
        df = df[df['month'] == month]
    
    
    if day != 'all':
        days = ['saturday', 'sunday', 'monday', 'tuesday', 'wednesday', 'thursday', 'friday']
        day = days.index(day) + 1
        df = df[df['day_of_week'] == day]


    return df




def time_stats(df):
    """Displays statistics on the most frequent times of travel."""

    print('\nCalculating The Most Frequent Times of Travel...\n')
    start_time = time.time()

    # TO DO: display the most common month
    print('Most common month : {}'.format(df['month'].mode()[0]))

    # TO DO: display the most common day of week
    print('Most common day  : {}'.format(df['day_of_week'].mode()[0]))

    # TO DO: display the most common start hour
    print('Most common start hour  : {}'.format(df['start hour'].mode()[0]))
    


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def station_stats(df):
    """Displays statistics on the most popular stations and trip."""

    print('\nCalculating The Most Popular Stations and Trip...\n')
    start_time = time.time()

    # TO DO: display most commonly used start station
    print('Most common start station : {}'.format(df['Start Station'].mode()[0]))


    # TO DO: display most commonly used end station
    print('Most common end station : {}'.format(df['End Station'].mode()[0]))


    # TO DO: display most frequent combination of start station and end station trip
    df['route']=df['Start Station']+","+df['End Station']
    print('Most common route : {}'.format(df['route'].mode()[0]))


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def trip_duration_stats(df):
    """Displays statistics on the total and average trip duration."""

    print('\nCalculating Trip Duration...\n')
    start_time = time.time()

    # TO DO: display total travel time
    print('Total travel time : ',(df['Trip Duration'].sum()).round())


    # TO DO: display mean travel time
    print('average travel time : ', (df['Trip Duration'].mean()).round())


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def user_stats(df):
    """Displays statistics on bikeshare users."""

    print('\nCalculating User Stats...\n')
    start_time = time.time()

    # TO DO: Display counts of user types
    if 'User Type' in df:
        print('Counts of user types : ',df['User Type'].value_counts())


    # TO DO: Display counts of gender
    if 'Gender' in df:
        print('Counts of gender : ',df['Gender'].value_counts())
    

    # TO DO: Display earliest, most recent, and most common year of birth
    if 'Birth Year' in df:
        print('Earliest year of birth  : ',int(df['Birth Year'].min()))
        print('Most recent year of birth  : ',int(df['Birth Year'].max()))
        print('Most Common year of birth  : ',int(df['Birth Year'].mode()[0]))
          
    
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

def display_data(df):
    #TO DO: Ask the user if he would like to display the raw data for that city in 5 rows based on user input
    print('\nRow data is available for verification... \n')
    
    index=0
    user_input=input('do you want to display 5 rows of raw data? , please write yes or no').lower()
    if user_input not in ['yes', 'no']:
        print('that is an invalid option, please write yes or no')
        user_input=input('do you want to display 5 rows of raw data? , please write yes or no').lower()
    if user_input != 'yes' :
        print('Thanks')
    else:
        while index+5 < df.shape[0]:
            print(df.iloc[index:index+5])
            index += 5
            user_input = input('do you want to display more 5 rows of row data? ').lower()
            if user_input != 'yes':
                print('Thanks')
                break
        
    
def main():
    while True:
        city, month, day = get_filters()
        df = load_data(city, month, day)

        time_stats(df)
        station_stats(df)
        trip_duration_stats(df)
        user_stats(df)
        display_data(df)

        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower() != 'yes':
            print('Thanks')
            break


if __name__ == "__main__":
	main()
