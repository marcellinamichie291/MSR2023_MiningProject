# IP2Location 8.9.0


This is a IP2Location Python library that enables the user to find the country, region or state, city, latitude and longitude, ZIP code, time zone, Internet Service Provider (ISP) or company name, domain name, net speed, area code, weather station code, weather station name, mobile country code (MCC), mobile network code (MNC) and carrier brand, elevation, usage type, address type and IAB category by IP address or hostname originates from. The library reads the geo location information from **IP2Location BIN data** file.

Supported IPv4 and IPv6 address.

For more details, please visit:
[https://www.ip2location.com/developers/python](https://www.ip2location.com/developers/python)

# Requirements

1. Python 3.5 and above.

# Installation

1. Unzip the package.
2. Execute python setup.py build
3. Execute python setup.py install

or

To install this module type the following (for PyPI):

```bash
pip install IP2Location
```

# Usage

You can check the **sample.py** file to learn more about usage.

## BIN Database

Below is the description of the functions available in the **BIN Database** lookup.

| Method Name       | Description                                                  |
| ----------------- | ------------------------------------------------------------ |
| open              | Open the IP2Location BIN data for lookup. Default mode: File I/O. |
| close             | Close and clean up the file pointer.                         |
| get_all           | Return the geolocation information in array.                 |
| get_country_short | Return the ISO3166-1 country code (2-digits) of the IP address. |
| get_country_long  | Return the ISO3166-1 country name of the IP address.         |
| get_region        | Return the ISO3166-2 region name of the IP address. Please visit [ISO3166-2 Subdivision Code](https://www.ip2location.com/free/iso3166-2) for the information of ISO3166-2 supported |
| get_city          | Return the city name of the IP address.                      |
| get_latitude      | Return the city latitude of the IP address.                  |
| get_longitude     | Return the city longtitude of the IP address.                |
| get_isp           | Return the ISP name of the IP address.                       |
| get_domain        | Return the domain name of IP address.                        |
| get_zipcode       | Return the zipcode of the city.                              |
| get_timezone      | Return the UTC time zone (with DST supported).               |
| get_netspeed      | Return the Internet connection type. Please see [Internet Connection Type](https://github.com/[chrislim2888/IP2Location-Python](https://github.com/chrislim2888/IP2Location-Python)#internet-connection-type) for details. |
| get_idd_code      | Return the IDD prefix to call the city from another country. |
| get_area_code     | Return the area code of the city.                            |
| get_weather_code  | Return the nearest weather observation station code.         |
| get_weather_name  | Return the nearest weather observation station name.         |
| get_mcc           | Return the Mobile Country Codes (MCC).                       |
| get_mnc           | Return the Mobile Network Code (MNC).                        |
| get_mobile_brand  | Commercial brand associated with the mobile carrier. Please visit [Mobile Carrier Coverage](https://www.ip2location.com/mobile-carrier-coverage) to view the coverage report. |
| get_elevation     | Return average height of city above sea level in meters (m). |
| get_usage_type    | Return the ISP's usage type of IP address. Please see [Usage Type](https://github.com/[chrislim2888/IP2Location-Python](https://github.com/chrislim2888/IP2Location-Python)#usage-type) for details. |
| get_address_type  | Return the IP address type (A-Anycast, B-Broadcast, M-Multicast & U-Unicast) of IP address or domain name. |
| get_category      | Return the IAB content taxonomy category of IP address or domain name. You can get a full list of IAB content taxonomy category from [here](https://www.ip2location.com/free/iab-categories). |

## Web Service

Below is the description of the functions available in the **Web Service** lookup.

| Function Name | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| Constructor   | Expect 3 input parameters:<ol><li>IP2Location API Key.</li><li>Package (WS1 - WS25)</li><li>Use HTTPS or HTTP</li></ol> |
| lookup        | Return the IP information in array.  <ul><li>country_code</li><li>country_name</li><li>region_name</li><li>city_name</li><li>latitude</li><li>longitude</li><li>zip_code</li><li>time_zone</li><li>isp</li><li>domain</li><li>net_speed</li><li>idd_code</li><li>area_code</li><li>weather_station_code</li><li>weather_station_name</li><li>mcc</li><li>mnc</li><li>mobile_brand</li><li>elevation</li><li>usage_type</li><li>address_type</li><li>category</li><li>category_name</li><li>continent<ul><li>name</li><li>code</li><li>hemisphere</li><li>translations</li></ul></li><li>country<ul><li>name</li><li>alpha3_code</li><li>numeric_code</li><li>demonym</li><li>flag</li><li>capital</li><li>total_area</li><li>population</li><li>currency<ul><li>code</li><li>name</li><li>symbol</li></ul></li><li>language<ul><li>code</li><li>name</li></ul></li><li>idd_code</li><li>tld</li><li>translations</li></ul></li><li>region<ul><li>name</li><li>code</li><li>translations</li></ul></li><li>city<ul><li>name</li><li>translations</li></ul></li><li>geotargeting<ul><li>metro</li></ul></li><li>country_groupings</li><li>time_zone_info<ul><li>olson</li><li>current_time</li><li>gmt_offset</li><li>is_dst</li><li>sunrise</li><li>sunset</li></ul></li><ul> |
| get_credit    | Return remaining credit of the web service account.          |

### IP Tools

Below is the description of the functions available in the **IP Tools** class.

| Function Name   | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| is_ipv4         | Return either **true** or **false**. Verify if a string is a valid IPv4 address. |
| is_ipv6         | Return either **true** or **false**. Verify if a string is a valid IPv6 address. |
| ipv4_to_decimal | Translate IPv4 address from dotted-decimal address to decimal format. |
| decimal_to_ipv4 | Translate IPv4 address from decimal number to dotted-decimal address. |
| ipv6_to_decimal | Translate IPv6 address from hexadecimal address to decimal format. |
| decimal_to_ipv6 | Translate IPv6 address from decimal number into hexadecimal address. |
| ipv4_to_cidr    | Convert IPv4 range into a list of IPv4 CIDR notation.        |
| cidr_to_ipv4    | Convert IPv4 CIDR notation into a list of IPv4 addresses.    |
| ipv6_to_cidr    | Convert IPv6 range into a list of IPv6 CIDR notation.        |
| cidr_to_ipv6    | Convert IPv6 CIDR notation into a list of IPv6 addresses.    |
| compressed_ipv6 | Compress a IPv6 to shorten the length.                       |
| expand_ipv6     | Expand a shorten IPv6 to full length.                       

### Country Class

Below is the description of the functions available in the **Country** class.

| Function Name                          | Description                                                  |
| -------------------------------------- | ------------------------------------------------------------ |
| Constructor                            | Expect a IP2Location Country Information CSV file. This database is free for download at https://www.ip2location.com/free/country-information |
| get_country_info | Provide a ISO 3166 country code to get the country information in array. Will return a full list of countries information if country code not provided. Below is the information returned: <ul><li>country_code</li><li>country_alpha3_code</li><li>country_numeric_code</li><li>capital</li><li>country_demonym</li><li>total_area</li><li>population</li><li>idd_code</li><li>currency_code</li><li>currency_name</li><li>currency_symbol</li><li>lang_code</li><li>lang_name</li><li>cctld</li></ul> |

### Region Class

Below is the description of the functions available in the **Region** class.

| Function Name                                       | Description                                                  |
| --------------------------------------------------- | ------------------------------------------------------------ |
| Constructor                                         | Expect a IP2Location ISO 3166-2 Subdivision Code CSV file. This database is free for download at https://www.ip2location.com/free/iso3166-2 |
| get_region_code | Provide a ISO 3166 country code and the region name to get ISO 3166-2 subdivision code for the region. | |

# Testing

    python sample.py
    python test.py
    python lookup.py <ip_address>

# Sample BIN Databases
* Download free IP2Location LITE databases at [https://lite.ip2location.com](https://lite.ip2location.com)  
* Download IP2Location sample databases at [https://www.ip2location.com/developers](https://www.ip2location.com/developers)

# IPv4 BIN vs IPv6 BIN
* Use the IPv4 BIN file if you just need to query IPv4 addresses.
* Use the IPv6 BIN file if you need to query BOTH IPv4 and IPv6 addresses.

# Internet Connection Type

| Internet Connection Type | Description                  |
| ------------------------ | ---------------------------- |
| DIAL                     | Dial Up                      |
| DSL                      | Broadband/Cable/Fiber/Mobile |
| COMP                     | Company/T1                   |

# Usage Type

| Usage Type | Description                     |
| ---------- | ------------------------------- |
| COM        | Commercial                      |
| ORG        | Organization                    |
| GOV        | Government                      |
| MIL        | Military                        |
| EDU        | University/College/School       |
| LIB        | Library                         |
| CDN        | Content Delivery Network        |
| ISP        | Fixed Line ISP                  |
| MOB        | Mobile ISP                      |
| DCH        | Data Center/Web Hosting/Transit |
| SES        | Search Engine Spider            |
| RSV        | Reserved                        |

# Support

Email: support@ip2location.com.  
URL: [https://www.ip2location.com](https://www.ip2location.com)
