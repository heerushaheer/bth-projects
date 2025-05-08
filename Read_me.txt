COVID-19 Simulation Analysis

	This project simulates and analyzes the COVID-19 pandemic's progression in different countries. The analysis includes visualizations and data summaries on infection status, including healthy, infected, immune, and deceased populations over time.

Project Structure
	The project includes the following files:

	a2-countries.csv: Contains country data used in the simulation.
	a2-covid-simulated-timeseries.csv: Contains the simulated COVID-19 timeseries data across various regions.
	a2-covid-summary-timeseries.csv: Summarized timeseries data representing different COVID-19 status categories.
	a2-covid-simulation.png: Visualization showing the COVID-19 infection status in Afghanistan, Sweden, and Australia over time.
	assignment2.py: The main script for running the COVID-19 simulation model.
	helper.py: Contains helper functions used by the main simulation script.
	sim_parameters.py: Contains parameters and configurations for the COVID-19 simulation.
	streamlit_ui.py: Streamlit-based UI for visualizing the simulation data interactively.
	sample.csv: A sample dataset that can be used for testing or initial setup.

Getting Started
	Prerequisites
		Python 3.7+
	Required Python libraries:
		pandas, matplotlib, streamlit, numpy

Usage
	Run Simulation: Use assignment2.py to perform the COVID-19 simulation and generate results. This script uses helper.py and sim_parameters.py for additional support.
	
	Data Visualization: The generated plot (e.g., a2-covid-simulation.png) visualizes the COVID-19 infection status in different countries (Afghanistan, Sweden, and Australia) over time, showing population distributions by health status.
	
	Interactive UI: Launch the Streamlit app using streamlit_ui.py to interactively visualize the COVID-19 timeseries data.
							streamlit run streamlit_ui.py

Data Files
	Simulation Timeseries: a2-covid-simulated-timeseries.csv contains raw simulation data showing infection progression by day and region.
	Summary Timeseries: a2-covid-summary-timeseries.csv is a summarized version of the timeseries data.

Visualizations
	The visualizations, like a2-covid-simulation.png, provide insight into the population distributions across health statuses:

Green: Healthy individuals
Orange: Infected (without symptoms)
Red: Infected (with symptoms)
Cyan: Immune
Gray: Deceased
These visualizations assist in analyzing infection trends and the effectiveness of interventions over time.