# Carbon Emission Analysis

## 1. Report Introduction
<img width="700" alt="image" src="https://github.com/user-attachments/assets/150f29f5-a834-4f25-af1c-6ad337436909" />

Photo by Chris LeBoutillier (unplash.com)

This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.
Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.
Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.

## 2. Data Source: Where Our Data Comes From

Our dataset is compiled from publicly available data from nature.com and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent)

  ### 2.1 Data Structure
  
  The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/101a7040-9aff-4fc9-b339-2565c012ff0b" />

### 2.2 Tables columns description

**Table 'product_emissions'**

1.	id: Identifier for each product emission record.
2.	company_id: Identifier for the company associated with the product.
3.	country_id: Identifier for the country where the product is being produced.
4.	industry_group_id: Identifier for the industry group to which the product belongs.
5.	year: The year in which the emissions data was recorded.
6.	product_name: The name of the product associated with the emissions data.
7.	weight_kg: The weight of the product in kilograms.
8.	carbon_footprint_pcf: The carbon footprint of the product, measured in CO2 equivalent.
9.	upstream_percent_total_pcf: The percentage of the total carbon footprint attributed to upstream activities.
10.	operations_percent_total_pcf: The percentage of the total carbon footprint attributed to operations.
11.	downstream_percent_total_pcf: The percentage of the total carbon footprint attributed to downstream activities.
 
**Table 'industry_groups'**

1.	id: Unique identifier for each industry group.
2.	industry_group: The name of the industry group, categorizing businesses within similar sectors based on their products or services offered.
   
**Table 'companies'**
1.	id: Unique identifier for each company.
2.	company_name: The name of the company, identifying the specific organization within the dataset.
   
**Table 'countries'**
1.	id: Unique identifier for each country.
2.	country_name: The name of the country.

## 3. Research Questions

1.	Which products contribute the most to carbon emissions?
2.	What are the industry groups of these products?
3.	What are the industries with the highest contribution to carbon emissions?
4.	What are the companies with the highest contribution to carbon emissions?
5.	What are the countries with the highest contribution to carbon emissions?
6.	What is the trend of carbon footprints (PCFs) over the years?
7.	Which industry groups have demonstrated the most notable decrease in carbon footprints (PCFs) over time?

To answer the above questions, we will use SQL queries to extract the needed information based on the given tables listed in the Data structure

Combine 4 tables into 1, so that we will get **a master table** which includes all information we need

**SQL query:**

        SELECT *
        FROM product_emissions as pro
        JOIN industry_groups as ind ON pro.industry_group_id = ind.id
        JOIN companies as com ON pro.company_id = com.id
        JOIN countries as cou ON pro.country_id = cou.id;

**SQL query explanation**: We use JOIN to combine 4 tables

### 3.1 Which products contribute the most to carbon emissions?

We calculate the carbon emissions of each product based on the carbon_footprint_pcf

**SQL query explanation:** We SELECT the product name and average carbon footprint, then GROUP BY product name and arrange the result DESCENDING on average carbon footprint

        SELECT product_name, ROUND(AVG(carbon_footprint_pcf),2)  as Average_carbon_footprint
        FROM product_emissions as pro
        JOIN industry_groups as ind ON pro.industry_group_id = ind.id
        JOIN companies as com ON pro.company_id = com.id
        JOIN countries as cou ON pro.country_id = cou.id
        GROUP BY product_name
        ORDER BY Average_carbon_footprint DESC
        LIMIT 10;

**Results:**

product_name | Average_carbon_footprint
------------- | -------------
Wind Turbine G128 5 Megawats |	3718044.00
Wind Turbine G132 5 Megawats |	3276187.00
Wind Turbine G114 2 Megawats |	1532608.00
Wind Turbine G90 2 Megawats |	1251625.00
Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit. |	191687.00
Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall |	167000.00
TCDE |	99075.00
Mercedes-Benz GLE (GLE 500 4MATIC) |	91000.00
Mercedes-Benz S-Class (S 500) |	85000.00
Mercedes-Benz SL (SL 350) |	72000.00

The results indicate that, in terms of product emissions, wind turbines contribute the highest carbon output. Ironically, a solution designed for green energy production results in the most significant carbon emissions.

### 3.2 What are the industry groups of above products?

To find the industry groups of above products, we add column ‘industry_group’ into the SQL query

**SQL query explanation:** We SELECT the product name, average carbon footprint, industry_group then GROUP BY product name and arrage the result DESCENDING on average carbon footprint

        SELECT product_name, ROUND(AVG(carbon_footprint_pcf),2)  as Average_carbon_footprint, industry_group
        FROM product_emissions as pro
        JOIN industry_groups as ind ON pro.industry_group_id = ind.id
        JOIN companies as com ON pro.company_id = com.id
        JOIN countries as cou ON pro.country_id = cou.id
        GROUP BY product_name
        ORDER BY Average_carbon_footprint DESC
        LIMIT 10;

**Result:**

product_name |	Average_carbon_footprint |	industry_group
------------- | ------------- | -------------
Wind Turbine G128 5 Megawats |	3718044.00 |	Electrical Equipment and Machinery
Wind Turbine G132 5 Megawats |	3276187.00 |	Electrical Equipment and Machinery
Wind Turbine G114 2 Megawats |	1532608.00 | 	Electrical Equipment and Machinery
Wind Turbine G90 2 Megawats |	1251625.00 |	Electrical Equipment and Machinery
Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit. |	191687.00 |	Automobiles & Components
Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall |	167000.00 |	Materials
TCDE |	99075.00 |	Materials
Mercedes-Benz GLE (GLE 500 4MATIC) |	91000.00 |	Automobiles & Components
Mercedes-Benz S-Class (S 500) |	85000.00 |	Automobiles & Components
Mercedes-Benz SL (SL 350) |	72000.00 |	Automobiles & Components

The products with the highest levels of carbon emissions are typically associated with heavy industry.

### 3.4 What are the companies with the highest contribution to carbon emissions?

We calculate the carbon emissions of each company based on the carbon_footprint_pcf

**SQL query explanation:** We SELECT the company_name and average carbon footprint, then GROUP BY company_name and arrage the result DESCENDING on average carbon footprint

        SELECT company_name, ROUND(AVG(carbon_footprint_pcf),2)  as Average_carbon_footprint
        FROM product_emissions as pro
        JOIN industry_groups as ind ON pro.industry_group_id = ind.id
        JOIN companies as com ON pro.company_id = com.id
        JOIN countries as cou ON pro.country_id = cou.id
        GROUP BY company_name
        ORDER BY Average_carbon_footprint DESC
        LIMIT 10;

**Result:**

company_name |	Average_carbon_footprint
------------- | -------------
"Gamesa Corporación Tecnológica, S.A." |	2444616.00
"Hino Motors, Ltd." |	191687.00
Arcelor Mittal |	83503.50
Weg S/A |	53551.67
Daimler AG |	43089.19
General Motors Company |	34251.75
Volkswagen AG |	26238.40
Waters Corporation |	24162.00
"Daikin Industries, Ltd." |	17600.00
CJ Cheiljedang |	15802.83

Most of the companies with the highest carbon emissions are typically linked to heavy industries. Leading the list is Gamesa Corporación, a manufacturer of wind turbines, with a carbon footprint of 2,444,616. Top 1 is Gamesa Corporación which produce the wind turbine, contribute 2.444.616 carbon footprint. It is truly paradoxical that a green energy company emits the largest amount of carbon emissions into the earth’s atmosphere.

### 3.5 What are the countries with the highest contribution to carbon emissions?

We calculate the carbon emissions of each country based on the carbon_footprint_pcf

**SQL query explanation:** We SELECT the country_name and average carbon footprint, then GROUP BY country_name and arrange the result DESCENDING on average carbon footprint

        SELECT country_name, ROUND(AVG(carbon_footprint_pcf),2)  as Average_carbon_footprint
        FROM product_emissions as pro
        JOIN industry_groups as ind ON pro.industry_group_id = ind.id
        JOIN companies as com ON pro.company_id = com.id
        JOIN countries as cou ON pro.country_id = cou.id
        GROUP BY country_name
        ORDER BY Average_carbon_footprint DESC
        LIMIT 10;

**Result:**

country_name |	Average_carbon_footprint
------------- | -------------
Spain |	699009.29
Luxembourg |	83503.50
Germany |	33600.37
Brazil |	9407.61
South Korea |	5665.61
Japan |	4600.26
Netherlands |	2011.91
India |	1535.88
USA |	1332.60
South Africa |	1119.27

Spain is the largest contributor to carbon emissions in the air. Notably, four of the top ten countries with the highest emissions are from Europe.

### 3.6 What is the trend of carbon footprints (PCFs) over the years?

To calculate the trend, we have to get 2 information: year and average carbon footprint each year

**SQL query explanation:** We SELECT year and average carbon footprint, then GROUP BY year

        SELECT year, ROUND(AVG(carbon_footprint_pcf),2)  as Average_carbon_footprint
        FROM product_emissions as pro
        JOIN industry_groups as ind ON pro.industry_group_id = ind.id
        JOIN companies as com ON pro.company_id = com.id
        JOIN countries as cou ON pro.country_id = cou.id
        GROUP BY year
        LIMIT 10;

**Result:**

year |	Average_carbon_footprint
------------- | -------------
2013 |	2399.32
2014 |	2457.58
2015 |	43188.90
2016 |	6891.52
2017 |	4050.85

<img width="700" alt="image" src="https://github.com/user-attachments/assets/933ae6b4-443f-4416-a0ad-dd99a0bb0de0" />

Overall, carbon emissions showed a slight increase from 2013 to 2017. A notable observation is the sharp rise in 2014, from 2,457 to 43,188, followed by an equally steep decline in 2015, dropping from 43,188 to 6,891

### 3.7 Which industry groups have demonstrated the most notable decrease in carbon footprints (PCFs) over time?

We copy the data from the master table and paste it into the Excel file so that we can draw the chart more easily

**Result:**

<img width="700" alt="image" src="https://github.com/user-attachments/assets/80ff295a-8e4d-40f6-9298-55a26f364e9a" />

The results indicate that the Automobiles and Components industry achieved the most significant reduction in carbon emissions between 2013 and 2017, with a decrease of 26,038.

## Summary

In term of the highest carbon emissions:
Category | Name | Carbon emissions
------------- | ------------- | -------------
Product	| Wind turbine |	3718044
Industry group |	Electrical Equipment and Machinery	|	891050.73
Company	|	Gamesa Corporación Tecnológica, S.A.	|	2444616
Country	|	Spain	|	699009.29

The global trend of carbon emissions has been declining due to the implementation of regulations aimed at protecting the planet.
