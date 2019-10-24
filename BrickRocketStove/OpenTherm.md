# OpenTherm

## Overview

Boiler control using a current loop with galvanic isolation.

The boiler current source sends 7mA or 20mA to the thermostat, while the thermostat modulates the voltage drop to send data back to the boiler. The result (if my eyes are working) looks like full duplex communication over twisted pair. It might be possible to hook these interfaces to a UART RX/TX, but the data is none standard, so probably that is not helpful.


# Thermostat

The thermostat interface behaves like an OpenTherm Room Unit / Master device. It can modulate the voltage to send data and sense the remote currnt source to receive data.


# Boiler 

The boilder interface behaves like an OpenTherm Boiler Unit / Slave device. It can adjust the current source on the wire loop, and sense the voltage from the thermostate.


# Gateway

Place a gateway between the boiler and thermostat.

http://otgw.tclcode.com/schematic.html

As an Arduino shield.

https://github.com/jpraus/arduino-opentherm

A gateway has a thermostat interface that connects to the boiler, and a boiler interface that connects to a thermostat. Don't over think it; just realize that it goes between the boiler and the thermostat. 


# Boiler Controller

If I want to do a boiler controller for my stove, it may be useful to have the thermostat interface.

