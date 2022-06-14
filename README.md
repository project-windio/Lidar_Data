# Lidar_Data
**Using this code it is possible to extract data using Modbus TCP/IP from the Lidar unit. This data gets interpreted and may be used for different purposes such as the digital twin (IoT application).
The register numbers are based on the Modbus Guide by the manufacturer ZXLidar. 
It is important to be aware that the implemented Modbus function is read only. Any changes to the Lidar unit must be made using the Waltz Software.**

##Data Structure:
After receiving the data (using lidar_data.py) the dataset is sent using ZeroMQ. The dataset needs to contain specific parameters to be compatible
with other services running on the Motion Sensor Box. 
![Data Structure](doc/data_structure.png)

## Glossary:

**timestamp_data_received** = if a new data set is available a timestamp is generated

**uptime** = the system uptime

**reference_of_dataset** = each data set got its own reference number,
					   the refernce number may be used to check wheter any data	
					   might be lost
					   
**timestamp_data_generated** = this timestamp is generated by the lidar unit and corresponds
						   to the reference number
						 
**set_height_x** = the height which is configured in the Waltz Software

**individual_reference_x** = since the lidat unit is only able to messure each height at every full
						 iteration once, the individual_reference_x corresponds only to this 
						 sub data set. This reference is used for the calculation of the individual_timestamp_x
						 
**individual_timestamp_x** = the individual_timestamp_x is not received from the lidar station. It is calculated as the following.
						 The time it takes to measure one height (usally 1 second) is multiplied by the place in the order of heights
						 (measured from heighest to lowest) and than add to the timestamp received by the lidar unit.
						 
**horizontal_windspeed_x** = the horizontal windspeed at the given height measured in m/s

**vertical_windspeed_x** = the vertical windspeed at the given height. A positive value equals up wind, a negative equals down wind.

**wind_direction_x** = the wind direction at the given height. 0° equals North.

**temperature** = the air temperature measured by the met station 

**battery** = the voltage of the battery

**air pressure** = the air pressure measured by the met station

**windspeed(*met-station*)** = the windspeed measured by the met station (approx. 1,5 m above the ground)

**tilt** = horizontal tilt from the met station

**humidity** = air humidity

**wind_direction** = wind direction measured at approx. 1,5 m above the ground

**pod_upper_temperature** = internal pod temperature as measured by internal 
						sensor located near the window
						
**pod_lower_temperature** = internal pod temperature as measured by internal 
						sensor located near the external heat sink
						
**pod_humidity** = internal pod relative humidity

**scan_dwell_time** = the time it takes to measure all windspeeds and parameters at one height


## Error Codes:
**If the Lidar can not measure a specific value, the Modbus register contain error codes instead. Explanation
is as the following:**

**9999** = It is not possible to measure wind speed data. Probale causes may be
	   very low windspeed, partial obstruction on the scanner window or interference
	   with the laser beam.
		
**9998** = atmospheric conditions which affect the wind speed measurement. This may be 
	   fog or precipitation.

