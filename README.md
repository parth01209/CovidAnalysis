
# CovidAnalysis
SQL Data Exploration  

# Personal Learning Project - Covid 19 Data Exploration

This is a personal learning project that involves exploring Covid-19 data using SQL queries. The project focuses on various aspects of the Covid-19 pandemic, including cases, deaths, population statistics, vaccination rates, and more. The SQL queries used in this project demonstrate skills such as Joins, CTE's, Temp Tables, Windows Functions, Aggregate Functions, Creating Views, and Converting Data Types.

## Table of Contents

- [Introduction](#personal-learning-project---covid-19-data-exploration)
- [How to Use](#how-to-use)
- [SQL Queries](#sql-queries)
- [Data Exploration](#data-exploration)
- [Creating Views](#creating-views)
- [License](#license)

### How to Use

1. Clone the repository to your local machine:

```bash
git clone https://github.com/your-username/your-repo.git
```
Set up your database environment with the required Covid-19 dataset.

Execute the SQL queries in your database environment to explore the Covid-19 data.


### SQL Queries
The project includes several SQL queries that provide insights into the Covid-19 data. Here's a brief explanation of each query:

       Select * From PortfolioProject..CovidDeaths...:

Description: Selects all data from the CovidDeaths table, filtering out records with a non-null continent value, and orders the results.
Use case: Basic data exploration.
Select Location, date, total_cases, new_cases, total_deaths, population...:

Description: Selects specific data columns from the CovidDeaths table, filtering out records with a non-null continent value, and orders the results.
Use case: Initial data exploration focusing on location, date, cases, deaths, and population.

      Select Location, date, total_cases, total_deaths, (total_deaths/total_cases)*100 as DeathPercentage
      From PortfolioProject..CovidDeaths
      Where location like '%states%'
      and continent is not null
      order by 1, 2


Total Cases vs Total Deaths:
Description: This query selects data from the CovidDeaths table for locations containing the word 'states' in their name, along with the total_cases, total_deaths, and calculates the DeathPercentage as (total_deaths/total_cases)*100. It shows the likelihood of dying if you contract Covid-19 in your country.
Use case: Analyzing the relationship between total cases and total deaths in locations with 'states' in their name.


      Select Location, date, Population, total_cases, (total_cases/population)*100 as PercentPopulationInfected
      From PortfolioProject..CovidDeaths
      --Where location like '%states%'
      order by 1, 2

Total Cases vs Population:
Description: This query selects data from the CovidDeaths table, calculating the percentage of the population infected with Covid-19 in different locations. It uses the formula (total_cases/population)*100 to calculate the PercentPopulationInfected.
Use case: Examining the impact of Covid-19 on different locations relative to their population size.



## Data Exploration
The SQL queries in this project cover various aspects of Covid-19 data exploration, such as:

Likelihood of dying if you contract Covid-19 in your country.
Percentage of the population infected with Covid-19 in different locations.
Countries with the highest infection rate compared to population.
Countries with the highest death count per population.
Breakdown by continent, showing continents with the highest death count per population.
Global Covid-19 numbers, including total cases, total deaths, and death percentage.


## Creating Views
The project includes the creation of a view named PercentPopulationVaccinated. This view stores data related to Covid-19 vaccinations, including location, date, population, new vaccinations, and a rolling count of vaccinated people.

The view is created with the following query:

      Create View PercentPopulationVaccinated as
      Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
      , SUM(CONVERT(int,vac.new_vaccinations)) OVER (Partition by dea.Location Order by dea.location, dea.Date) as RollingPeopleVaccinated
      --, (RollingPeopleVaccinated/population)*100
      From PortfolioProject..CovidDeaths dea
      Join PortfolioProject..CovidVaccinations vac
      	On dea.location = vac.location
      	and dea.date = vac.date
      where dea.continent is not null








