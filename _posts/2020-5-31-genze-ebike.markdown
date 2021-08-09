---
layout: post
title:  "What I learned cracking open my Genze Ebike"
date:   2020-5-31 15:53:00 -0500
---

I have never owned a car and when COVID hit, I bought a used ebike as an alternative to public transit.  Electric bikes have exploded in popularity in recent years as batteries have gotten cheaper giving people like me a way to get around on 2 wheels in an environmentally friendly way without peadaling up hills.

Electric bikes are pretty simple electronic systems, but there isn't much info on what's going on inside.  I have a Genze e101.  It's a pretty standard low price ebike setup.  I bought it online but Costco carried a slightly less powerful version of the same bike. It's made by a large Indian conglomerate Mahindra, and the bikes have been used in rideshare fleets and delivery fleets for postmates.

## Basic ebike components

An ebike typically has a motor, a motor controller, a display, a throttle, and a pedal assist sensor.

Genze e101
Motor: 350W hub motor produced by DAPU.  A 350W motor is enough that you can throttle around at 20mph on flat ground, but have to pedal on most hills.  For me, this is a good balance.
Controller: produced by DAPU
Display and throttle: C600 produced by Bigstone.
Pedal assist sensor: A torque sensor, also produced by DAPU

## Adding headlights and taillights
I wanted to wire in headlights and taillights into the ebike.  Many options are available that work with a range of voltages.

The controller often has an additional connection for external power that may have a resistor wired between the controller and a JST connector.  On my controller, the controller provides power here even when the display is off.

Controller references
https://pedegogo.zendesk.com/hc/en-us/articles/203061346-Controller-Diagrams

## Speed limits
US law specifies 3 classes of ebikes that dictate their top speed.  My bike, a class 2 ebike is software limited to 20mph.  Meaning, no matter how hard I pedal, the motor will not kick in if the bike is going over 20 mph.  20 mph is probably about as fast as you can responsibly go on an ebike, but for offroad use it would be fun to go a little faster.

Many C600 displays allow you to increase this top speed using a top speed setting.  This is dictated by the firmware of the LCD display.  Short of reprogramming the device with the DAPU programmer, the easiest way to increase the top speed would be to replace the display with one that allows a higher top speed, or program a mircocontroller to intercept the signal from the LCD display and change the bytes that encode the top speed.  There's a good post about this process here https://www.pedelecforum.de/forum/index.php?threads/kommunikation-zwischen-c7-display-und-motorkontroller-ncm-venice-das-kit.57050/  Since aerodynamic drag increases with the square of your speed, this could dramatically reduce the battery life if ridden continuously at high speeds.


## Connectors

There are a lot of different types of connectors used in ebikes - here's the best visual guide I found
https://www.ebikes.ca/learn/connectors.html
