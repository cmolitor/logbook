---
layout:     post
title:      Adjusting measurement data in influxdb
author:     Christoph Molitor
tags: 		Influxdb Grafana Python Node-RED
subtitle:  	Trying to adjust some measuring data in influxdb - ongoing
category:  	report
published:	true
---
<!-- Start Writing Below in Markdown -->

## Summary

I wanted to adjust measurement data in my influxdb due to some wrong settings in the measurement device. The procedure to adjust the data is described in the following sections. However, after performing the adjustment, I realized that the adjustment is not suitable for all of the different measurement types I had in the database.

## Background 

I was recording some measurement data from a Janitza energy meter using Node-RED, influxdb and grafana. After a while I noticed that the measurement data is not plausible. The only option was that the current transformer ratio was not set correct at the Janitza energy meter. And indeed the actual current transformer ratio was 250 and the parameter set at the energy meter was 150. 
After changing the parameter, the newly recorded data looked plausible. 

As I didn't want to delete the recorded data (more than 6 month of data), I wanted to adjust the recorded data.
However, my first attempt to do this, didn't work out as expected. As I am writing this, I am still looking for a solution.

## Procedure

As influxdb offers no option to adjust/manipulate the data, I thought of the following procedure:

1. Export data with the following command:
``` influx_inspect export -datadir "/var/lib/influxdb/data" -waldir "/var/lib/influxdb/wal" -out "smartfarm" -database smartfarm -retention autogen```

2. Manipulate data with a Python script:
    As an example the data looked the following way (excerpt):
    ```
    E_real_con value=6.09633e+07 1542060000543464984
    E_real_con value=6.10586e+07 1542146400564041196
    E_real_con value=6.1149736e+07 1542232800588670399
    E_real_con value=6.1245752e+07 1542319200593853720
    E_real_con value=6.1333508e+07 1542405600602153330
    E_real_con value=6.1419752e+07 1542492000604431684
    ```
    I performed the following steps to manipulate the data:
    1. Search and replace "value="" with "". So remove "value="
    2. Run the following Python script which basically multiplies the values with a correction factor:
        ```python
        import numpy as np 
        import pandas as pd

        data = pd.read_csv('smartfarm', sep=" ", header=8)
        data.columns = ["measurement", "value", "timestamp"]

        # Filter only relevant lines
        data_corrected = data[(data['timestamp']<1563012000000000000) & ((data['measurement']=="P") | (data['measurement']=="P1") | (data['measurement']=="P2") | (data['measurement']=="P3") | (data['measurement']=="E_cap") | (data['measurement']=="E_ind") | (data['measurement']=="E_real_con") | (data['measurement']=="E_real_del"))]

        # Scale data 
        data_corrected.value *=5.0/3.0
        data_corrected.to_csv("smartfarm_processed", sep=' ', encoding='utf-8', index=False)
        ```
    3. Add "value=" back to the manipulated data file with search and replace "E_real_con " by "E_real_con value=". 


3. Put data back to influxdb
```
curl -i -XPOST "http://localhost:8086/write?db=smartfarm" --data-binary @smartfarm_processed
```

Influxdb "updates" the value if the timestamp is identical.

## Result

Actually the four power values (every phase and sum) have been adjusted properly and data looks fine. However, the energy values are cumulative values and scaling of these values caused the following issue: 
The last cumulative energy value recorded before changing the current transformer ratio has been changed to the value as if the current transformer ratio has been set properly from the beginning. But the first reading of these values after correcting the current transformer ratio still starts form the value stored in the energy meter which canot be adjusted. As result, the cumulative energy value dropped at the time the current transformer ratio was changed.

Why haven't I thought about this before?
Actually, I never display the cumulative energy value but the daily/monthly or annualy delta which is calculated by the query in Grafana. Thus, I saw the problem after putting the data back to the database and looking at my data in Grafana dashboard. Scaling the daily/monthly or annualy delta values by the correction factor would have been fine.
On the day the current transformer ratio has been adjusted, the displayed data was inverted and with high absolute values.

## Possible other solutions to try out

1. Continuous query in influxdb

    The idea is to copy the orginal data from the backup back to the database and calculate the daily/montly/yearly values using continuous query where I multiply the values before changing the current transformer ratio with the correction factor.

2. Add an offset to the energy meter values
    
    The offset would be the delta between the corrected energy meter value and the value still stored in the energy meter Modbus register.

