# Forked version
This is a forked version from [Xorfor/Domoticz-PWS-Plugin](https://github.com/Xorfor/Domoticz-PWS-Plugin)

# Changes
* Higher resolution baro sensor
* Indoor Temperature and humidity sensor


# Domoticz PWS Plugin
This Domoticz Plugin allows you to get the data **directly** from your own personal weather station (PWS). So this plugin does **NOT** require that you register your PWS to cloud accounts, like WeatherUnderground (support will finish in the near future!), Ecowitt (also displays your indoor sensors in the cloud!), WeatherCloud, WOW (takes weeks to get key), etc, or the use of WeeWX (extra software).

**This plugin will directly capture the data from your weather station!** 

## Supported devices
In general, if the station is supplied with `EasyWeather` software (version 1.4.x, 1.5.x), it is likely that the station will work with this Domoticz plugin!

### Tested

* Alecto WS-5500
* Garni 940
* Garni 2055 Arcus
* Sainlogic WS3500
* Ventus W830
* Waldbeck Halley Professional Weather Station

#### EasyWeather
* 1.4.x
* 1.5.x
* 1.6.1 up to 1.6.7

## Prerequisites
Your PWS needs to be connected to your network. This can be done by using [WS View](#ws-view-ws-tool) or the [Garni Technology](#garni-not-supported) app.

### WS View (WS Tool)
If supported by your PWS, connect your PWS with `WS View Plus` (and also the 'older' `WS View`) to your router by wifi, so that your PWS can upload weather data to Domoticz.

1. Install `WS View Plus` on your mobile device
    * [Google Play Store](https://play.google.com/store/apps/details?id=com.ost.wsautool)
    * [Apple App Store](https://apps.apple.com/nl/app/wsview-plus/id1581353359)
1. , or `WS View`
    * [Google Play Store](https://play.google.com/store/apps/details?id=com.ost.wsview)
    * [Apple App Store](https://apps.apple.com/us/app/ws-view/id1362944193)
1. Follow the instructions to connect your PWS to your router
1. Goto to Device List in Menu and choose your PWS
1. Click on Next untill you are on on the `Customized` page
1. Choose `Enable`
1. For `Protocol Type Same As` choose `Wunderground` (preferred)
1. For `Server IP / Hostname` enter your Domoticz Server ip address, eg. 192.168.0.10
1. If you choose for `Wunderground` protocol:
    * Fill in `Station ID` with a value
    * Fill in `Station Key` with a value
1. `Port` enter a free port number, eg. `5000`
1. `Upload Interval`, leave it `60` seconds
1. Click on `Save`

![Screenshot](/images/screendump2.png) ![Screenshot](/images/screendump3.png)

Now your PWS will start to upload its data to your Domoticz server at the specified port. 

### Garni (NOT SUPPORTED)
Some users of this plugin reported that also a Garni tool can be used with the Garni weather stations. **I was not able to test this**, but the instructions are something like:

1. Install `Garni technology` on your mobile device
    * [Google Play Store](https://play.google.com/store/apps/details?id=com.garnitechnology.app)
1. Follow the instructions to connect your PWS to your router
1. In Weather Server Setup:
    * Fill in `URL` your Domoticz Server ip address, eg. 192.168.0.10
    * Fill in `Station ID` with a value
    * Fill in `Station key` with a value
1. As Garni sends its data to port 80, which can be used by other webservices (Domoticz uses 8080 by default), you have to route the data to another port by:
    * Linux (Raspberry PI, etc) 
        * `iptables -t nat -A PREROUTING -s [IP of PWS] -p tcp --dport 80 -j DNAT --to-destination [IP of Domoticz]:5000`
    * Windows
        * `netsh interface portproxy add v4tov4 connectaddress=[IP of PWS] connectport=80 listenaddress=[IP of Domoticz] listenport=5000` NOT TESTED YET!!!

Now your Garni PWS will start to upload its data to your Domoticz server at port `5000`.

### ObserverIP
Not implemented yet!!!

## Configure Domoticz plugin
Next step is to install the Domoticz plugin. This plugin will automatically create the required devices, listen to the specified port, retrieve the data and update the devices with the latest information.

Unfortunately you can connect your PWS only to **one** Domoticz server!

### Installation
1. Clone repository into your Domoticz plugins folder
    ```
    cd domoticz/plugins
    git clone https://github.com/Xtremernl/Domoticz-PWS-Plugin.git
    ```
1. Restart domoticz
    ```
    sudo service domoticz.sh restart
    ```
1. Make sure that "Accept new Hardware Devices" is enabled in Domoticz settings
1. Go to "Hardware" page and add new hardware with Type "PWS"
1. Enter the Port number as used in WS View
1. Press Add

### Update
1. Go to plugin folder and pull new version
    ```
    cd domoticz/plugins/Domoticz-PWS-Plugin
    git pull
    ```
1. Restart domoticz
    ```
    sudo service domoticz.sh restart
    ```

### Parameters
| Name     | Description
| :---     | :---
| **Port** | Port number as choosen in WS View or in the Garni setup, eg. `5000`

The selected port is displayed on Hardware overview as Address.

## Devices
![Devices](/images/screendump.jpg)

I have created as much devices as possible, so you can select your own favourites.

| Name                     | Description
| :---                     | :---
| **Barometer (absolute)** | Pressure (absolute) in hPa
| **Barometer (relative)** | Pressure (relative) in hPa
| **Chill**                | Chill (calculated when `Ecowitt` protocol is used)
| **Dew point**            | Dew point (calculated when `Ecowitt` protocol is used)
| **Gust**                 | Gust
| **Heat index**           | Heat index (calculated based on temperature and humidity)
| **Humidity**             | Humidity
| **Humidity (indoor)**    | Humidity (indoor)
| **Rain**                 | Current rain rate and daily total
| **Rain rate**            | Current rain rate
| **Station**              | Format: [ip adress] ([software]): [Protocol] (`Wunderground` or `Ecowitt`), from your PWS.
| **Soilmoisture**         | Soil moisture
| **Solar radiation**      | Solar radiation W/m<sup>2</sup>
| **Solar radiation**      | Solar radiation Lux
| **Temp + Hum**           | Temperature and humidity
| **Temp + Hum (indoor)**  | Temperature and humidity (indoor)
| **Temperature**          | Temperature
| **Temperature (indoor)** | Temperature (indoor)
| **THB**                  | Temperature, humidity and barometer (pressure and prediction)
| **UVI**                  | UV index
| **UV Alert**             | UV index + warning level (calculated)
| **Wind**                 | Wind direction, speed and gust
| **Wind**                 | Wind direction, speed, gust, temperature and gust
| **Wind direction**       | Wind direction
| **Wind Speed**           | Wind speed

## Protocols
WS View supports 2 protocols for `Customized` upload: `Wunderground` or `Ecowitt`. My information about the data to be uploaded is based on my own experience and information from:

### Wunderground
Information can be found at: https://support.weather.com/s/article/PWS-Upload-Protocol?language=en_US.

#### Not supported (yet)
| Name                   |
| :---                   |
| weather                |
| soiltempf              |
| leafwetness            |
| visibility             |
| Aq* (like AqNO, AqBC ) |

#### Not used
The following data send by the weatherstations is not used, e.g. because this is historical data which is already maintained by Domoticz, or not relevant as sensor:

| Name                |
| :---                |
| action              |
| ID                  |
| monthlyrainin       |
| PASSWORD            |
| realtime            |
| rtfreq              |
| weeklyrainin        |

### Ecowitt
Information not found yet.
