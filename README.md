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

The original deep learning model in [2] was developed for ground irradiance estimates at a single location, which is centered in the domain of satellite images with $11\times11$ pixels ((a) in the above figure). The target station can be anywhere as long as there are on-site irradiance measurements available. Following the same methodology, the target is expanded from one station to the $11\times11$ surrounding area with 121 locations ((b) in the above figure). Selected spectral satellite images of GOES-16 with the size of $21\times21$ pixels are used to obtain the GHI estimates for the whole region ($11\times11$ pixels) via the pre-trained deep learning model. For more details on solar irradiance estimation using spectal satellite images and deep learning, please refer to [2].

#### Results

![](https://github.com/sl-chen/Solar-resourcing-with-satellite-images-and-deep-learning/blob/main/figure/Tables.PNG | width=100)


#### Example data and notebooks

The spectral satellite data of GOES-16 for BON is available [here](https://drive.google.com/drive/folders/1oUjJ_2rKpEEG6TIbKOHX7C1zAueWnucN?usp=sharing).

The NSRDB data for BON is available [here](https://drive.google.com/drive/folders/12n7YmZbkDdZkt_WcykwvgnRvsx6Eo-16?usp=sharing), satellite-derived spatial GHI using deep learning can be found at [here](https://drive.google.com/drive/folders/1to2rdRhWoN1jdBqllqGgQE7dd6_m8zbW?usp=sharing).

The notebook of the end-to-end deep learning model for satellite-based solar forecasting up to 3 hours is available [here](https://github.com/sl-chen/Solar-forecasting-with-deep-learning-model-chain/blob/main/ghi_forecasting_bon_sat_3h.ipynb).

The notebook of the hybrid physical deep learning model is available [here](https://github.com/sl-chen/Solar-forecasting-with-deep-learning-model-chain/blob/main/ghi_forecasting_bon_nsrdb-3h.ipynb).

The deep learning model using satellite-derived spatial GHI estimates (with a pre-trained deep learning model) thus forms a deep learning model chain, the notebook is available [here](https://github.com/sl-chen/Solar-forecasting-with-deep-learning-model-chain/blob/main/ghi_forecasting_bon_sat-dl-3h.ipynb).


#### Results

The forecast skills of SDL (the deep learning model chain), NS (the hybrid physical deep learning model), and SAT (the end-to-end deep learning model) are shown in the following figure for all the SURFRAD stations.

![image](https://github.com/sl-chen/Solar-forecasting-with-deep-learning-model-chain/blob/main/figures/Skill.PNG)



Note that the examples made here are only for the BON station. However, the results for other stations follows the same methods and procedure.

#### References
[1] Yang, D. (2018). SolarData: An R package for easy access of publicly available solar datasets. Solar Energy, 171, A3-A12.

[2] Chen, S., Li, C., Xie, Y., & Li, M. (2023). Global and direct solar irradiance estimation using deep learning and selected spectral satellite images. Applied Energy, 352, 121979.
