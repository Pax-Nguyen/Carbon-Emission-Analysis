# Carbon Emission Analysis

## 1. Report Introduction
<img width="700" alt="image" src="https://github.com/user-attachments/assets/150f29f5-a834-4f25-af1c-6ad337436909" />

Photo by Chris LeBoutillier (unplash.com)

This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.
Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.
Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.

## 2. Data Source: Where Our Data Comes From

Our dataset is compiled from publicly available data from nature.com and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent)

## 3. Data Structure

The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.

<img width="700" alt="image" src="https://github.com/user-attachments/assets/101a7040-9aff-4fc9-b339-2565c012ff0b" />

## 4. Tables columns description

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





