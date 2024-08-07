### Global and direct solar irradiance estimation using deep learning and selected spectral satellite images

This repository includes the data as well as example scripts for the research paper: [https://www.sciencedirect.com/science/article/pii/S0306261923013430](https://www.sciencedirect.com/science/article/pii/S0306261923013430).

The following figure shows the flowchart for GHI and DNI estimations using spectral satellite images and deep learning.

![image](https://github.com/sl-chen/Solar-resourcing-with-satellite-images-and-deep-learning/blob/main/figure/flowchart.jpg)


#### Data
The satellite data of GOES-16 is downloaded via public available source, e.g., Amazon Web Services, for GOES-16, please refer to: https://docs.opendata.aws/noaa-goes16/cics-readme.html#accessing-goes-data-on-aws.

|Band|$\lambda$ [μm]|Center $\lambda$ [μm]|Resolution (km)|Nickname|Type|
|:-----:|:---------: | :---------: | :--------: |:------:| :------------: |
|  1  |  0.45-0.49   | 0.47  | 1  | Blue |  Visible  |
|  2  |  0.59-0.69   | 0.64  | 0.5 | Red | Visible |
|  3  |  0.846-0.885 | 0.865 | 1 | Veggie | Near-Infrared |
|  4  |  1.371-1.386 | 1.378 | 2 | Cirrus | Near-Infrared  | 
|  5  |  1.58-1.64   | 1.61  | 1 | Snow/Ice | Near-Infrared  |
|  6  |  2.225-2.275 | 2.25  | 2 | Cloud particle size | Near-Infrared  |
|  7  |  3.80-4.00   | 3.90  | 2 | Shortwave window | Infrared  |
|  8  |  5.77-6.60   | 6.19  | 2 | Upper-level water vapor | Infrared   |
|  9  |  6.75-7.15   | 6.95  | 2 | Mid-level water vapor | Infrared  |
|  10 |  7.24-7.44   | 7.34  | 2 | Lower-level water vapor | Infrared |
|  11 |  8.30-8.70   | 8.50  | 2 | Cloud-top phase | Infrared   |
|  12 |  9.42-9.80   | 9.61  | 2 | Ozone | Infrared |
|  13 | 10.10-10.60  | 10.35 | 2 | "Clean" longwave window |  Infrared |        
|  14 | 10.80-11.60  | 11.20 | 2 | Longwave window | Infrared     |
|  15 | 11.80-12.80  | 12.30 | 2 | "Dirty" longwave window | Infrared     |  
|  16 | 13.00-13.30  | 1.378 | 2 | CO2 longwave  | Infrared |

The correlation analysis show some satellite bands are highly correlated, therefore, only a sub set of bands is selected: C01, C03, C04, C05, C06, C07, C09, and C11.

The used ground data is from SURFRAD stations (see the following table) with quality control.

|Station|Latitude (°)|Longitude (°)|Altitude (m)|Timezone|
|:-----:|:---------: | :---------: | :--------: |:------:|
|  BON  |  40.05     | -88.37      |  230       |  UTC-6 |
|  DRA  |  36.62     | -116.02     |  1007      |  UTC-8 |
|  FPK  |  48.31     | -105.10     |  634       |  UTC-7 |
|  GWN  |  34.25     | -89.87      |  98        |  UTC-6 |
|  PSU  |  40.72     | -77.93      |  376       |  UTC-5 |
|  SXF  |  43.73     | -96.92      |  473       |  UTC-6 |
|  TBL  |  40.12     | -105.24     |  1689      |  UTC-7 |

#### The deep learning model for solar resource assessment

The following figure shows an illustration of the structure of deep learning model for estimating ground solar irradiance using the GOES-16 images of selected bands. The inputs are images of 8 selected bands with size of $11\times11$ pixels, the output is the clear-sky index (Note that the hyperparameters might vary for different locations, this figure is just to show the structure).

![image](https://github.com/sl-chen/Solar-resourcing-with-satellite-images-and-deep-learning/blob/main/figure/Model.jpg)

The output is the clear-sky index, which is defined as the ratio between measured GHI and clear-sky GHI estimate. Clear-sky index is used to normalize the irradiance time series and reduce the seasonal and diurnal variations. Note that the clear-sky index is defined based on GHI. However, when developing the deep learning model for DNI estimation, the concept of clear-sky index is also adopted for DNI measurements and clear-sky DNI. Clear-sky GHI and clear-sky DNI are estimated by the Ineichen-Perez model [1] due to the simplicity.

#### Results

The overall performance of GHI and DNI estimations using deep learning model (DNNa8) and physical solar model (NSRDB) are presented in the following Tables. Generally, DNNa8 performs better than NSRDB.

![image](https://github.com/sl-chen/Solar-resourcing-with-satellite-images-and-deep-learning/blob/main/figure/Tables.PNG)


#### Example data and notebooks

The spectral satellite data of GOES-16 for BON is available [here](https://drive.google.com/drive/folders/1oUjJ_2rKpEEG6TIbKOHX7C1zAueWnucN?usp=sharing) (year of 2019).

The NSRDB data for BON could be obtained from [here](https://nsrdb.nrel.gov/data-viewer) using lat and lon (lat, lon) information.

The notebook of GHI estimation at BON is available [here](https://github.com/sl-chen/Solar-resourcing-with-satellite-images-and-deep-learning/blob/main/ghi_estimation_bon.ipynb).

Note that the examples made here are only for GHI estimation at the BON station. However, the estimations of GHI and DNI at other SURFRAD stations follow the same methodology.


#### References
[1] Ineichen, P., & Perez, R. (2002). A new airmass independent formulation for the Linke turbidity coefficient. Solar Energy, 73(3), 151-157.
