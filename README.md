# MK-Stair-Sensors
This project was to help expand my smarthome and learn a bit more about combining various things together

## Project Goal

Use multiple Sensors at the top and buttom of stairs to enable "stairs in use detection" and illuminate based upon it

integrate additional external device (doorbell chime)

Do these things without perminent modification to the house. e.g. drilling the wall for mounting or getting power to devices.

## The Plan
Use  Everything Presence Lite's (EPL) for detection

Use a RGB smartbulb for illumination (and also potential for being used for notifications by changing colour etc...)

Using the exposed GPIO to drive doorbell

## Downstairs sensor
There is a prexisting mains powered doorbell chime (that was actually disused) mounted on the wall which I made my target for setting up this sensor as it has power already routed to it.

I opted to buy a doorbell exactly the same so i could study plan and modify without touching the orginal with the eventual plan to be building the modifications into the front panel then switching it over and then keep the orginal cover so that I can quickly switch it back and it looks like nothing ever happened

A hole in the doorbell casing and the electronics box allows for cables to pass through

This allows for mains power to enter the box and for 2 cables to exit to connect to the chime button connections

The bulb holder was liberated from a cheap lamp bought for the purpose of taking apart to get this part of it.

the mains uses a wago connector to split to both the bulb holder and a 5v 600ma power supply which powers the EPL

the EPL then outputs 2 cables back into the box which provide 3.3v and a GPIO pin to a relay (which gets its ground directly from the 5v PSU) This relay then Signals the chime switch

The relay board used is definetly over rated for this particular job however it is what I had and it fits

Was then a matter of modifying the firmware of the EPL to add functionality to drive the relay. This was done by adding the following:

````
switch:
  - platform: gpio
    pin: GPIO32
    inverted: true
    id: doorbell_relay
    name: "Doorbell-Relay"
    icon: "mdi:door"
    on_turn_on:
    - delay: 200ms
    - switch.turn_off: doorbell_relay

button:
  - platform: template
    name: "Ding-Dong"
    on_press:
      - switch.turn_on: doorbell_relay
````

once in use the switch is hidden in home assistant and the button is what is used however it was handy to have during testing/troubleshooting

As if by magic we get a new button on home assistant that makes it go "Ding-Dong". this also means i can trigger it from the ring doorbell which it was not attached to previously. so it has actually regained its purpose as a doorbell chime during this process.

The reason for going for a perminently powered smartbulb instead of a relay driven bulb was for nicer and smoother control and options into the future for differnt ways of displaying things and have it fade up and down in a controlled way independant of the relay powering it up and down.
