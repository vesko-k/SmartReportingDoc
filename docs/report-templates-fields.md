---
Title: Report Templates - Fields
---

Fields represent numeric data. This data can either be a constant number
or specific data extracted from the **PHD** process history.

A field, used for data extraction has following structure:

`part1#part2#part3#part4`

A field consists of parts and part delimiters where the sign `#` is
used as a part delimiter. The parts 1, 2 and 3 are case insensitive,
while part 4 is case sensitive. Strict order of parts must be followed
so the field can be correctly processed by the interpreter. Number of
parts used for data extraction are described below.


## phdN

> **Part:** `part1`  
> **Function:** Data provider  
> **Description:** Identifies that data is acquired from **PHD** server number  “**N**”. The number “**N**” (**N**=1,2,3) defines the number of the available PHD servers. The different **PHD**s are declared in the service configuration file. If **N** is omitted the first **PHD** connection is used (`phd1`). Maximum number of supported connections is **3**.

## raw

> **Part:** `part2`  
> **Function:** Data selector  
> **Description:** Returns raw value from the **PHD** server. 

## (raw|snapshot)_timestamp32

> **Part:** `part2`  
> **Function:** Data selector  
> **Description:** Returns timestamp in Unix time (INT32) of the raw value.

> __Example:__

> `raw_timestamp32`


> **Applicable only for:**  
> `last_val`, `last_val_dN_hM`, `week_day_last_val_dN_hM`, `month_day_last_val_dN_hM`, `around_val_dN_hM_oS`, `rval_RST` and `rval_RET`

## (raw|snapshot)_timestamp64

> **Part:** `part2`  
> **Function:** Data selector  
> **Description:** Returns timestamp in Windows file time (INT64) of the raw value.

> **Applicable only for:**  
> `last_val`, `last_val_dN_hM`, `week_day_last_val_dN_hM`, `month_day_last_val_dN_hM`, `around_val_dN_hM_oS`, `rval_RST` and `rval_RET`

## (raw|snapshot)_timestamp

> **Part:** `part2`  
> **Function:** Data selector  
> **Description:** Returns timestamp in the following format: `yyyy-MM-dd HH:mm:ss`

> **Applicable only for:**  
> `last_val`, `last_val_dN_hM`, `week_day_last_val_dN_hM`, `month_day_last_val_dN_hM`, `around_val_dN_hM_oS`, `rval_RST` and `rval_RET`

## snapshot

> **Part:** `part2`  
> **Function:** Data selector  
> **Description:** Returns snapshot value from the PHD server.


## avg_day_dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns average value considered from the requested report date **0h** shifted backward with **N+1** days and forward with **M** hours. **N** and **M** are numbers without leading zero.

```text
Sampletype:         Snapshot
SampleFrequency:    1s
ReductionFrequency: 86400s
ReductionType:      Average
ReductionOffset:    After
UseSampleFrequency: False
```

## begin_mon

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns data value for the beginning of the month of the requested report date.

## day_val

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns value for the requested report date.


## rval_RST

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns closest to the requested relative **PHD** time (**RST**) with highest confidence value. **RST** is a relative **PHD** time keyphrase like: `NOW`, `TODAY`, `YESTERDAY`, `TOMORROW`, `YEAR`. Also you can add or subtract offset by defining it with H for hours M for minutes, `S` for seconds, `MO` for months, `Y` for years and `D` for days. The `+` or `–` sign must be escaped like `[plus]` or `[minus]` respectively. **PHD** keyphrases and offsets are case insensitive.

> __Example:__

```text
rval_NOW
rval_NOW[minus]1D
rval_TODAY[plus]1H
you can combine offset phrases like:
rval_TODAY[plus]1H15M
```

## rval_RST_RET

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Same as `rval_RST`,  except that **RST** defines relative start time and **RET** defines relative end time of the requested period. Returns the closest to the end of the requested period value with highest confidence. 

> __Example:__

```text
rval_TODAY[minus]1D_TODAY
```

## day_val_dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns value for the requested report date shifted backward with **N** days and forward with **M** hours. **N** and **M** are numbers without leading zero.

```text
ValueDate = ReportDate(0h) - Ndays + Mhours
```

## around_val_dN_hM_oS

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns closest by time value with highest confidence to the requested date shifted backward with **N** days and forward with **M** hours. Seek interval is **S** seconds around the requested time `(RequestedTime +/- Sseconds)`. Under equal other circumstances sample with time greater than requested time has higher priotity. If snapshot is requested, sample frequency is set to **60** seconds. 
**N**, **M** and **S** are numbers without leading zero.

```text
RequestedTime = ReportDate(0h) - Ndays + Mhours
Seek Interval: [RequestedTime – Sseconds ; RequestedTime + Sseconds]
```

## last_val

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns last value for the requested report date.

## last_val_dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns value for the requested report date previous month last day **0h** shifted forward with **N** days and **M** hours. **N** and **M** are numbers without leading zero. Tags are separated with "**,**" and returns the value of the tag with the latest timestamp.

`ValueDate = ReportDatePrevMonthLastDay(0h) + Ndays + Mhours`

## month_avg_day_dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns average value considered from the requested report date previous month last day **0h** shifted forward with **N-1** days and **M** hours. **N** and **M** are numbers without leading zero.

```text
Sampletype:         Snapshot
SampleFrequency:    1s
ReductionFrequency: 86400s
ReductionType:      Average
ReductionOffset:    After
UseSampleFrequency: False
```

## month_day_dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns value for the requested report date previous month last day **0h** shifted forward with **N** days and **M** hours. **N** and **M** are numbers without leading zero.

`ValueDate = ReportDatePrevMonthLastDay(0h) + Ndays + Mhours`

## month_day_last_val_dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns value for the requested report date previous month last day **0h** shifted forward with **N** days and **M** hours. **N** and **M** are numbers without leading zero. Tags are separated with "**,**" and returns the value of the tag with the latest timestamp.

`ValueDate = ReportDatePrevMonthLastDay(0h) + Ndays + Mhours`

## prev_day_val

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns value for the requested report date shifted backward with **1** day.

## sum_avghour_day_ dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns accumulated average hour values for **24** hours period considered from  the requested report date shifted backward with **N+1** days and forward with M hours. **N** and **M** are numbers without leading zero.
```text
ValueDate= ReportDate(0h) – (N+1)days + Mhours

Sampletype:         Snapshot
SampleFrequency:    60s
ReductionFrequency: 3600s
ReductionType:      Average
ReductionOffset:    After
UseSampleFrequency: False
BeginTime:          ValueDate
EndTime:            ValueDate + 24 hours
```

## sum_avghour_dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns accumulated average hour values considered from  the requested report date shifted backward with **N** days and forward with **M** hours till last hour of the the requested report date. **N** and **M** are numbers without leading zero.

```text
ValueDate= ReportDate(0h) – Ndays + Mhours

Sampletype:         Snapshot
SampleFrequency:    60s
ReductionFrequency: 3600s
ReductionType:      Average
ReductionOffset:    After
UseSampleFrequency: False
BeginTime:          ValueDate
EndTime:            ReportDate last hour
```

## sum_avgmin_dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns accumulated average minute values considered from  the requested report date shifted backward with **N** days and forward with **M** hours till last miniute of the the requested report date. **N** and **M** are numbers without leading zero.

```text
ValueDate= ReportDate(0h) – Ndays + Mhours

Sampletype:         Snapshot
SampleFrequency:    1s
ReductionFrequency: 60s
ReductionType:      Average
ReductionOffset:    After
UseSampleFrequency: False
BeginTime:          ValueDate
EndTime:            ReportDate last minute
```

## sum_avg_dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns accumulated average values considered from  the requested report date shifted backward with **N** days and forward with **M** hours till last miniute of the the requested report date. **N** and **M** are numbers without leading zero.

`ValueDate= ReportDate(0h) – Ndays + Mhours`

> The result is:
> `sum_avghour_dN_hM + sum_avgmin_d0_hL/60` , where **L** is last hour of the requested report date.
The first part accumulates average hour values till the last hour of the requested report date with function `sum_avghour_dN_hM` and the second part accumulates average minute values considered from the last hour of the report date till the last minute of the requested report date with function `sum_avgmin_d0_hL`  and divided by **60** (considered that requested parameter unit is per hour).

## avghour_dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns average hour value considered from  the requested report date shifted backward with **N** days and forward with **M-1** hours. **N** and **M** are numbers without leading zero.

```text
ValueDate= ReportDate(0h) – Ndays + (M-1)hours

Sampletype:         Snapshot
SampleFrequency:    60s
ReductionFrequency: 3600s
ReductionType:      Average
ReductionOffset:    After
UseSampleFrequency: False
```

## maxhour_ dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns maximum hour value considered from  the requested report date shifted backward with **N** days and forward with **M-1** hours. **N** and **M** are numbers without leading zero.

```text
ValueDate= ReportDate(0h) – Ndays + (M-1)hours

Sampletype:         Snapshot
SampleFrequency:    60s
ReductionFrequency: 3600s
ReductionType:      Maximum
ReductionOffset:    After
UseSampleFrequency: False
```

## sum_beg_mon

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns accumulated value from the beginning of the month of the requested report date to the requested report date.

## sum_beg_mon_bo_dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns accumulated value from the beginning of the month of the requested report date shifted forward with **N** days and **M** hours till requested report date.  **N** and **M** are numbers without leading zero.

## sum_beg_mon_dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns accumulated value from the beginning of the month of the requested report date to the **N** days and **M** hours ahead considered from the last day **0h** of the previous month.  **N** and **M** are numbers without leading zero.

## sum_beg_mon_eod

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns accumulated value from the beginning of the month of the requested report date till the requested report date shifted with **1** day forward. 

## sum_beg_mon_eod_bo_dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns accumulated value from the beginning of the month of the requested report date shifted forward with **N** days and **M** hours till requested report date shifted forward with **1** day.  **N** and **M** are numbers without leading zero.

## begin_week

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns data value for the beginning of the week of the requested report date.

## sum_beg_week

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns accumulated value from the beginning of the week of the requested report date to the requested report date.

## sum_beg_week_bo_dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns accumulated value from the beginning of the week of the requested report date shifted forward with **N** days and **M** hours till requested report date. **N** and **M** are numbers without leading zero.

## sum_beg_week_eod

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns accumulated value from the beginning of the week of the requested report date till the requested report date shifted with **1** day forward. 

## sum_beg_week_eod_bo_dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns accumulated value from the beginning of the week of the requested report date shifted forward with **N** days and **M** hours till requested report date shifted forward with **1** day.  **N** and **M** are numbers without leading zero.

## week_day _dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns value for the requested report date previous week last day **0h** shifted forward with **N** days and **M** hours. **N** and **M** are numbers without leading zero.

`ValueDate = ReportDatePrevWeekLastDay(0h) + Ndays + Mhours`

## sum_beg_week_dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns accumulated value from the beginning of the week of the requested report date to the **N** days and **M** hours ahead considered from the last day **0h** of the previous week.  **N** and **M** are numbers without leading zero.

## week_day_last_val_dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns value for the requested report date previous week last day **0h** shifted forward with **N** days and **M** hours. **N** and **M** are numbers without leading zero. Tags are separated with "**,**" and returns the value of the tag with the latest timestamp.

`ValueDate = ReportDatePrevWeekLastDay(0h) + Ndays + Mhours`

## week_avg_day_dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns average value considered from the requested report date previous week last day **0h** shifted forward with **N-1** days and **M** hours. **N** and **M** are numbers without leading zero.

```text
Sampletype:         Snapshot
SampleFrequency:    1s
ReductionFrequency: 86400s
ReductionType:      Average
ReductionOffset:    After
UseSampleFrequency: False
```

## week_avg_dN_hM

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns average week value considered from the requested report date **0h** shifted backward with **N+7** days and forward **M** hours. **N** and **M** are numbers without leading zero.

```text
ValueDate= ReportDate(0h) – (N+7)days + Mhours

Sampletype:         Snapshot
SampleFrequency:    60s
ReductionFrequency: 604800s
ReductionType:      Average
ReductionOffset:    After
UseSampleFrequency: False
```

## Tolerance

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns the tolerance of the PHD tag, regardless if the field is called with raw or snapshot data.

## ToleranceType

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns the tolerance type of the PHD tag, regardless if the field is called with raw or snapshot data.

## Tag properties

> **Part:** `part3`  
> **Function:** Data operations  
> **Description:** Returns the requested property of the **PHD** tag, regardless if the field is called with raw or snapshot data.  
> **Available properties: **
*Tagno, TagName, Description, Units, Tolerance, ToleranceType, 
CollectionEnable, DemandCalculationEnable, ManualInputEnable,DownloadEnable,
StoreEnable, EditEnable, ArchiveResampleEnable, SourceSystem, SourceTagname,
SourceTagtype, SourceAttribute, SourceUnits, SourceCollector, ScanFrequency,
ScanUnit, SourceIndexA, SourceIndexB, SourceIndexC, SourceIndexD, RemoteTag,
DataType, DataSize, HighExtreme, LowExtreme, Quantum, HiHiLimit, HiLimit, 
LoLimit, LoLoLimit, HiHiEnable, HiEnable, LoEnable, LoLoEnable, 
FilterConstant, CompressionToleranceFactor, MinimumCompression, SigmaLimit, 
SigmaSamples, GateLevel, PercentFill, QueueSize, ExtrapolationDampIntervals,
ResampleMethod, InterpolationMethod, ParentTagname, ParentTagno, LinkName, 
PointName, ParameterName, TagSyncEnable, TagSyncRuleName, AssetName, 
ItemName, EnumEnable*

__Example:__

`$$phd#raw#Units#TI301_63.PV`


## PHD Tagname

> **Part:** `part4`  
> **Function:** PHD Tagname
> **Description:** The name of a valid tag from the PHD server. 

__Example:__

`$$phd#snapshot#month_day _d3_h14#TI301_63.PV`

This cell will return a snapshot of the value of the tag "**TI301_63.PV**" for the period **00-14h** of the **3rd** day on the month for which the report is called.


*``Remark:`` If a phrase syntax is wrong then the phrase will return
"**StringError**" in the generated report. These strings are configured
in the configuration file and can be changed. "**StringBadValue**" is
returned if the confidence of the requested data is invalid. The default
strings are:*

  Returned string in reports   |Error string
  ---------------------------- |--------------
  StringNotAvailable           |\#NA
  StringBadValue               |\#BAD
  StringError                  |\#ERR
  StringConnectionError        |\#CONN  
