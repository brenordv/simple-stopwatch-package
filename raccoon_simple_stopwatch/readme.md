# Simple Stopwatch
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=brenordv_simple-stopwatch-package&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=brenordv_simple-stopwatch-package)
[![Maintainability Rating](https://sonarcloud.io/api/project_badges/measure?project=brenordv_simple-stopwatch-package&metric=sqale_rating)](https://sonarcloud.io/summary/new_code?id=brenordv_simple-stopwatch-package)
[![Reliability Rating](https://sonarcloud.io/api/project_badges/measure?project=brenordv_simple-stopwatch-package&metric=reliability_rating)](https://sonarcloud.io/summary/new_code?id=brenordv_simple-stopwatch-package)
[![Security Rating](https://sonarcloud.io/api/project_badges/measure?project=brenordv_simple-stopwatch-package&metric=security_rating)](https://sonarcloud.io/summary/new_code?id=brenordv_simple-stopwatch-package)
[![Coverage](https://sonarcloud.io/api/project_badges/measure?project=brenordv_simple-stopwatch-package&metric=coverage)](https://sonarcloud.io/summary/new_code?id=brenordv_simple-stopwatch-package)

[![Bugs](https://sonarcloud.io/api/project_badges/measure?project=brenordv_simple-stopwatch-package&metric=bugs)](https://sonarcloud.io/summary/new_code?id=brenordv_simple-stopwatch-package)
[![Code Smells](https://sonarcloud.io/api/project_badges/measure?project=brenordv_simple-stopwatch-package&metric=code_smells)](https://sonarcloud.io/summary/new_code?id=brenordv_simple-stopwatch-package)
[![Duplicated Lines (%)](https://sonarcloud.io/api/project_badges/measure?project=brenordv_simple-stopwatch-package&metric=duplicated_lines_density)](https://sonarcloud.io/summary/new_code?id=brenordv_simple-stopwatch-package)
[![Lines of Code](https://sonarcloud.io/api/project_badges/measure?project=brenordv_simple-stopwatch-package&metric=ncloc)](https://sonarcloud.io/summary/new_code?id=brenordv_simple-stopwatch-package)
[![Technical Debt](https://sonarcloud.io/api/project_badges/measure?project=brenordv_simple-stopwatch-package&metric=sqale_index)](https://sonarcloud.io/summary/new_code?id=brenordv_simple-stopwatch-package)
[![Vulnerabilities](https://sonarcloud.io/api/project_badges/measure?project=brenordv_simple-stopwatch-package&metric=vulnerabilities)](https://sonarcloud.io/summary/new_code?id=brenordv_simple-stopwatch-package)


This package is a simple stopwatch implementation I use on several projects and got tired of copying and pasting this
code all around.

The default elapsed time format is `DAYS.HOURS:MINUTES:SECONDS.MS`



# Installation
```shell
pip install -U raccoon-simple-stopwatch
```



# Examples
## 01. Creating new stopwatch

```python
from raccoon_simple_stopwatch.stopwatch import StopWatch

sw = StopWatch()
```
The snippet above shows how to instantiate a StopWatch. Creating an instance like this will not start the stopwatch. 


## 02. Creating new stopwatch

```python
from raccoon_simple_stopwatch.stopwatch import StopWatch

sw = StopWatch(True)

# or
sw2 = StopWatch(auto_start=True)
```
The snippet above shows how to instantiate a StopWatch. Creating an instance like this WILL autostart the stopwatch.


## 03. Getting the final elapsed time

```python
import time
from raccoon_simple_stopwatch.stopwatch import StopWatch

sw = StopWatch(True)
time.sleep(10)
elapsed_time = sw.end()
print(elapsed_time)
```
Output:
```text
0:00:10.001899
```
This snippet auto starts the stopwatch, waits for 10 seconds, stops it and then gets the total elapsed time.


## 04. Getting current/partial elapsed time

```python
import time
from raccoon_simple_stopwatch.stopwatch import StopWatch

sw = StopWatch(True)
for i in range(10):
    print(f"[{i:02d}] Partial: {sw.elapsed()}")
    time.sleep(1)
elapsed_time = sw.end()
print(f"Total Elapsed time: {elapsed_time}")
```
Output:
```text
[00] Partial: 0:00:00.000010
[01] Partial: 0:00:01.008056
[02] Partial: 0:00:02.020088
[03] Partial: 0:00:03.030782
[04] Partial: 0:00:04.040916
[05] Partial: 0:00:05.054144
[06] Partial: 0:00:06.062206
[07] Partial: 0:00:07.075621
[08] Partial: 0:00:08.088999
[09] Partial: 0:00:09.100672
Total Elapsed time: 0:00:10.115585
```
This snippet auto starts the stopwatch, gets the partial elapsed time 10 times during 10 seconds, stops it and then gets the total elapsed time.

## 05. Getting current/partial elapsed time in timedelta format

```python
import time
from raccoon_simple_stopwatch.stopwatch import StopWatch

sw = StopWatch(True)
for i in range(10):
    print(f"[{i:02d}] Partial: {sw.elapsed(True).total_seconds()}")
    time.sleep(1)
elapsed_time = sw.end(True).total_seconds()
print(f"Total Elapsed time: {elapsed_time}")
```
Output:
```text
[00] Partial: 1.5e-05
[01] Partial: 1.009903
[02] Partial: 2.022595
[03] Partial: 3.033897
[04] Partial: 4.046012
[05] Partial: 5.056192
[06] Partial: 6.068632
[07] Partial: 7.068943
[08] Partial: 8.084072
[09] Partial: 9.097145
Total Elapsed time: 10.111292
```
This works just like the previous snippet, but the elapsed time is retrieved in timedelta format, 
and then we call `total_seconds()` functions to get the elapsed time in seconds.


## 06. Getting the moment when the stopwatch started (UTC and local)

```python
from raccoon_simple_stopwatch.stopwatch import StopWatch

sw = StopWatch()  # Local datetime
print(sw.start_datetime)
sw.start()
print(f"{sw.start_datetime} | {sw.start_datetime.tzinfo}")
sw2 = StopWatch(use_utc=True)  # UTC datetime
print(sw2.start_datetime)
sw2.start()
print(f"{sw2.start_datetime} | {sw2.start_datetime.tzinfo}")
```
Output:
```text
None
2021-12-23 20:43:50.585377 | None
None
2021-12-23 23:43:50.585377 | None
```

> Note: All examples involving **start_datetime** can also be applied to **stop_datetime**.

## 07. Getting the moment when the stopwatch started, making the datetime timezone aware.

```python
from raccoon_simple_stopwatch.stopwatch import StopWatch

sw = StopWatch(use_utc=True, make_tz_aware=True)
print(sw.start_datetime)
sw.start()
print(f"{sw.start_datetime} | {sw.start_datetime.tzinfo}")
sw2 = StopWatch(make_tz_aware=True)
print(sw2.start_datetime)
sw2.start()
print(f"{sw2.start_datetime} | {sw2.start_datetime.tzinfo}")
```
Output:
```text
None
2021-12-23 23:41:56.415212+00:00 | UTC
None
2021-12-23 20:41:56.415212-03:00 | E. South America Standard Time
```



# Useful links
- https://github.com/brenordv/simple-stopwatch-package
- https://pypi.org/project/raccoon-simple-stopwatch