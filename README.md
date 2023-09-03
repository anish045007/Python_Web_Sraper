# Python_Web_Sraper

# Import libraries
import time
import datetime
import pandas as pd
from tabulate import tabulate

# Define the ticker and datetime frame 
ticker = 'CL=F'
period1 = int(time.mktime(datetime.datetime(2001, 1, 1).timetuple()))
period2 = int(time.mktime(datetime.datetime(2023, 9, 2).timetuple()))
interval = '1d'  # 1mo for 1-month interval

# Construct the dynamic query string
query_string = f'https://query1.finance.yahoo.com/v7/finance/download/CL=F?period1=978307200&period2=1693612800&interval=1d&events=history&includeAdjustedClose=true'

# Read the CSV data and parse the 'Date' column as dates, setting it as the index
df = pd.read_csv(query_string, parse_dates=["Date"], index_col="Date")

# Remove the time portion from the date index column
df.index = df.index.date

# Save the data to a CSV file
df.to_csv(f'{ticker}_1d.csv')

# Print the first 273 rows of the DataFrame in a tabular format
table = tabulate(df.head(273), headers='keys', tablefmt='pretty')
print(table)


"""**Data Manupalation**
drawing the sample of a specific year
claculating mean of price of crude oil in that year at closing price"""

# First, ensure the index is a datetime object
df.index = pd.to_datetime(df.index)

# Then, filter the DataFrame for the monthly 2014
month_2014_data = round(df['2014-1'],3)

# Print the data for the mounth 2014
print(month_2014_data)

print()

# Calculate the mean of the 'Close' column for the mounth 2014 and round to three decimal points
month_2014_mean_close = month_2014_data['Close'].mean()
month_2014_mean_close_rounded = round(month_2014_mean_close, 3)

print("Mean Close Price for 2014 (rounded to 3 decimal points):", month_2014_mean_close_rounded)

print()

# Calculate the mean of the 'Open' column for the month 2014 and round to three decimal points
month_2014_mean_open = month_2014_data['Open'].mean()
month_2014_mean_open_rounded = round(month_2014_mean_open, 3)

print("Mean Opening Price for 2014 (rounded to 3 decimal points):", month_2014_mean_open_rounded)



"""**Annual Mean**

1. Calculate the annual average cost of per barrel cost of crude oil 
2. create a bar chart of the same data """

# Resample the 'Close' column to calculate the yearly mean
yearly_mean_close = round(df['Close'].resample('Y').mean(),3)


# Create a new DataFrame to store the annual means
annual_means_df = pd.DataFrame({'Yearly Mean Close': yearly_mean_close})

# Print the annual mean values for both columns
print(annual_means_df)

print()

# Create a bar chart to visualize the annual means
ax = annual_means_df.plot(kind='bar', figsize=(10, 6))
plt.title('Yearly Mean Open and Close Prices of Crude Oil')
plt.xlabel('Year')
plt.ylabel('Mean Price')
plt.legend(loc='upper left')

# Format the x-axis labels to display only the year
ax.set_xticklabels([x.strftime('%Y') for x in annual_means_df.index])

plt.show()


""" **Trend Analisis**"""
# Calculate 50-day and 200-day moving averages
df['50-day MA'] = df['Close'].rolling(window=50).mean()
df['200-day MA'] = df['Close'].rolling(window=200).mean()

# Plot the crude oil prices and moving averages
import matplotlib.pyplot as plt

plt.figure(figsize=(12, 6))
plt.plot(df.index, df['Close'], label='Crude Oil Price', color='blue')
plt.plot(df.index, df['50-day MA'], label='50-day MA', color='orange')
plt.plot(df.index, df['200-day MA'], label='200-day MA', color='green')

plt.xlabel('Date')
plt.ylabel('Price')
plt.title('Crude Oil Price Trend Analysis')
plt.legend()
plt.grid(True)
plt.show()




"""**Voilation Analysis**"""
# Import libraries
import time
import datetime
import pandas as pd
import matplotlib.pyplot as plt

# Define the ticker and datetime frame 
ticker = 'CL=F'
period1 = int(time.mktime(datetime.datetime(2001, 1, 1).timetuple()))
period2 = int(time.mktime(datetime.datetime(2023, 9, 2).timetuple()))
interval = '1d'  # 1mo for 1-month interval

# Construct the dynamic query string
query_string = f'https://query1.finance.yahoo.com/v7/finance/download/CL=F?period1=978307200&period2=1693612800&interval=1d&events=history&includeAdjustedClose=true'

# Read the CSV data and parse the 'Date' column as dates, setting it as the index
df = pd.read_csv(query_string, parse_dates=["Date"], index_col="Date")

# Calculate daily returns
df['Daily Returns'] = df['Close'].pct_change()

# Calculate daily volatility (standard deviation of daily returns)
df['Volatility'] = df['Daily Returns'].rolling(window=21).std()

# Create a volatility chart
plt.figure(figsize=(12, 6))
plt.plot(df.index, df['Volatility'], label='Daily Volatility', color='blue')
plt.xlabel('Date')
plt.ylabel('Volatility')
plt.title('Crude Oil Daily Volatility')
plt.legend()
plt.grid(True)

# Show the volatility chart
plt.show()



"""**Seasonal Pattern**"""
# Import libraries
import time
import datetime
import pandas as pd
import matplotlib.pyplot as plt

# Define the ticker and datetime frame 
ticker = 'CL=F'
period1 = int(time.mktime(datetime.datetime(2001, 1, 1).timetuple()))
period2 = int(time.mktime(datetime.datetime(2023, 9, 2).timetuple()))
interval = '1d'  # 1mo for 1-month interval

# Construct the dynamic query string
query_string = f'https://query1.finance.yahoo.com/v7/finance/download/CL=F?period1=978307200&period2=1693612800&interval=1d&events=history&includeAdjustedClose=true'

# Read the CSV data and parse the 'Date' column as dates, setting it as the index
df = pd.read_csv(query_string, parse_dates=["Date"], index_col="Date")

# Calculate daily returns
df['Daily Returns'] = df['Close'].pct_change()

# Calculate daily volatility (standard deviation of daily returns)
df['Volatility'] = df['Daily Returns'].rolling(window=21).std()

# Create a volatility chart
plt.figure(figsize=(12, 6))
plt.plot(df.index, df['Volatility'], label='Daily Volatility', color='blue')
plt.xlabel('Date')
plt.ylabel('Volatility')
plt.title('Crude Oil Daily Volatility')
plt.legend()
plt.grid(True)

# Show the volatility chart
plt.show()




"""**Correlation Analysis**"""
# Import libraries
import time
import datetime
import pandas as pd
import matplotlib.pyplot as plt

# Define the ticker and datetime frame 
ticker = 'CL=F'
period1 = int(time.mktime(datetime.datetime(2001, 1, 1).timetuple()))
period2 = int(time.mktime(datetime.datetime(2023, 9, 2).timetuple()))
interval = '1d'  # 1mo for 1-month interval

# Construct the dynamic query string
query_string = f'https://query1.finance.yahoo.com/v7/finance/download/CL=F?period1=978307200&period2=1693612800&interval=1d&events=history&includeAdjustedClose=true'

# Read the CSV data and parse the 'Date' column as dates, setting it as the index
df = pd.read_csv(query_string, parse_dates=["Date"], index_col="Date")

# Calculate daily returns
df['Daily Returns'] = df['Close'].pct_change()

# Calculate daily volatility (standard deviation of daily returns)
df['Volatility'] = df['Daily Returns'].rolling(window=21).std()

# Create a volatility chart
plt.figure(figsize=(12, 6))
plt.plot(df.index, df['Volatility'], label='Daily Volatility', color='blue')
plt.xlabel('Date')
plt.ylabel('Volatility')
plt.title('Crude Oil Daily Volatility')
plt.legend()
plt.grid(True)

# Show the volatility chart
plt.show()




"""**Event Analysis**

1. Russian invasion of Ukraine 24 February 2022
2. Southeastern front  28 August 2022
3. 2022 Ukrainian counteroffensives  11 November 2022
4. Second stalemate 12 November 2022 
5. 2023 Ukrainian counteroffensive 8 June 2023"""

# Import libraries
import time
import datetime
import pandas as pd
from tabulate import tabulate
import matplotlib.pyplot as plt
from matplotlib.dates import datestr2num

# Define the ticker and datetime frame 
ticker = 'CL=F'
period1 = int(time.mktime(datetime.datetime(2022, 1, 1).timetuple()))
period2 = int(time.mktime(datetime.datetime(2023, 9, 2).timetuple()))
interval = '1d'  # 1d for 1-day interval

# Construct the dynamic query string
query_string = f'https://query1.finance.yahoo.com/v7/finance/download/CL=F?period1=978307200&period2=1693612800&interval=1d&events=history&includeAdjustedClose=true'

# Read the CSV data and parse the 'Date' column as dates, setting it as the index
df = pd.read_csv(query_string, parse_dates=["Date"], index_col="Date")

# Remove the time portion from the date index column
df.index = df.index.date

# Save the data to a CSV file
df.to_csv(f'{ticker}_1d.csv')

# Define hypothetical events and their dates
events = {
    'Event 1': '2022-02-24',
    'Event 2': '2022-08-28',
    'Event 3': '2022-11-11',
    'Event 4': '2022-11-12',
    'Event 5': '2023-06-08',
}

# Convert event dates to datetime objects
event_dates = {event: datetime.datetime.strptime(date, '%Y-%m-%d') for event, date in events.items()}

# Function to plot events within a specified date range
def plot_events_within_range(df, events, start_date, end_date):
    # Convert start_date and end_date to datetime objects
    start_date = pd.to_datetime(start_date)
    end_date = pd.to_datetime(end_date)
    
    # Filter the DataFrame for the specified date range
    df_filtered = df[start_date:end_date]
    
    plt.figure(figsize=(12, 6))
    plt.plot(df_filtered.index, df_filtered['Close'], label='Close Price', color='blue')
    
    # Plot vertical lines for each event within the date range
    for event, date in events.items():
        event_date = pd.to_datetime(date)
        if start_date <= event_date <= end_date:
            plt.axvline(x=event_date, color='red', linestyle='--', label=event)
    
    plt.xlabel('Date')
    plt.ylabel('Close Price')
    plt.title('Crude Oil Price with Hypothetical Events (Jan 2022 - Sep 2023)')
    plt.legend()
    plt.grid(True)
    plt.show()

# Call the updated function to plot events within the specified date range
plot_events_within_range(df, event_dates, '2022-01-01', '2023-09-02')




"""**Forecasting**"""
# Import necessary libraries
import matplotlib.pyplot as plt
from datetime import datetime, timedelta
import pandas as pd
import statsmodels.api as sm
from statsmodels.tsa.arima.model import ARIMA

# Define a function for ARIMA forecasting
def arima_forecast(data, order, steps):
    model = ARIMA(data, order=order)
    model_fit = model.fit()
    forecast = model_fit.forecast(steps=steps)  # Forecast for the next 'steps' days
    return forecast

# Define the start and end dates for forecasting (2024-01-01 to 2025-12-31)
forecast_start_date = datetime(2023, 9, 3)
forecast_end_date = datetime(2025, 12, 31)

# Read the CSV data and parse the 'Date' column as dates, setting it as the index
df = pd.read_csv(query_string, parse_dates=["Date"], index_col="Date")

# Create Training and Test datasets for the first 1000 days and last 1000 days
train_first = df['Close'][:1000]
test_first = df['Close'][1000:2000]
train_last = df['Close'][-1000:]
test_last = df['Close'][-2000:-1000]

# Build Model for the first 1000 days
order = (3, 2, 2)  # Example order, you can tune this
forecasted_values_first = arima_forecast(train_first, order, steps=len(test_first))

# Build Model for the last 1000 days
forecasted_values_last = arima_forecast(train_last, order, steps=len(test_last))

# Create date ranges for the forecasted periods
forecast_dates_first = [forecast_start_date + timedelta(days=i) for i in range(len(test_first))]
forecast_dates_last = [forecast_start_date + timedelta(days=i) for i in range(len(test_last))]

# Filter the data and forecasts for the period from 2022-01-01 to 2025-12-31
start_date_filter = datetime(2022, 1, 1)
end_date_filter = datetime(2025, 12, 31)

filtered_df = df[(df.index >= start_date_filter) & (df.index <= end_date_filter)]
filtered_forecast_dates_first = [date for date in forecast_dates_first if date >= start_date_filter]
filtered_forecast_dates_last = [date for date in forecast_dates_last if date >= start_date_filter]

# Plot historical data and forecasted values for both periods within the filtered date range
plt.figure(figsize=(12, 6))
plt.plot(filtered_df.index, filtered_df['Close'], label='Historical Data')
plt.plot(filtered_forecast_dates_first, forecasted_values_first[:len(filtered_forecast_dates_first)], label='Forecasted Data (First 1000 Days)', linestyle='--')
plt.plot(filtered_forecast_dates_last, forecasted_values_last[:len(filtered_forecast_dates_last)], label='Forecasted Data (Last 1000 Days)', linestyle='--')
plt.xlabel('Date')
plt.ylabel('Close Price')
plt.title('Crude Oil Price Forecast (2022-2025)')
plt.legend()
plt.grid(True)
plt.show()




"""**Statistical Test** 

1. ADS Test (Augmented Dickey Fuller test) """

from statsmodels.tsa.stattools import adfuller

# Perform the ADF test
result = adfuller(df['Close'])

# Extract and print the ADF test results
adf_statistic, p_value, _, _, critical_values, _ = result
print(f'ADF Statistic: {adf_statistic}')
print(f'p-value: {p_value}')
print('Critical Values:')
for key, value in critical_values.items():
    print(f'   {key}: {value}')


import matplotlib.pyplot as plt

# Plot crude oil prices
plt.figure(figsize=(12, 6))
plt.plot(df.index, df['Close'], label='Crude Oil Prices')
plt.xlabel('Date')
plt.ylabel('Price')
plt.title('Crude Oil Prices Over Time')
plt.legend()
plt.grid(True)
plt.show()


# Create a DataFrame to store the test results
test_results = pd.DataFrame({
    'Test': ['ADF Statistic', 'p-value'],
    'Value': [adf_statistic, p_value]
})

# Display the test results table
print(test_results)





