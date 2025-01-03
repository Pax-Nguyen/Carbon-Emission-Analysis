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

To answer the above questions, we will use SQL queries to extract the needed information base on the given tables listed in Data structure

Combine 4 tables into 1, so that we will get a master table which inlucdes all information we need

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
------------- | -------------
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





