# Seoul-Bike-Sharing-Demand

## 1. Problem Statement

Many metropolitan cities now provide bike rental services to enhance urban mobility and convenience. Ensuring that rental bikes are available when needed is essential for minimizing wait times, making the steady supply of bikes a key priority. In this context, the predicted hourly demand plays a vital role.

Bike-sharing systems streamline the process of membership, rentals, and returns through a network of automated stations. Users can pick up a bike from one location and return it either to the same spot or a different one. Rentals are facilitated through membership or on-demand access, all managed by a citywide automated system.

This dataset is designed to forecast the demand for Seoul’s Bike Sharing Program by analyzing historical usage trends alongside factors such as temperature, time, and other variables.

---

## 2. Hypothesis Generation for the Problem Statement

### On the basis of Weather Conditions
- Does temperature have a positive impact on bike rentals (higher demand in pleasant weather)?
- Are bike rentals lower when humidity is very high?
- Are rentals less frequent during rainfall or snowfall?
- Do windy conditions discourage people from renting bikes?
- Is there a relationship between visibility and bike demand (low visibility → fewer rentals)?
- Do solar radiation levels (sunny vs cloudy days) influence rentals?

### On the basis of Time (Temporal Factors)
- Are rentals higher during morning and evening peak hours (commute times)?
- Do people rent more bikes on weekends compared to weekdays?
- Are rentals lower on holidays compared to working days?
- Do rentals vary significantly across seasons (e.g., highest in summer, lowest in winter)?
- Is there a difference in late night vs daytime rental behavior?

---

## 3. Dataset Description

The dataset records the hourly count of public bicycles rented in the Seoul Bike Sharing System, along with corresponding weather conditions and holiday information. In many modern cities, rental bikes have been introduced to improve mobility and convenience. Ensuring that bikes are available and accessible at the right time is essential to reduce waiting times, making a consistent and reliable supply a key concern. Predicting the number of bikes required each hour is therefore critical for maintaining this stability.

The dataset includes detailed weather attributes such as temperature, humidity, wind speed, visibility, dew point, solar radiation, snowfall, and rainfall, along with rental counts per hour and date-related information. In total, the dataset consists of **14 columns and 8,760 rows**.

---

## 4. Understanding Variables

The dataset contains weather information (Temperature, Humidity, Windspeed, Visibility, Dewpoint, Solar radiation, Snowfall, Rainfall), the number of bikes rented per hour, and date information.

- **Date** : year-month-day  
- **Rented Bike count** : Count of bikes rented at each hour  
- **Hour** : Hour of the day  
- **Temperature** : Temperature in Celsius  
- **Humidity** : %  
- **Windspeed** : m/s  
- **Visibility** : 10m  
- **Dew point temperature** : Celsius  
- **Solar radiation** : MJ/m2  
- **Rainfall** : mm  
- **Snowfall** : cm  
- **Seasons** : Winter, Spring, Summer, Autumn  
- **Holiday** : Holiday/No holiday  
- **Functional Day** : NoFunc (Non Functional Hours), Fun (Functional hours)  
