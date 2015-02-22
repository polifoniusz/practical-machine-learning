# Prediction Assignment Writeup

The assignment was completed in following steps:

1. Dataset inspection

I've started by inspecting the downloaded CSV file in a plain text viewer, to reach the following conclusions:
* The first 7 columns are metadata.
* Many columns contain data only in rows where new_window=yes, making them mostly empty.
 
These columns will be removed from the dataset as they have little to no predictive power. 

2. Dataset import

I've imported the CSV using the standard *read.csv* function:


```r
tr = read.csv('C:\\sync\\pml\\pml-training.csv',quote='')
```

The *quote* parameter was necessary so that R would actually parse the CSV into separate columns.
Without it, all lines were read as strings.

The resulting data frame took the following form:

```r
head(tr)
```

```
##   X. X..user_name.. X..raw_timestamp_part_1.. X..raw_timestamp_part_2..
## 1 "1   ""carlitos""                1323084231                    788290
## 2 "2   ""carlitos""                1323084231                    808298
## 3 "3   ""carlitos""                1323084231                    820366
## 4 "4   ""carlitos""                1323084232                    120339
## 5 "5   ""carlitos""                1323084232                    196328
## 6 "6   ""carlitos""                1323084232                    304277
##    X..cvtd_timestamp.. X..new_window.. X..num_window.. X..roll_belt..
## 1 ""05/12/2011 11:23""          ""no""              11           1.41
## 2 ""05/12/2011 11:23""          ""no""              11           1.41
## 3 ""05/12/2011 11:23""          ""no""              11           1.42
## 4 ""05/12/2011 11:23""          ""no""              12           1.48
## 5 ""05/12/2011 11:23""          ""no""              12           1.48
## 6 ""05/12/2011 11:23""          ""no""              12           1.45
##   X..pitch_belt.. X..yaw_belt.. X..total_accel_belt..
## 1            8.07         -94.4                     3
## 2            8.07         -94.4                     3
## 3            8.07         -94.4                     3
## 4            8.05         -94.4                     3
## 5            8.07         -94.4                     3
## 6            8.06         -94.4                     3
##   X..kurtosis_roll_belt.. X..kurtosis_picth_belt.. X..kurtosis_yaw_belt..
## 1                    """"                     """"                   """"
## 2                    """"                     """"                   """"
## 3                    """"                     """"                   """"
## 4                    """"                     """"                   """"
## 5                    """"                     """"                   """"
## 6                    """"                     """"                   """"
##   X..skewness_roll_belt.. X..skewness_roll_belt.1.. X..skewness_yaw_belt..
## 1                    """"                      """"                   """"
## 2                    """"                      """"                   """"
## 3                    """"                      """"                   """"
## 4                    """"                      """"                   """"
## 5                    """"                      """"                   """"
## 6                    """"                      """"                   """"
##   X..max_roll_belt.. X..max_picth_belt.. X..max_yaw_belt..
## 1                 NA                  NA              """"
## 2                 NA                  NA              """"
## 3                 NA                  NA              """"
## 4                 NA                  NA              """"
## 5                 NA                  NA              """"
## 6                 NA                  NA              """"
##   X..min_roll_belt.. X..min_pitch_belt.. X..min_yaw_belt..
## 1                 NA                  NA              """"
## 2                 NA                  NA              """"
## 3                 NA                  NA              """"
## 4                 NA                  NA              """"
## 5                 NA                  NA              """"
## 6                 NA                  NA              """"
##   X..amplitude_roll_belt.. X..amplitude_pitch_belt..
## 1                       NA                        NA
## 2                       NA                        NA
## 3                       NA                        NA
## 4                       NA                        NA
## 5                       NA                        NA
## 6                       NA                        NA
##   X..amplitude_yaw_belt.. X..var_total_accel_belt.. X..avg_roll_belt..
## 1                    """"                        NA                 NA
## 2                    """"                        NA                 NA
## 3                    """"                        NA                 NA
## 4                    """"                        NA                 NA
## 5                    """"                        NA                 NA
## 6                    """"                        NA                 NA
##   X..stddev_roll_belt.. X..var_roll_belt.. X..avg_pitch_belt..
## 1                    NA                 NA                  NA
## 2                    NA                 NA                  NA
## 3                    NA                 NA                  NA
## 4                    NA                 NA                  NA
## 5                    NA                 NA                  NA
## 6                    NA                 NA                  NA
##   X..stddev_pitch_belt.. X..var_pitch_belt.. X..avg_yaw_belt..
## 1                     NA                  NA                NA
## 2                     NA                  NA                NA
## 3                     NA                  NA                NA
## 4                     NA                  NA                NA
## 5                     NA                  NA                NA
## 6                     NA                  NA                NA
##   X..stddev_yaw_belt.. X..var_yaw_belt.. X..gyros_belt_x..
## 1                   NA                NA              0.00
## 2                   NA                NA              0.02
## 3                   NA                NA              0.00
## 4                   NA                NA              0.02
## 5                   NA                NA              0.02
## 6                   NA                NA              0.02
##   X..gyros_belt_y.. X..gyros_belt_z.. X..accel_belt_x.. X..accel_belt_y..
## 1              0.00             -0.02               -21                 4
## 2              0.00             -0.02               -22                 4
## 3              0.00             -0.02               -20                 5
## 4              0.00             -0.03               -22                 3
## 5              0.02             -0.02               -21                 2
## 6              0.00             -0.02               -21                 4
##   X..accel_belt_z.. X..magnet_belt_x.. X..magnet_belt_y..
## 1                22                 -3                599
## 2                22                 -7                608
## 3                23                 -2                600
## 4                21                 -6                604
## 5                24                 -6                600
## 6                21                  0                603
##   X..magnet_belt_z.. X..roll_arm.. X..pitch_arm.. X..yaw_arm..
## 1               -313          -128           22.5         -161
## 2               -311          -128           22.5         -161
## 3               -305          -128           22.5         -161
## 4               -310          -128           22.1         -161
## 5               -302          -128           22.1         -161
## 6               -312          -128           22.0         -161
##   X..total_accel_arm.. X..var_accel_arm.. X..avg_roll_arm..
## 1                   34                 NA                NA
## 2                   34                 NA                NA
## 3                   34                 NA                NA
## 4                   34                 NA                NA
## 5                   34                 NA                NA
## 6                   34                 NA                NA
##   X..stddev_roll_arm.. X..var_roll_arm.. X..avg_pitch_arm..
## 1                   NA                NA                 NA
## 2                   NA                NA                 NA
## 3                   NA                NA                 NA
## 4                   NA                NA                 NA
## 5                   NA                NA                 NA
## 6                   NA                NA                 NA
##   X..stddev_pitch_arm.. X..var_pitch_arm.. X..avg_yaw_arm..
## 1                    NA                 NA               NA
## 2                    NA                 NA               NA
## 3                    NA                 NA               NA
## 4                    NA                 NA               NA
## 5                    NA                 NA               NA
## 6                    NA                 NA               NA
##   X..stddev_yaw_arm.. X..var_yaw_arm.. X..gyros_arm_x.. X..gyros_arm_y..
## 1                  NA               NA             0.00             0.00
## 2                  NA               NA             0.02            -0.02
## 3                  NA               NA             0.02            -0.02
## 4                  NA               NA             0.02            -0.03
## 5                  NA               NA             0.00            -0.03
## 6                  NA               NA             0.02            -0.03
##   X..gyros_arm_z.. X..accel_arm_x.. X..accel_arm_y.. X..accel_arm_z..
## 1            -0.02             -288              109             -123
## 2            -0.02             -290              110             -125
## 3            -0.02             -289              110             -126
## 4             0.02             -289              111             -123
## 5             0.00             -289              111             -123
## 6             0.00             -289              111             -122
##   X..magnet_arm_x.. X..magnet_arm_y.. X..magnet_arm_z..
## 1              -368               337               516
## 2              -369               337               513
## 3              -368               344               513
## 4              -372               344               512
## 5              -374               337               506
## 6              -369               342               513
##   X..kurtosis_roll_arm.. X..kurtosis_picth_arm.. X..kurtosis_yaw_arm..
## 1                   """"                    """"                  """"
## 2                   """"                    """"                  """"
## 3                   """"                    """"                  """"
## 4                   """"                    """"                  """"
## 5                   """"                    """"                  """"
## 6                   """"                    """"                  """"
##   X..skewness_roll_arm.. X..skewness_pitch_arm.. X..skewness_yaw_arm..
## 1                   """"                    """"                  """"
## 2                   """"                    """"                  """"
## 3                   """"                    """"                  """"
## 4                   """"                    """"                  """"
## 5                   """"                    """"                  """"
## 6                   """"                    """"                  """"
##   X..max_roll_arm.. X..max_picth_arm.. X..max_yaw_arm.. X..min_roll_arm..
## 1                NA                 NA               NA                NA
## 2                NA                 NA               NA                NA
## 3                NA                 NA               NA                NA
## 4                NA                 NA               NA                NA
## 5                NA                 NA               NA                NA
## 6                NA                 NA               NA                NA
##   X..min_pitch_arm.. X..min_yaw_arm.. X..amplitude_roll_arm..
## 1                 NA               NA                      NA
## 2                 NA               NA                      NA
## 3                 NA               NA                      NA
## 4                 NA               NA                      NA
## 5                 NA               NA                      NA
## 6                 NA               NA                      NA
##   X..amplitude_pitch_arm.. X..amplitude_yaw_arm.. X..roll_dumbbell..
## 1                       NA                     NA           13.05217
## 2                       NA                     NA           13.13074
## 3                       NA                     NA           12.85075
## 4                       NA                     NA           13.43120
## 5                       NA                     NA           13.37872
## 6                       NA                     NA           13.38246
##   X..pitch_dumbbell.. X..yaw_dumbbell.. X..kurtosis_roll_dumbbell..
## 1           -70.49400         -84.87394                        """"
## 2           -70.63751         -84.71065                        """"
## 3           -70.27812         -85.14078                        """"
## 4           -70.39379         -84.87363                        """"
## 5           -70.42856         -84.85306                        """"
## 6           -70.81759         -84.46500                        """"
##   X..kurtosis_picth_dumbbell.. X..kurtosis_yaw_dumbbell..
## 1                         """"                       """"
## 2                         """"                       """"
## 3                         """"                       """"
## 4                         """"                       """"
## 5                         """"                       """"
## 6                         """"                       """"
##   X..skewness_roll_dumbbell.. X..skewness_pitch_dumbbell..
## 1                        """"                         """"
## 2                        """"                         """"
## 3                        """"                         """"
## 4                        """"                         """"
## 5                        """"                         """"
## 6                        """"                         """"
##   X..skewness_yaw_dumbbell.. X..max_roll_dumbbell..
## 1                       """"                     NA
## 2                       """"                     NA
## 3                       """"                     NA
## 4                       """"                     NA
## 5                       """"                     NA
## 6                       """"                     NA
##   X..max_picth_dumbbell.. X..max_yaw_dumbbell.. X..min_roll_dumbbell..
## 1                      NA                  """"                     NA
## 2                      NA                  """"                     NA
## 3                      NA                  """"                     NA
## 4                      NA                  """"                     NA
## 5                      NA                  """"                     NA
## 6                      NA                  """"                     NA
##   X..min_pitch_dumbbell.. X..min_yaw_dumbbell..
## 1                      NA                  """"
## 2                      NA                  """"
## 3                      NA                  """"
## 4                      NA                  """"
## 5                      NA                  """"
## 6                      NA                  """"
##   X..amplitude_roll_dumbbell.. X..amplitude_pitch_dumbbell..
## 1                           NA                            NA
## 2                           NA                            NA
## 3                           NA                            NA
## 4                           NA                            NA
## 5                           NA                            NA
## 6                           NA                            NA
##   X..amplitude_yaw_dumbbell.. X..total_accel_dumbbell..
## 1                        """"                        37
## 2                        """"                        37
## 3                        """"                        37
## 4                        """"                        37
## 5                        """"                        37
## 6                        """"                        37
##   X..var_accel_dumbbell.. X..avg_roll_dumbbell.. X..stddev_roll_dumbbell..
## 1                      NA                     NA                        NA
## 2                      NA                     NA                        NA
## 3                      NA                     NA                        NA
## 4                      NA                     NA                        NA
## 5                      NA                     NA                        NA
## 6                      NA                     NA                        NA
##   X..var_roll_dumbbell.. X..avg_pitch_dumbbell..
## 1                     NA                      NA
## 2                     NA                      NA
## 3                     NA                      NA
## 4                     NA                      NA
## 5                     NA                      NA
## 6                     NA                      NA
##   X..stddev_pitch_dumbbell.. X..var_pitch_dumbbell.. X..avg_yaw_dumbbell..
## 1                         NA                      NA                    NA
## 2                         NA                      NA                    NA
## 3                         NA                      NA                    NA
## 4                         NA                      NA                    NA
## 5                         NA                      NA                    NA
## 6                         NA                      NA                    NA
##   X..stddev_yaw_dumbbell.. X..var_yaw_dumbbell.. X..gyros_dumbbell_x..
## 1                       NA                    NA                     0
## 2                       NA                    NA                     0
## 3                       NA                    NA                     0
## 4                       NA                    NA                     0
## 5                       NA                    NA                     0
## 6                       NA                    NA                     0
##   X..gyros_dumbbell_y.. X..gyros_dumbbell_z.. X..accel_dumbbell_x..
## 1                 -0.02                  0.00                  -234
## 2                 -0.02                  0.00                  -233
## 3                 -0.02                  0.00                  -232
## 4                 -0.02                 -0.02                  -232
## 5                 -0.02                  0.00                  -233
## 6                 -0.02                  0.00                  -234
##   X..accel_dumbbell_y.. X..accel_dumbbell_z.. X..magnet_dumbbell_x..
## 1                    47                  -271                   -559
## 2                    47                  -269                   -555
## 3                    46                  -270                   -561
## 4                    48                  -269                   -552
## 5                    48                  -270                   -554
## 6                    48                  -269                   -558
##   X..magnet_dumbbell_y.. X..magnet_dumbbell_z.. X..roll_forearm..
## 1                    293                    -65              28.4
## 2                    296                    -64              28.3
## 3                    298                    -63              28.3
## 4                    303                    -60              28.1
## 5                    292                    -68              28.0
## 6                    294                    -66              27.9
##   X..pitch_forearm.. X..yaw_forearm.. X..kurtosis_roll_forearm..
## 1              -63.9             -153                       """"
## 2              -63.9             -153                       """"
## 3              -63.9             -152                       """"
## 4              -63.9             -152                       """"
## 5              -63.9             -152                       """"
## 6              -63.9             -152                       """"
##   X..kurtosis_picth_forearm.. X..kurtosis_yaw_forearm..
## 1                        """"                      """"
## 2                        """"                      """"
## 3                        """"                      """"
## 4                        """"                      """"
## 5                        """"                      """"
## 6                        """"                      """"
##   X..skewness_roll_forearm.. X..skewness_pitch_forearm..
## 1                       """"                        """"
## 2                       """"                        """"
## 3                       """"                        """"
## 4                       """"                        """"
## 5                       """"                        """"
## 6                       """"                        """"
##   X..skewness_yaw_forearm.. X..max_roll_forearm.. X..max_picth_forearm..
## 1                      """"                    NA                     NA
## 2                      """"                    NA                     NA
## 3                      """"                    NA                     NA
## 4                      """"                    NA                     NA
## 5                      """"                    NA                     NA
## 6                      """"                    NA                     NA
##   X..max_yaw_forearm.. X..min_roll_forearm.. X..min_pitch_forearm..
## 1                 """"                    NA                     NA
## 2                 """"                    NA                     NA
## 3                 """"                    NA                     NA
## 4                 """"                    NA                     NA
## 5                 """"                    NA                     NA
## 6                 """"                    NA                     NA
##   X..min_yaw_forearm.. X..amplitude_roll_forearm..
## 1                 """"                          NA
## 2                 """"                          NA
## 3                 """"                          NA
## 4                 """"                          NA
## 5                 """"                          NA
## 6                 """"                          NA
##   X..amplitude_pitch_forearm.. X..amplitude_yaw_forearm..
## 1                           NA                       """"
## 2                           NA                       """"
## 3                           NA                       """"
## 4                           NA                       """"
## 5                           NA                       """"
## 6                           NA                       """"
##   X..total_accel_forearm.. X..var_accel_forearm.. X..avg_roll_forearm..
## 1                       36                     NA                    NA
## 2                       36                     NA                    NA
## 3                       36                     NA                    NA
## 4                       36                     NA                    NA
## 5                       36                     NA                    NA
## 6                       36                     NA                    NA
##   X..stddev_roll_forearm.. X..var_roll_forearm.. X..avg_pitch_forearm..
## 1                       NA                    NA                     NA
## 2                       NA                    NA                     NA
## 3                       NA                    NA                     NA
## 4                       NA                    NA                     NA
## 5                       NA                    NA                     NA
## 6                       NA                    NA                     NA
##   X..stddev_pitch_forearm.. X..var_pitch_forearm.. X..avg_yaw_forearm..
## 1                        NA                     NA                   NA
## 2                        NA                     NA                   NA
## 3                        NA                     NA                   NA
## 4                        NA                     NA                   NA
## 5                        NA                     NA                   NA
## 6                        NA                     NA                   NA
##   X..stddev_yaw_forearm.. X..var_yaw_forearm.. X..gyros_forearm_x..
## 1                      NA                   NA                 0.03
## 2                      NA                   NA                 0.02
## 3                      NA                   NA                 0.03
## 4                      NA                   NA                 0.02
## 5                      NA                   NA                 0.02
## 6                      NA                   NA                 0.02
##   X..gyros_forearm_y.. X..gyros_forearm_z.. X..accel_forearm_x..
## 1                 0.00                -0.02                  192
## 2                 0.00                -0.02                  192
## 3                -0.02                 0.00                  196
## 4                -0.02                 0.00                  189
## 5                 0.00                -0.02                  189
## 6                -0.02                -0.03                  193
##   X..accel_forearm_y.. X..accel_forearm_z.. X..magnet_forearm_x..
## 1                  203                 -215                   -17
## 2                  203                 -216                   -18
## 3                  204                 -213                   -18
## 4                  206                 -214                   -16
## 5                  206                 -214                   -17
## 6                  203                 -215                    -9
##   X..magnet_forearm_y.. X..magnet_forearm_z.. X..classe...
## 1                   654                   476       ""A"""
## 2                   661                   473       ""A"""
## 3                   658                   469       ""A"""
## 4                   658                   469       ""A"""
## 5                   655                   473       ""A"""
## 6                   660                   478       ""A"""
```

3. Dataset cleanup

The necessary cleanup steps included these identified during inspection as well as extra ones that were needed to fix the non-ideal import result.

The resulting steps went as follows:

* removal of metadata columns

```r
tr <- tr[,8:160]
```

* removal of unnecessary quotes

```r
tr[,which(!sapply(tr, is.numeric))] <- sapply(tr[,which(!sapply(tr, is.numeric))], function(x) gsub("\"", "", x))
```
Extra care had to be taken to avoid processing of numeric columns with the *gsub* function - as it converts them to character based.

* removal unnecessary characters from the headers (X and dots)

```r
names(tr) <- gsub("X|\\.","",names(tr))
```

* removing mostly empty columns

```r
tr <- tr[,which(!sapply(tr, function(x) sum(is.na(x))>0 || x==''))]
```
The condition *is.na(x))>0 || x==''* was designed to catch columns where missing values were represented as NA as well as those with empty strings.


The cleaned up data frame looked like this:

```r
head(tr)
```

```
##   roll_belt pitch_belt yaw_belt total_accel_belt gyros_belt_x gyros_belt_y
## 1      1.41       8.07    -94.4                3         0.00         0.00
## 2      1.41       8.07    -94.4                3         0.02         0.00
## 3      1.42       8.07    -94.4                3         0.00         0.00
## 4      1.48       8.05    -94.4                3         0.02         0.00
## 5      1.48       8.07    -94.4                3         0.02         0.02
## 6      1.45       8.06    -94.4                3         0.02         0.00
##   gyros_belt_z accel_belt_x accel_belt_y accel_belt_z magnet_belt_x
## 1        -0.02          -21            4           22            -3
## 2        -0.02          -22            4           22            -7
## 3        -0.02          -20            5           23            -2
## 4        -0.03          -22            3           21            -6
## 5        -0.02          -21            2           24            -6
## 6        -0.02          -21            4           21             0
##   magnet_belt_y magnet_belt_z roll_arm pitch_arm yaw_arm total_accel_arm
## 1           599          -313     -128      22.5    -161              34
## 2           608          -311     -128      22.5    -161              34
## 3           600          -305     -128      22.5    -161              34
## 4           604          -310     -128      22.1    -161              34
## 5           600          -302     -128      22.1    -161              34
## 6           603          -312     -128      22.0    -161              34
##   gyros_arm_x gyros_arm_y gyros_arm_z accel_arm_x accel_arm_y accel_arm_z
## 1        0.00        0.00       -0.02        -288         109        -123
## 2        0.02       -0.02       -0.02        -290         110        -125
## 3        0.02       -0.02       -0.02        -289         110        -126
## 4        0.02       -0.03        0.02        -289         111        -123
## 5        0.00       -0.03        0.00        -289         111        -123
## 6        0.02       -0.03        0.00        -289         111        -122
##   magnet_arm_x magnet_arm_y magnet_arm_z roll_dumbbell pitch_dumbbell
## 1         -368          337          516      13.05217      -70.49400
## 2         -369          337          513      13.13074      -70.63751
## 3         -368          344          513      12.85075      -70.27812
## 4         -372          344          512      13.43120      -70.39379
## 5         -374          337          506      13.37872      -70.42856
## 6         -369          342          513      13.38246      -70.81759
##   yaw_dumbbell total_accel_dumbbell gyros_dumbbell_x gyros_dumbbell_y
## 1    -84.87394                   37                0            -0.02
## 2    -84.71065                   37                0            -0.02
## 3    -85.14078                   37                0            -0.02
## 4    -84.87363                   37                0            -0.02
## 5    -84.85306                   37                0            -0.02
## 6    -84.46500                   37                0            -0.02
##   gyros_dumbbell_z accel_dumbbell_x accel_dumbbell_y accel_dumbbell_z
## 1             0.00             -234               47             -271
## 2             0.00             -233               47             -269
## 3             0.00             -232               46             -270
## 4            -0.02             -232               48             -269
## 5             0.00             -233               48             -270
## 6             0.00             -234               48             -269
##   magnet_dumbbell_x magnet_dumbbell_y magnet_dumbbell_z roll_forearm
## 1              -559               293               -65         28.4
## 2              -555               296               -64         28.3
## 3              -561               298               -63         28.3
## 4              -552               303               -60         28.1
## 5              -554               292               -68         28.0
## 6              -558               294               -66         27.9
##   pitch_forearm yaw_forearm total_accel_forearm gyros_forearm_x
## 1         -63.9        -153                  36            0.03
## 2         -63.9        -153                  36            0.02
## 3         -63.9        -152                  36            0.03
## 4         -63.9        -152                  36            0.02
## 5         -63.9        -152                  36            0.02
## 6         -63.9        -152                  36            0.02
##   gyros_forearm_y gyros_forearm_z accel_forearm_x accel_forearm_y
## 1            0.00           -0.02             192             203
## 2            0.00           -0.02             192             203
## 3           -0.02            0.00             196             204
## 4           -0.02            0.00             189             206
## 5            0.00           -0.02             189             206
## 6           -0.02           -0.03             193             203
##   accel_forearm_z magnet_forearm_x magnet_forearm_y magnet_forearm_z
## 1            -215              -17              654              476
## 2            -216              -18              661              473
## 3            -213              -18              658              469
## 4            -214              -16              658              469
## 5            -214              -17              655              473
## 6            -215               -9              660              478
##   classe
## 1      A
## 2      A
## 3      A
## 4      A
## 5      A
## 6      A
```

4. Classifier training

I have chosen to use a Random Forest classifier for high accuracy and applied the *randomForest* package for this task:


```r
library(randomForest)
r <- randomForest(formula = as.factor(classe) ~ ., data = tr)
```

The resulting classifier achieved the following results:

```r
r
```

```
## 
## Call:
##  randomForest(formula = as.factor(classe) ~ ., data = tr) 
##                Type of random forest: classification
##                      Number of trees: 500
## No. of variables tried at each split: 7
## 
##         OOB estimate of  error rate: 0.29%
## Confusion matrix:
##      A    B    C    D    E  class.error
## A 5578    1    0    0    1 0.0003584229
## B   12 3781    4    0    0 0.0042138530
## C    0   10 3411    1    0 0.0032144944
## D    0    0   22 3192    2 0.0074626866
## E    0    0    0    3 3604 0.0008317161
```

Out of the box, the classifier performance was very good, so I performed no further fine tuning.

5. Test set import and clean-up

The testing set CSV was handled in the same way as the training set CSV:


```r
ts = read.csv('C:\\sync\\pml\\pml-testing.csv',quote='')
ts[,which(!sapply(ts, is.numeric))] <- sapply(ts[,which(!sapply(ts, is.numeric))], function(x) gsub("\"", "", x))
names(ts) <- gsub("X|\\.","",names(ts))
```

Column removal steps were omited as the classifier is already trained to use the right subset.

6. Prediction

Prediction was performed in a straight forward way, producing the following values:


```r
pr <- predict(r, ts)
pr
```

```
##  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 
##  B  A  B  A  A  E  D  B  A  A  B  C  B  A  E  E  A  B  B  B 
## Levels: A B C D E
```

7. Output

The results were saved to disk using the function provided at the course website.


```r
pml_write_files = function(x){
  n = length(x)
  for(i in 1:n){
    filename = paste0("C:\\sync\\pml\\problem_id_",i,".txt")
    write.table(x[i],file=filename,quote=FALSE,row.names=FALSE,col.names=FALSE)
  }
}
pml_write_files(pr)
```

This produced a set of text files which I submitted one by one for a perfect score.
