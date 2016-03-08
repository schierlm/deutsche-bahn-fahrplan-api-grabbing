DB Fahrplan API Grabbing
========================

This repository contains aggregated results from [Deutsche Bahn Fahrplan API](http://data.deutschebahn.com/apis/fahrplan/).

NO WARRANTY; first because the API does not provide any, and second because I might have made mistakes during grabbing.


stations.tsv
------------

This file contains a list of all stations returned or understood by the API. These flags are used in the first column:

- ***N***: Station is returned by the Name search (`location.name`)
- ***D***: Station has a (non-empty) departure board
- ***R***: Station is returned in journey details (note that some stations are included in the results but do neither have a departure board themselves nor are supported by the Name search)
- ***G***: Station has been chosen for grabbing every day from April to September, in the expectation that this finds all trains available (if I missed some here, please tell me)

Each station contains only the data returned by the API (ID, name, latitude, longitude).
Implicitly included is the [UIC country code](https://en.wikipedia.org/wiki/List_of_UIC_country_codes), which consists
of the first two digits of the ID, and usually indicates the location of the station 
([8000026 Basel Bad Bf](https://en.wikipedia.org/wiki/Basel_Badischer_Bahnhof) being one of the exceptions).

More data may be available in the CSV file available [in the OpenData portal](http://data.deutschebahn.com/datasets/haltestellen/).


opencage.tsv
------------

Unique list of geo coordinates from `stations.tsv` requested from the OpenStreetMaps related OpenCage geocoder.
Contains ISO2 code, timezone, state, zipcode, city.


timezones.tsv
-------------

As the API only returns local time, duration calculation is hard without timezones. This file contains a minimal mapping file
(valid for all the stations known in stations.tsv) to determine timezone from station id.

It is based on the rationale that most countries in Europe (in fact, of all the returned countries all except Russia) only
have one time zone, and that the UIC code is a good indicator for time zone. Therefore, the general mapping is based on UIC code.

Exceptions, as well as Russian stations, are individually listed in the file. As of now, there are only 14 exceptions.



trains.tsv
----------

Contains one line for every train number seen during grabbing (so a good way to find out where ICE 123 is going to).
In addition, there is also a day index included that can be used with traindays.tsv to find on which days (between April and September)
this train has been seen, some flags, and a sample routing. These flags are used:

- ***T***: This train has been seen to travel at different times (not every day the same time)
- ***P***: This train has been seen to use different platforms (not every day the same platform)
- ***p***: This train sometimes has platform info, sometimes not (but if it has, the platform info has been seen consistent).

The routing consists of a comma separated list of `arrival|stationid[platform]|departure`
for each station. For easier parsing, there is also a normalized version available in the file below.


trainroutings.tsv
-----------------

Normalized version of the routings of `trains.tsv`, each stop is on its own line, consisting of train name, index, station, platform,
departure and arrival times (and optional day offset).


traindays.tsv
-------------

Lookup for the train day index in `trains.tsv`. For every day between April and September, a `#` indicates the train has been seen on that day
(days are always based on the first departure in case the train spans multiple days), and `-` indicates that it has not.


Last but not least:
-------------------

Have fun `:-)`
