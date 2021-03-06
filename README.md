## Project Title - COVID-19 Research ETL

### Project Description
This project's goal is to apply the acquired ETL (extract, transform, load) learnings in class to a topic of our choice.<br/>
Our group has decided to focus this project on the major topic of the moment - Corona Virus / Covid-19.

### Project Team -Pandas in Pandemic
- Kendall Marquard
- Smiti Swain
- Luis Ramirez
- Salvador Neves
- Tejas Naik

### ETL:

![ETL.PNG](Images/ETL.PNG)
<br/>
![ERD.PNG](Images/ERD.png)

<ins>**Country Table**</ins><br/>
#### Data Extraction:
The data source is a json file from https://worldpopulationreview.com/

![Country_source.PNG](Images/Country_source.PNG)

1) Json library was used to load the file.
2) We looped through the json to append the values to arrays previously created. 
3) From there we created a Data Frame.

#### Data Transformation:
1) Selected only the necessary fields from json file.
2) Renamed fields while storing in Data Frame
3) Set Country ID as index.

![Country_transformation.PNG](Images/Country_transformation.PNG)


<ins>**State Table**</ins><br/>
#### Data Extraction:
The data source is wikipedia. https://en.wikipedia.org/wiki/List_of_states_and_territories_of_the_United_States_by_population

![State_source.PNG](Images/State_source.PNG)

1) Pandas scraping (pandas.read_html) was used.
2) The table information sourced directly from the website was stored in a Data Frame.


#### Data Transformation:
1) The table scraped from Wikipedia had two levels of column headers so we dropped the top one.
2) Dropped columns that we don't need in the data frame.
3) Renamed columns to friendly names.
4) Used a dictionary with state abreviations and names, to map state name andget state id to be populated in the main data frame.

![State_transformation1.PNG](Images/State_transformation1.PNG)

![State_transformation2.PNG](Images/State_transformation2.PNG)

<ins>**Country_Cases Table**</ins><br/>
#### Data Extraction:
The data source is a CSV file.

1) Using Padas read_csv,data was imported from the CSV. This data source has data from 3/13/2020 to 4/24/2020.  
2) A data frame was created.


#### Data Transformation:

1) selected only the desired columns from the data set and stored it in data frame.
2) Droped unwanted columns and renamed columns to friendly names.
3) Used the pycountry library to create a Country_id column based on the iso_code. This will allow future users to join with the Country table when querying the DB.

![Country_Cases_transformation2.PNG](Images/country_cases_transformation.PNG)

<ins>**US_States_Cases Table**</ins><br/>
#### Data Extraction:
The data source is an API end point https://covidtracking.com/api/v1/states/daily.json

![US_States_cases_source.PNG](Images/US_States_cases_source.PNG)

1) Using Requests module, response was received from the API endpoint.
2) Using JSON traversal, desired datapoints were collected and stored in lists.
3) The data retrived was then stored in a Data Frame.


#### Data Transformation:
1) For the US_States_Cases Table the json data returned from API was not consistent . For example , if for a day there are no positive cases , the json data did not have the key altogether. In order to fix this issue, try - except code bloack was used, when the key is not found, store the value as "0".
2) Converted the date returned by the API from string to Date format.

![US_States_cases_Transformation.PNG](Images/US_States_cases_Transformation.PNG)
![US_States_cases_DF.PNG](Images/US_States_cases_DF.PNG)


<ins>**Index_prices Table**</ins><br/>
#### Data Extraction:
The data source is "yfinance" module, which is a python library that scrapes data from Yahoo! Finance and returns the data in a DataFrame format to get free stock market data.

1) We needed to pip install yfinance.
2) We created a dictionary to hold the ticker and country code for major world indices.
3) Using a For loop , and the yfinance module, data frame was created for each of the ticker.

![Ticker_source.PNG](Images/Ticker_source.PNG)


#### Data Transformation:
1) Data Frame returned from yfinance module did not have the Index Ticker, name and County. Using a dictionary and a for loop to iterate through it , new columns were added to the main Data Frames. 
2) Reset Index on the Data Frame, that previously had Date as the index.

![Ticker_transformation.PNG](Images/Ticker_transformation.PNG)


<ins>**Hospital Beds**</ins><br/>
#### Data Extraction:

1)	Pandas read_CSV was used to read csv file form Kaggle data set.
2)	CSV was downloaded directly to resources folder and used. 


#### Data Transformation:
1)	After loading CSV to data frame, reduced data frame for data of interest.
2)	Data frame was further filtered for US states to keep data limited to US.
3)	Used aggregation/group by function and to get total bed per 1000 population by states.

![Hospital_transformation.PNG](Images/Hospital_transformation.PNG)


<ins>**Gas_Price table**</ins><br/>
### Data Extraction: 
The data source is https://www.eia.gov/petroleum/data.php which is U.S. Energy Information Administration. 

1) This website provided us with several csv files.
2) Pandas read_CSV was used to read csv file and created data frame.

#### Data Transformation:
1) Merged the data sets oil production and stock by date.
2) Isolated the coloumns that we are measuring and saving them to a new variable.
3) Renaming the old titles into clear names.
4) Imported a second csv file for retail price of oil, filter the table to only show the states, rename the titles and save them to a new data frame.
5) The data presented had columns for each state and the retail gas prices, where as in the final sql table we needed those as a row.
6) In order to handle this , we created an array of states, looped through the array to create data frame for the specific state and write that to the sql table, by appending data each time.

![gas_price_transformation2.PNG](Images/gas_price_transformation2.PNG)
![gas_price_transformation3.PNG](Images/gas_price_transformation3.PNG)

<ins>**US_Unemployment_Stats Table**</ins><br/>
### Data Extraction: 
The data source we used was https://www.bls.gov/web/laus/lauhsthl.htm , which came from the U.S. Bureau of Labor Statistics. 

1) Pandas scraping (pandas.read_html) was used.
2) The table information sourced from the website was stored in a Data Frame.

#### Data Transformation:
1) Import the HTML file on Jupyter notebook and print the table to understand how the data is formatted. 
2) Isolate and identify the columns that we are measuring and create a new dataframe with this data.
3) Dropped unwanted rows.
4) Renamed the old titles into names that are cohesive with other data sets.
5) Mapped the State name with a dictionary of state name and abbreviation , to get the abbreviation and store it in a new State ID column.


![Unemployment_Trans_1.PNG](Images/Unemployment_Trans_1.PNG)
![Unemployment_Trans_2.PNG](Images/Unemployment_Trans_2.PNG)

### Data Load:

1) Created tables in postgres database.
2) Using sqlalchemy create-engine , connection was set up with the postgres database.
3) Using pandas to_sql(), data was inserted into the tables.



### Data Retrieval
In order to conclude the project, we thought would be interesting to perform some sql queries on our Covid-19 DB to confirm the data was loaded properly and tables can be join in order to perform aggregated queries.

<ins>**US States Cases and Hospital Beds (per K population)**</ins>
![State_Corona_Cases.PNG](Images/Query_states_cases_hospital.PNG)
<br/>

<ins>**US States Cases and Unemployment rate for march 2020**</ins>
![State_Corona_Cases_vs_Unemployment.PNG](Images/Query_states_cases_unemployment.PNG)
<br/>

<ins>**Weekly Gas prices**</ins>
![Weekly_Gas_Prices_per_State.PNG](Images/Query_states_Gas_price.PNG)
<br/>

<ins>**Country Cases Vs Index price change**</ins>
![Country_Cases_vs_Index_Prices.PNG](Images/Query_Country_cases_index.PNG)
