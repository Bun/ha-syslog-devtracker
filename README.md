# ha-syslog-devtracker

A real-time device tracker for Home Assistant based on syslog events. The
tracker depends on syslog forwarding on a Linux-based dnsmasq DHCP server
and hostapd, including routers based on OpenWRT/LEDE.

Due to the nature of how DHCP works, it is most suitable for detecting when
devices come online.  Detecting when devices go offline may be subject to
delays, especially regarding wired devices.  In general it works well for
wireless devices. See the notes below for more information.

Supported message formats:

* dnsmasq
* hostapd


## Configuration

Copy `syslog.py` to the `custom_components/device_tracker/` directory of your
Home Assistant configuration directory.

Next, add the following configuration:

    device_tracker:
    - platform: syslog
      host: 0.0.0.0
      port: 514
      devices:
        a1b234567890: ben
        d9e87654321f: server

You can add an arbitrary number of MAC address to device name pairs. Make sure
the MAC address is lowercase and without colons.


## Notes

### Wireless devices

Since wireless devices may disconnect/timeout when they are idling (e.g. to
preserve battery -- especially Apple devices), a grace time will be
configurable during which the device will remain marked as active.

The recommended grace time is about 5 minutes and should be configured globally
or per device.


### Wired devices

While it is straightforward to detect devices coming online based on the DHCP
requests, detecting when a wired device has gone offline is more complicated
since this does not appear in the log messages.

If you are interested in tracking wired devices, you should set a reasonable
DHCP lease timeout so that devices that do not renew their lease within that
period can be marked offline.


## TODO

This is a work in progress.

* Only UDP based forwarding is currently supported. TCP support is planned.
* Implement a (syslog origin) whitelist to prevent other hosts on the network
  from sending spoofed messages, especially in UDP mode; currently we assume
  the network can be trusted.
* Do something useful with the fact that we can have multiple log sources.


## Future work

* Considering implementing a method to "ping" devices (e.g. using the IP
  learned from DHCP) to periodically to test if they are still online.
