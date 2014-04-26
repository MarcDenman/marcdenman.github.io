---
layout: post
title: Spark Core Powered Sous Vide
---


For my first foray into the world of Arduino, in particular the SparkCore (backed from their superb [Kickstarter campagin](https://www.kickstarter.com/projects/sparkdevices/spark-core-wi-fi-for-everything-arduino-compatible)) I decided to try and make a quick and easy "[Sous-Vide](http://en.wikipedia.org/wiki/Sous-vide)" machine.

I have been interested in the concept of Sous-Vide cooking for a little while and with the combination of having an old slow-cooker, a shiny new Spark Core, the [Spark Cloud](http://docs.spark.io/#/start/wait-what-is-this-thing-the-spark-cloud) and the number of tutorials on building a machine around the internet has lead to an appealing and simple project.

The actual components for the project surprised me in just how short it was:

* [Slow Cooker ](http://www.amazon.co.uk/Morphy-Richards-48718-Cooker-Litre/dp/B002NPC0RG)
* [Spark Core](http://spark.io)
* [Relay Shield](http://spark.io)
* [DS18B20](http://www.ebay.co.uk/itm/DS18b20-Waterproof-digital-probe-thermometer-temperature-sensor-thermal-/131111240055) (with an additional 4.7k Resistor)
* Project Box

I have pulled libraries from a couple different places which were ported for use with the Spark Core 

* [PID](https://github.com/br3ttb/Arduino-PID-Library)
* [DS1820](https://github.com/billprozac/ds1820)
* [OneWire](https://github.com/billprozac/ds1820)

There are elements of the code and structure is based from various tutorials around the 'tubes and I have tried to attribute those as I used them.

The GitHub repo for the project is [HERE](https://github.com/MarcDenman/SousVideFirmware)

*** PRETTY PICTURES ***


The code behind the sous-vide machine is based around the [Arduino PID controller library](http://playground.arduino.cc/Code/PIDLibrary). The controller requires several variables; a *SetPoint* (target temperature for the machine), *Input* (current temperature of the machine) as well as three other variables (*Kp*, *Ki*, *Kd*) which controls the speed and aggression of the controller in heating up the machine and as it approaches the SetPoint. These three values are currently hard-coded into the code and require required tuning for the specific slow cooker, though there is a [Arduino PID AutoTune library](http://playground.arduino.cc/Code/PIDAutotuneLibrary) which can generate the va0lues if needed. I am currently using `Kp = 0.1`, `Ki = 150`, `Kd = 0.45` which work fairly well for me, however they could be tuned better in the future. 

###### Creating a new PID controller 

`PID myPID(&Input, &Output, &Setpoint, Kp, Ki, Kd, DIRECT);`


###### Using the PID controller

`myPID.Compute();`


On calling the `Compute()` function the controller uses the various inputs described above to assign a value to *Output* which, in this scenario effectively controls whether the relay is off or on. If the output is above a certain value the relay is turned on, below the relay is turned off. The `Compute()` function gets called manually approximately 10 seconds.



I am currently getting around 2 degree variance, with the *SetPoint* at 65c the lowest temperature is set around 64.5c and then up to 66.5c. The largest issue with the machine at the moment is that it takes a very very long time for the machine to reach the *SetPoint*.



Future Features:

* Pre-heating of machine, wanting to cook at 14:00 with the water at 70c. The machine ensures the water is ready at the correct time. 
* Web API to simplify logging and storing of temperatures.
* Graphing of temperatures.
* Nice [shiny](https://www.youtube.com/watch?v=aFj8eFZx-TA) simple interface to control the machine.

Whilst writing this up I come across [Mellow](http://cookmellow.com/meet-mellow/) which looks eerily similar to what I have been planning to do with my machine with future! I am unbelievably close to ordering one, if only they delivered internationally!