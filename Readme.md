## Analysis of the Public Bicycle System in Buenos Aires: Data Preparation for Power BI ##

This repository documents the **Extraction, Transformation**, and **Loading (ETL)** process applied to a public dataset containing trip records from the Buenos Aires public bike-sharing system. 
Link: https://data.buenosaires.gob.ar/dataset/bicicletas-publicas/resource/0fb65518-a9a7-42c1-8284-2dece001fe5d

The first stage of the project focused on preparing and structuring the data to seamlessly integrate it into an **interactive Power BI dashboard**. This involved data cleaning, creation of new variables, and an **initial exploratory analysis** to uncover patterns, trends, and potential anomalies.

The resulting dashboard was designed to enable proactive, **data-driven decision-making**, with the aim of optimizing operations, enhancing the user experience, and supporting the system’s sustainable growth. It provides a comprehensive view of key metrics, including:

**Total trip volume and temporal distribution:** such as the number of trips per day, stations used, and hourly distribution of journeys.

**Trip duration:** focusing on average trip duration.

**Contextual usage analysis:** breaking down trips by day type (business day, weekend, or holiday).

**Mobility patterns:** identifying the most frequent destinations per station and analyzing round trips (trips with the same origin and destination).

A custom report theme was designed for the dashboard, along with three interactive filters: **date, station**, and **day type**.

To correctly display the most frequent destinations table, two specific DAX measures were created to handle the logic when the user selects “All stations” in the filter, as the standard Top N filter would otherwise fail to work properly.

### Strategic and Operational Value of the Dashboard ###
The data and insights generated through this dashboard support:

- **Optimizing bicycle redistribution:** anticipating peak and off-peak demand to minimize station imbalances.

- **Planning preventive maintenance:** leveraging average trip duration and usage volume to schedule maintenance more effectively, extending the lifespan of bicycles and reducing breakdowns.

- **Strategic monitoring and network expansion:** using station usage and mobility patterns to identify where to add new stations or relocate existing ones to maximize coverage and service quality.

- **Informed growth planning:** supporting investment decisions and service adaptation based on clear growth trends and data-backed analysis.

### DAX Measures ####

EstacionesSeleccionadas = 
IF (
    COUNTROWS ( ALLSELECTED(recorridos_2024[nombre_estacion_origen_clean]) ) 
        = CALCULATE ( COUNTROWS ( ALL(recorridos_2024[nombre_estacion_origen_clean]) ) ),
    "Todas",
    "Parcial"
)

NumeroViajesTop10 = 
IF (
    [EstacionesSeleccionadas] = "Todas",
    CALCULATE ( COUNT(recorridos_2024[id_recorrido]) ),
    CALCULATE ( 
        COUNT(recorridos_2024[id_recorrido]),
        FILTER (
            ALL(recorridos_2024[nombre_estacion_origen_clean]),
            recorridos_2024[nombre_estacion_origen_clean] IN VALUES(recorridos_2024[nombre_estacion_origen_clean])
        )
    )
)
These measures ensure that the Top 10 most frequent trips are displayed correctly whether a specific station or all stations are selected.


**Local Setup Instructions:**
To ensure that the notebooks (`bicis.ipynb`) and the dashboard (`bikes.pbix`) function correctly, please:
1.  Download the `badata_ecobici_recorridos_realizados_2024.csv` file from the provided link.
2.  Place the downloaded file inside the `data/` folder at the root of your local clone of this repository.


The interactive Power BI dashboard (bikes.pbix) is not directly included in this repository due to its size.

You can download the .pbix file from the following link:
https://drive.google.com/file/d/1V797ttty-qw-ECb1uLdZ3eofcGXnLt8a/view?usp=sharing