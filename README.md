# Estimation of room occupancy based on temperature, CO2 concentration, electricity consumption and other inputs

## General objective

The objective is to develop a tool in Java that is able to estimate, based on the "office.csv" file, the occupancy of the following office:

![image](https://user-images.githubusercontent.com/49452402/139596994-101fa9d1-5486-4ad3-b401-18aae12d03d6.png)

## Chosen approach

Given the data provided in the .csv file, it is possible to estimate the occupation of the office in 3 different ways:

### 1. Laptop power consumption
The consumption of each one of the 4 laptops is measured by a power meter (variable ‘power_laptopX_zoneY’). If the average consumption is greater than the standby consumption (15W), it is considered that someone is working on the computer
      
### 2. Motion detections
It is considered that the number of motions detected within an hour is proportional to the number of occupants in the office. In order to determine the best proportional coefficient, a simple dichotomy algorithm (considering the laptop consumption based estimation as the actual occupancy) was implemented in the "DichotomicScaler" class.

<p>Dichotomy algorithm:</p>

![image](https://user-images.githubusercontent.com/49452402/139597896-ea42ce53-ac92-48b7-97c2-e9db23d03f0c.png) 

### 3. CO2 concentration
In order to estimate the occupancy based on the CO2 level, it is necessary to study the air mass balance. The air flow exchanged from indoor and outdoor can be decomposed into:
       
       • Qout = Qout(0) + ζwindow*Qwindow_out
       Where:
       • Qout --> Air flow exchanged from indoor and outdoor.
       • Qout(0) --> Air infiltration
       • ζwindow*Qwindow_out --> Average constant renewal air flow going through the window when it is opened
       • ζwindow --> It is worth 0 when it is always closed and 1 when it is opened

In the same way, the renewable of air flow through the door is given by:

       • Qcorridor = Qdoor(0) + ζdoor*Qdoor_corridor
       Where:
       • Qcorridor --> Air flow exchanged from indoor and outdoor.
       • Qcorridor(0) --> Air infiltration
       • ζdoor*Qdoor_corridor --> Average constant renewal of air flow going through the corridor when the door is opened
       • ζdoor --> It is worth 0 when it is always closed and 1 when it is opened

The air mass balance leads to:

       • V dΓoffice/dt = −(Qout + Qcorridor)Γoffice + QoutΓout + QcorridorΓcorridor +Sbreath*n
       Where:
       • Γ, the CO2 concentration and Qout ≈ 395ppm, the outdoor average CO2 concentration
       • V , the room volume i.e. 55m3 for the office
       • Sbreath, the average CO2 production per occupant. It is estimated to be 4ppm.m3/s
       • n, the number of occupants in the office

<p>Considering average values over a time period of Ts = 3600 seconds, the differential equation can be solved to get a recurrent equation (Xk means average value of X during period [kTs, (k +1)Ts]:</p> 

![image](https://user-images.githubusercontent.com/49452402/139601293-f9030b13-5367-4cf1-8b6d-2b6353bd8edc.png)

<p>Where:</p> 

![image](https://user-images.githubusercontent.com/49452402/139601301-93264c0b-a555-46f6-ad87-04966f63e21d.png)

<p>The estimate comes directly from the equation. It yields:</p>

![image](https://user-images.githubusercontent.com/49452402/139601354-fc2579a3-b296-4ade-8154-a16a6803a138.png)

In order to produce numerical answers, I used the following values for the variables:

       • Qout(0) = 25/3600 m3/s
       • Qcorridor(0) = 25/3600 m3/s
       • Qwindow_out = 150/3600 m3/s
       • Qdoor_corridor = 150/3600 m3/s
 
## Architecture used (UML diagram)

![image](https://user-images.githubusercontent.com/49452402/139602362-38e0f680-5335-420b-ac93-03f0feef9acd.png)


## Results

The use of this tool is simple and accessible to all. The user has to choose the variables he wants to analyze and then he just has to click on the "plot" button to receive the graphic representation of the selected variables.

An example of how to use the application is given below, where we can see the difference between the estimates made from the consumption of the laptops and the one from the movements detected in the office:

![Occuppancy estimated via detected motions Vs laptop consumption](https://user-images.githubusercontent.com/49452402/139602912-b688f7dd-248d-4832-bc28-5eee76c0c88e.png)

We can also visualize other data for additional analysis 

![Office CO2 concentration Vs corridor CO2 concentration](https://user-images.githubusercontent.com/49452402/139602898-7c5ab33b-cce0-47a0-9375-601720240d45.png)

![Occuppancy estimated via detected motions](https://user-images.githubusercontent.com/49452402/139602885-45e0dad3-4dda-4e8a-b094-603a53530f49.png)

![Occuppancy estimated via laptop consumption](https://user-images.githubusercontent.com/49452402/139602890-107f4670-ea16-4bcf-b095-c30c93fde0f3.png)


## Tech stack

• Java




          
            
