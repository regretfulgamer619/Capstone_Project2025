# Capstone Project Summer Analytics 2025
  Dynamic Pricing for Urban Parking Lots

  Urban parking spaces are a limited and highly demanded resource. Prices that remain static
  throughout the day can lead to inefficiencies — either overcrowding or underutilization.
  To improve utilization, dynamic pricing based on demand, competition, and real-time
  conditions is crucial.
  
  This project tackles the problems of static pricing using an intelligent, data-driven
  pricing engine for 14 parking spaces using real-time data streams, basic economic theory,
  and ML models built from scratch using NumPy and Pandas libraries.
  
  This project solves the problem by building a dynamic parking pricing engine using real-time inputs like-
      
  1) Location (Longitudes and Latitudes)
  2) Occupancy
  3) Traffic
  4) Queue Lengths
  5) Vehicle Types
  6) Whether it is a special day or not


# TECHSTACKS USED FOR THE PROJECT - 
  1) Python 3.11.13 
  2) Pathway
  3) Bokeh
  4) Panel
  5) Google Colab
  6) Pandas

  7) NumPy
# ARCHITECTURE FLOW
![Architectural Flow](https://github.com/user-attachments/assets/0936feea-4ff8-426a-b2aa-fab4594e1083)


# WORK FLOW
1) Firstly we extraxt the data from the provided CSV file. We find that the original dataset contains columns like- 'ID', 'SystemCodeNumber', 'Capacity', 'Latitude', 'Longitude','Occupancy', 'VehicleType', 'TrafficConditionNearby', 'QueueLength', 'IsSpecialDay', 'LastUpdatedDate', 'LastUpdatedTime'. We need to process this data according to our needs next.

2) We create another CSV file that has the **'Timestamp'** column and has two columns **'Vehicle_Weight'** and **'Traffic_Weight'** that have allocated the different weights to different vehicles and different traffic conditions. We then move on to real time data streaming with **Pathway**. We use the following command -

    **data_2 = pw.demo.replay_csv("parking_stream.csv", schema=ParkingSchema, input_rate=1000)**
   
   This feeds each row as a time-stamped event into the Pathway streaming engine, enabling real-time processing and pricing decisions as if data is coming from parking sensors.


3) We then proceed towards the calculation of demand and the final price considering different factors. We use 2 distinct models for price computation.

   1) **Linear Model** -

       price= 10 + (pw.this.occ_max - pw.this.occ_min) / pw.this.cap

      This model responds only to current parking load and encourages early filling. However it doesn't account for demand volatility.


   2) **Demand Based Model** -

      price_model2 = 10 + 4.5* (
        (
            1.5 * (pw.this.occ_sum / pw.this.count / pw.this.cap) +
            1.5 * (pw.this.queue_sum / pw.this.count) -
            3.0 * (pw.this.traffic_sum / pw.this.count) +
            1.5 * pw.this.special_day +
            2.0 * (pw.this.vehicle_sum / pw.this.count)
            - pw.this.demand_min
        ) / (pw.this.demand_max - pw.this.demand_min + 1e-6)
      )

      This model allows pricing to scale dynamically depending on real-world conditions (e.g., high traffic, longer queues, VIP events). The price is normalised so that uniformity is maintained throughout.


4) We then move to Visualization and Plotting using **Bokeh**(for time series plot) and **Panel**(to host the interactive dashboard) for observing the price variations for the two models. The final prices that are calculated using both the models are sent to a live dashboard build.

# Summary

   Dynamic Parking Pricing System is a real-time pricing engine designed to optimize parking space utilization in urban environments. Using a stream processing framework **(Pathway)**, the system simulates real-time events from parking lot data to dynamically compute parking prices.

The project focuses on two pricing models:

**Model 1**: A static pricing approach that increases the price linearly with occupancy, encouraging early parking and preventing overcrowding.

**Model 2**: A more adaptive model that computes a demand score based on multiple real-world factors like occupancy, queue length, traffic conditions, special event days, and vehicle type. This score is normalized daily and used to set dynamic, context-aware prices.

The data is visualized in real time using **Bokeh** and **Panel**, enabling operators or users to monitor how prices fluctuate based on live conditions. This project demonstrates the application of real-time data engineering, feature engineering, and pricing strategy in the context of smart city parking management.
   

   




      





  



    

    


