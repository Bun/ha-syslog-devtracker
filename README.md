# ha-syslog-devtracker

A real-time device tracker for Home Assistant based on syslog events. The
tracker depends on syslog forwarding on e.g. a Linux-based DHCP server or
router.

Due to the nature of how this device tracker works, it is most suitable for
cases where detecting when a device becomes online.


Supported message formats:

* dnsmasq
* hostapd


## Notes

### Wireless devices

Since wireless devices may disconnect/timeout when they are idling (e.g. to
preserve battery -- especially Apple devices), a grace time can be configured
during which the device will remain marked as active.

The recommended grace time is about 5 minutes and can be configured globally
or per device.


### Wired devices

While it is straightforward to detect devices coming online based on the DHCP
requests, detecting when a device has gone offline is more complicated since
this is not logged in any sense.

If you are interested in tracking wired devices, you should set a reasonable



## TODO

This is a work in progress.  For testing purposes, it is currently implemented
with some help from the Home Assistant MQTT device tracker component to
actually receive the states.

* Do something useful with the fact that we can have multiple log sources.


## Future work

* Considering implementing a method to "ping" devices (e.g. using ARP)
  periodically to test if they are still online.
