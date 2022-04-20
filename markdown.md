Markdown/API Project
=====================

This repository contains the source code for the GetMyWeather API. For more information, consult our [site](http://www.getmyweather.com/getWeather).

## Table of Contents

+ [What is GetMyWeather?](#api)
    + [Requirements](#req)
+ [What is the getWeather method?](#get)
    + [Using getWeather parameters](#param)
    + [Interpreting the getWeather return package](#interpret)
+ [How do I use getWeather?](#use)
    + [Initializing getWeather](#init)
    + [Calling getWeather](#call)
    + [Returning getWeather](#return)
+ [Errors](#error)


## <a name="api"></a> What is GetMyWeather?
GetMyWeather is a simple API, with one method (getWeather) and four input parameters (Location, Specificity, Time, Key). Developers can use the GetMyWeather API to embed "micro-forecasts" (weather forecasts for a very specific location) into an HTML page. 

### <a name="req"></a> Requirements for use
* **API key** Register for an API key at https://www.getmyweather.com. Cost depends on usage; with details available on the website. A no-cost key is available for application development and testing.
* **Browser** GetMyWeather is supported by all modern browsers that support Javascript. 
* **Javascript** Javascript must be enabled to use GetMyWeather.

## <a name="get"></a> What is the getWeather method?

The getWeather method is called for each forecast. Given four valid inputs (location, specificity, time, key), the method returns four values (temperature, windspeed, chanceRain, trust) for a micro-forecast. This may happen multiple times in a session, as the web application user may request forecasts with different parameters.

The getWeather method returns a static HTML package that you can process with Javascript and/or style with CSS. To change the parameters of the forecast, you must invoke getWeather again with the new parameter values. 

### <a name="param"></a> Using getWeather parameters
* **Location** Location expressed as lattitude/longitude, in ISO 6709 format.
* **Specificity** Defines the area of the forecast as a circle around the Location. Expressed as a radius, in meters. Minimum value is 10 meters; maximum is 100,000 meters.
* **Time** Time in the future for which the forecast is requested. Expressed in hours. Decimals are allowed. Minimum value is 0 hours; maximum is 280 hours.
* **Key** API key, provided by GetMyWeather upon registration to use the API.

A visual representation of the location format and specificity are shown below:

![ex](/Assets/input_ex.png)

### <a name="interpret"></a> Interpreting the getWeather return package
Once the getWeather method is successfully called with valid inputs, the getWeather method returns four values. Each of the values are listed and described below.
* **temperature** The temperature value indicates the forecasted temperature in the time and location specified. The value is represented in Fahrenheit.
* **windspeed** The windspeed value indicates the forecasted wind speed in the time and location specified. The value is represented in MPH (miles per hour)
* **chanceRain** The chanceRain value indicates the forecasted likelihood of rain in the time and location specified. The value is represented as a percentage.
* **trust** The Trust value indicates the reliability of the forecast and is a number between 1 and 100 (1 is very unreliable; 100 is absolutely reliable). Large values of Specificity and/or Time will make the forecast less reliable. Forecasts of greater than 240 hours (10 days) in the future are generally unreliable. A Trust value of 0-10 indicates that the forecast is based on historical averages for the location, not on anticipated weather patterns.

## <a name="use"></a> How do I use getWeather?

Include this empty element in your HTML page. Your browser will insert the getWeather return package here. 

```javascript
<div id="forecast"></div>
```

The following sections will provide code snippets to initialize and call the getWeather method in order to receive its output.

### <a name="init"></a> Initializing getWeather

The GetMyWeather API needs to be initialized just once for a web session. This establishes a connection between the host web server and the GetMyWeather server. Insert this code snippet in the relevant code base to initialize the API.

```javascript
<script type="text/javascript">
var weatherForecast;
function init() {
weatherForecast =
new getWeather(document.getElementById('forecast')}
</script>
```

### <a name="call"></a> Calling getWeather

For each forecast, call the getWeather method with valid parameters with the following code snippet. 

```javascript
<script async defer
src="https://www.getmyweather.com/getWeather?
key=<YOUR_KEY>&
callback=init&
location=<LATITUDE>:<LONGITUDE>&
specificity=<SPECIFICITY>&
time=<TIME>">
</script>
```

Note: Each of `YOUR_KEY`, `LATITUDE`, `LONGITUDE`, `SPECIFICITY`, and `TIME` should be replaced with the valid parameters.

### <a name="return"></a> Returning getWeather 
Upon success, the getWeather method returns the following HTML structure (values are samples).

```javascript
<div class="forecast">
    <div class="temperature">78</div>
    <div class="windspeed">15</div>
    <div class="chanceRain">30</div>
    <div class="trust">80</div>
</div>
```

Note: Time between invoking the getWeather method and the return package is negligible. Most web applications will not require a GUI indication that the application is waiting for a result.

## <a name="error"></a> Errors
The getWeather method may return the following error strings if it cannot provide a forecast:
* **"Invalid key"** 
* **"Invalid parameter format"**
* **"Parameter out of range"**
* **"Server unavailable"**
