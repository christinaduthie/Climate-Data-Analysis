# Climate-Data-Analysis

Source Code - [https://colab.research.google.com/drive/1h0oW6FKpjXdZsDhQf9bzfzXCGYb60vB0?usp=sharing](https://colab.research.google.com/drive/1h0oW6FKpjXdZsDhQf9bzfzXCGYb60vB0?usp=sharing)

## <strong> AIM </strong>

To perform data analysis on hourly climate data and visualize temperature and rainfall
patterns for 2 cities - Melbourne, FL and San Diego, CA.

## <strong> Description </strong>

Climate data analysis involves examining and interpreting various data sets related to climate, such as temperature, precipitation, wind speed, and humidity. The analysis of climate data helps scientists to better understand and predict weather patterns and the long-term effects of climate change.
There are powerful set of tools and methods to manage, process, and extract insights from climate data. The objective of this analysis to to gain insights from climate data of 2 cities.

## <strong>Analysis:</strong>

## <strong>Datasets</strong>

- Melbourne, FL - [https://drive.google.com/file/d/183dQq8VuRfXkbw6LsvuwlIIpHb71R41\_/view?usp=share_link](https://drive.google.com/file/d/183dQq8VuRfXkbw6LsvuwlIIpHb71R41_/view?usp=share_link)

- San Diego, CA -[https://drive.google.com/file/d/1e6d6NMe05Rqov7xZbSsU3Bj9-rcz0i97/view?usp=share_link](https://drive.google.com/file/d/1e6d6NMe05Rqov7xZbSsU3Bj9-rcz0i97/view?usp=share_link)

## <strong>Import necassary files</strong>

- CSV files containing the raw data to be analyzed.
- Python libraries such as Pandas or NumPy for data manipulation and analysis.
- Visualization libraries such as Matplotlib for creating plots and visualizations.

### <strong> [The following analysis is performed on both the datasets] </strong>

## <strong>Load the data</strong>

- Load the hourly climate data measurements into a pandas DataFrame using the `read_csv` function.

## <strong>Explore the data</strong>

- To get the shape of the dataframe
  </br>
  To get the shape of dataframe, `dataframe.shape` is used. It returns the number of rows and number of columns of the dataframe.
- Get the summary of dataframe
  </br>
  `dataframe.info()` prints information about a DataFrame including the index dtype and columns, non-null values, and memory usage.
- To get the description of the dataframe
  </br>
  The `pandas.describe()` function is used to get a descriptive statistics summary of a given dataframe. This includes mean, count, std deviation, percentiles, and min-max values of all the features.
- Detect missing values
  </br>
  The `dataframe.isnull()` method returns a DataFrame object where all the values are replaced with a Boolean value True for NULL values, and otherwise False.

## <strong> Data Cleaning </strong>

This involves tasks such as handling missing values, removing duplicates, correcting data types, and transforming data to make it consistent and usable for analysis.

- Input missing values using the mean.
  </br>
  The mean for the column with NaN is calculated and `fillna()` is used to fill the NaN values with the mean.

## <strong> Analysis of data </strong>

### Calculate the total number of hours that it rained using the precipitation values

`total_rainy_hours = (melbourne_data['precip_1hr'] > 0).sum()`

- It uses boolean indexing to create a new Series that is True where the precipitation is greater than zero, and False otherwise. Finally, it sums the number of True values to get the total number of hours that it rained.

</br>

### Calculate the total number of hours when temperature was greater than 100 and less than 32.

`num_hours_above_100 = (melbourne_data['temp'] > 100).sum()`
`num_hours_below_32 = (melbourne_data['temp'] < 32).sum()`

- This code creates a boolean series by comparing the temp column to 100, with True where the temperature is greater than 100 and False otherwise. It then applies the sum function to count the number of True values in the series, which corresponds to the number of rows where the temperature was greater than 100. The same is applied for finding temperature below 32.

</br>

### Find the average temperature for each month

`melbourne_data['month'] = melbourne_data['datetime'].dt.month`
`monthly_avg_temp = melbourne_data.groupby('month')['temp'].mean()`

- It then creates a new column 'month' by extracting the month component from each datetime value using the `dt.month` attribute. The data is then grouped by month using the `groupby()` function, and the mean of the temp column is calculated for each group using the `mean()` function.

</br>

### Calculate the sum of rainfall for each month

`monthly_sum_rainfall = melbourne_data.groupby('month')['precip_hrly'].sum()`
</br>

- The data is grouped by month using the groupby function, and the sum of the precip_hrly column is calculated for each group using the sum function.

</br>

### Find the total average temperature

`avg_temp = df['temp'].mean()`

- `mean()` method to calculate the average temperature from the 'temp' column.

</br>

### Calculate the humidity using values from the listed hourly temperature (T) and dew point (TD) values

Formula:
</br>
humidity=100∗exp(17.625∗TD)/243.04+TD / (exp(17.625∗T)/243.04+T)
</br>

`def calculate_humidity(temp, dew_point):`
</br>

`h = 100 _ np.exp(17.625 _ dew_point / (243.04 + dew_point)) / np.exp(17.625 \* temp / (243.04 + temp))`
</br>

`return h`
</br>

- This code defines a new function calculate_humidity that takes in temperature and dew point values and applies the provided formula to calculate the corresponding humidity value. The function uses the numpy exp function to compute the exponential values in the formula.

</br>

## <strong>Visualization </strong>

Matplotlib, a powerful data visualization library is used to create a wide range of charts and plots to better understand the data.

### Plot the temperature and humidity values as a line graph.

`ax.plot(melbourne_data['time'], melbourne_data['temp'], label='Temperature', color='orange')`
</br>

- It creates a plot with temperature values plotted as line graphs

`ax.plot(melbourne_data['time'], melbourne_data['H'], label='Humidity',color='pink')`

- It creates a plot with humidity values plotted as line graphs

</br>

### Mark the minimum and maximum temperature values on the graph as shaded dots.

`ax.plot(melbourne_data['time'].iloc[melbourne_data['temp'].idxmin()], melbourne_data['temp'].min(), 'ko', label='Minimum Temperature', color='blue')`
`ax.plot(melbourne_data['time'].iloc[melbourne_data['temp'].idxmax()], melbourne_data['temp'].max(), 'ko', label='Maximum Temperature',color='red')`

- It uses the idxmin() and idxmax() functions to get the indices of the minimum and maximum temperature values, and then plots a dot at the corresponding time on the x-axis.

</br>

### Mark horizontal lines for the average temperature of the year and the average humidity of the year

`ax.axhline(y=avg_temp, linestyle='--', color='blue', label='Average Temperature')`
`ax.axhline(y=avg_hum, linestyle='--', color='blue', label='Average Humidity')`

- The `axhline()` function is used to plot horizontal lines for the average temperature and humidity of the year.

</br>

### Generate 3 bar plots that help compares San Diego’s monthly temperature, humidity and rainfall totals with that of Melbourne’s monthly

The month from the 'time' column is extracted and grouped with the data using `.groupby('month')`. The monthly average temperature, humidity and hourse rained is calculated. A grouped bar plot is plotted for both cities with month names in x axis. The resulting plot shows the monthly average temperature,humidity and hours rained for both San Diego and Melbourne

## Conclusion

- According to the climate data for 2019, San Diego had a milder climate overall compared to Melbourne, with monthly average temperatures ranging from about 55°F in January to about 71°F in September. Melbourne, on the other hand, had monthly average temperatures ranging from about 62°F in January to about 83°F in August. Both cities experienced their warmest months during the summer, with San Diego reaching its highest average temperature in September and Melbourne in August.

- Based on the analysis, there were significant differences in the hourly rainfall patterns between Melbourne, FL and San Diego, CA. Melbourne experienced more frequent and higher intensity rainfall events throughout the year, while San Diego had fewer and less intense rainfall events. In Melbourne, the highest hourly rainfall events occurred during the summer months of July, August, September and December, which are also the months with the highest average rainfall. San Diego, on the other hand, experienced the highest hourly rainfall events during the winter months of Novermber, December, January, and February.
