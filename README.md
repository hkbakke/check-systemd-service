# check-systemd-service
Nagios compatible systemd service checker

This plugin checks the status of a given systemd service. However, what makes this service checker unique from other implementations is its oneshot service checker. Oneshot services with timers are modern more advanced replacement for cronjobs, and this checker makes it easy to monitor the status of these jobs in a centralized and generic way.

You may provide warning and critical levels for both running time and time since last exit to notify you if the job is not running in the expected interval or if it is running for too long. It also provide performance data for running time and time since exit for oneshot services.
