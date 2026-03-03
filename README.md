Luxury Housing Sales Analysis – Bengaluru

Problem Statement:
Build a complete real estate analytics solution using Python for advanced data cleaning, load the refined dataset into a SQL database, and use Power BI to directly connect to SQL and build a dashboard. The goal is to replicate a real-world enterprise-level data pipeline and analysis environment using a complex housing dataset with 1,00,000+ records.
Approach:
 Step 1: Python — Data Cleaning & Feature Engineering

Load the raw .csv file

Normalize text fields (Builder)
df = df.rename(columns={'Developer_Name': 'Builder'})


Handle nulls values:
1.#distribution is slightly negatively skewed, but very close to 0.
df['Unit_Size_Sqft'] = df['Unit_Size_Sqft'].fillna(df['Unit_Size_Sqft'].mean())
2.#- The data is right-skewed,i used meadin
df['Ticket_Price_Cr'] = df['Ticket_Price_Cr'].fillna(df['Ticket_Price_Cr'].median())
3.# skewness is close to 0 , mean and median are almost the same.

df['Amenity_Score'] = df['Amenity_Score'].fillna(df['Amenity_Score'].mean())
4.#distribution is strongly positively skewed.

df['Price_per_Sqft'] = df['Price_per_Sqft'].fillna(df['Price_per_Sqft'].median())

Derive columns like Price_per_Sqft, Quarter_Number, Booking_Flag
1.df['Booking_Flag'] = df['Transaction_Type'].map({'Primary': 1, 'Secondary': 0})
2.df['Quarter_Number'] = df['Purchase_Quarter'].dt.quarter
3.df.loc[df['Unit_Size_Sqft'] > 0, 'Price_per_Sqft'] = (
    df['Ticket_Price_Cr'] / df['Unit_Size_Sqft']
)
change datatypes:
1.df['Ticket_Price_Cr'] = pd.to_numeric(df['Ticket_Price_Cr'], errors='coerce')
2.df['Purchase_Quarter'] = pd.to_datetime(df['Purchase_Quarter'])

drop the column buyer comment
df.drop('Buyer_Comments', axis=1, inplace=True)

Handle duplicates:
df.drop_duplicates(inplace=True)
Load Clean Data into PGADMIN
Power BI — Visualize via Direct SQL Connection
Business Insights Generated
<img width="1156" height="657" alt="image" src="https://github.com/user-attachments/assets/c740e1af-48d3-45e6-b2a8-7264e79d2fbe" />
<img width="1180" height="665" alt="image" src="https://github.com/user-attachments/assets/75ae9449-5692-4e1b-b333-d3cfb89fbf0a" />

![thanks](https://github.com/user-attachments/assets/7d8c7c91-76ca-4356-a542-0818f334caa4)


