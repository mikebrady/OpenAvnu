
OpenAvnu aPTP Fork
==================
Support for the apple vendor ptp profile can be found here: https://github.com/rroussel/OpenAvnu
on the branch ArtAndLogic-aPTP-changes.

These changes allow interaction with apple proprietary ptp clocks. This implementation has been tested with the apple airplay SDK on a raspberry pi 3 running within a group of devices playing the same music stream.

To enable this feature one must build for the raspberry pi:
ARCH=RPI make clean all

NOTES:

    The gptp_cfg.ini file must have the variables marked as "Apple Vendor PTP profile" set to the values within the default ini file for the code to work properly. Some tweaking of the priority1 value may be necessary to achieve the desired selection of best master clock for a given environment.

    When in master state one must ensure that the unicast_send_nodes variable (in the ini file) is set with the IP addresses for the nodes to which ptp messages will be sent. This is a comma separated list of ipv4 or ipv6 addresses:
    unicast_send_nodes = fe80::16f4:a1c4:8d8d:c5b1,192.168.0.188

    An alternative to the use of the unicast_send_nodes ini variable is the use of the address registration message service. This allows other applications to register nodes at run time via socket communication. To use this interface set the ini variable address_registration_socket_port to the desired port number. For more detail about this interface please see addressapi.cpp

    Though one can run the RPI build on any linux compatible hardware, the pulse per second interface will only run on the raspberry pi 3. This will set the physical pin number 11 (GPIO17) high once per second.

		An extra flag -- LOGLEVEL -- can be defined to set the level of verbosity of logs if the DLT logging system is not installed. The flag is as follows:
LOGLEVEL=<log_level>, where <log_level> can be any one of the following (highest to lowest priority): GPTP_LOG_LVL_CRITICAL, GPTP_LOG_LVL_ERROR, GPTP_LOG_LVL_EXCEPTION, GPTP_LOG_LVL_WARNING, GPTP_LOG_LVL_INFO, GPTP_LOG_LVL_STATUS, GPTP_LOG_LVL_DEBUG, GPTP_LOG_LVL_VERBOSE.
Logs with the same or higher priority (lower priority number) will be logged.
Example:

ARCH=RPI LOGLEVEL=GPTP_LOG_LVL_CRITICAL make clean all

This will only output the most critical of log messages. However, some diagnostic messages that do not use the logging system are still logged.