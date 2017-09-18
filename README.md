# check-systemd-service
Nagios compatible systemd service checker

This plugin checks the status of a given systemd service. However, what makes this service checker unique from other implementations is its systemd oneshot service checker. Oneshot services with timers are modern, more advanced, replacements for cronjobs, and this checker makes it easy to monitor the status of these jobs in a centralized and generic way.

You may provide warning and critical levels for both running time and time since last exit to notify you if the job is not running correctly or if it is running for too long. It also provide performance data for running time and time since exit for oneshot services.

# Usage
## Icinga 2
Drop [check_systemd_service](src/check_systemd_service) into your nagios plugin dir, typically `/usr/lib/nagios/plugins`.

Command definition:

    object CheckCommand "systemd-service" {
      command = [ PluginDir + "/check_systemd_service" ]

      arguments = {
        "--service" = "$systemd_service$"
        "--exit-time-warning" = "$systemd_service_exit_time_warning$"
        "--exit-time-critical" = "$systemd_service_exit_time_critical$"
        "--running-time-warning" = "$systemd_service_running_time_warning$"
        "--running-time-critical" = "$systemd_service_running_time_critical$"
      }
    }
 
Service definition:
 
    apply Service for (systemd_service => config in host.vars.systemd_services) {
      import "generic-service-custom"

      check_command = "systemd-service"
      command_endpoint = host.name

      vars += config
    }

Host object usage examples:

    # Oneshot service examples
    vars.systemd_services["borgwrapper backup"] = {
        systemd_service = "borgwrapper-backup@config"
        systemd_service_exit_time_warning = 172800
        systemd_service_exit_time_critical = 259200
        systemd_service_running_time_warning = 172800
        systemd_service_running_time_critical = 259200
    }

    vars.systemd_services["borgwrapper backup verification"] = {
        systemd_service = "borgwrapper-verify@config"
        systemd_service_exit_time_warning = 172800
        systemd_service_exit_time_critical = 259200
        systemd_service_running_time_warning = 172800
        systemd_service_running_time_critical = 259200
    }
    
    # Standard service example. Only oneshot services have warning and critical levels as normal
    # services are expected to run continuously.
    vars.systemd_services["Salt Master"] = {
        systemd_service = "salt-master"
    }    
