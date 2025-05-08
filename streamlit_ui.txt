import streamlit as st
import pandas as pd
import os
import assignment2  # Ensure assignment2.py is in the same directory as this file

# Load the countries data from the CSV file
countries_df = pd.read_csv('a2-countries.csv')

# Streamlit inputs
st.title("COVID-19 Simulation Tool")

# Numeric input for sample ratio
SAMPLE_RATIO = st.number_input("Sample Ratio", value=1e6, format="%.0f")

# Date input for start date
START_DATE = st.date_input("Start Date", value=pd.to_datetime('2021-04-01'))

# Date input for end date
END_DATE = st.date_input("End Date", value=pd.to_datetime('2022-04-30'))

# Dropdown menu for countries
SELECTED_COUNTRIES = st.multiselect(
    "Select Countries",
    options=countries_df['country'].unique(),
    default=None
)

# Run button
if st.button("Run"):
    # Run the code using the selected parameters
    assignment2.run(
        countries=SELECTED_COUNTRIES,
        sample_ratio=SAMPLE_RATIO,
        start_date=START_DATE,
        end_date=END_DATE
    )
    
    # Check if the image file exists and display it
    if os.path.exists('a2-covid-simulation.png'):
        st.image('a2-covid-simulation.png')
    else:
        st.write("The image file 'a2-covid-simulation.png' was not found.")
