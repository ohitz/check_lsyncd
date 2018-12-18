# check_lsyncd

## Description

This is a nagios plugin to check the status of
[lsyncd](https://github.com/axkibe/lsyncd).

The following tests are implemented:

- Is an lsyncd process running? If not, CRITICAL status is returned.

- Does an lsyncd status file exist? If not, CRITICAL status is returned.

- Compare the total number of delays to warning and critical thresholds.

The plugin requires a `statusFile` to be configured. The path defaults to
`/var/run/lsyncd.status`.

## Usage

    check_lsyncd [-h] [-w N] [-c N] [-s PATH]
    
    -h
        Show help.
    
    -w N
        Warning if more than N delays (default: 10).
    
    -c N
        Critical if more then N delays (default: 100).
    
    -s PATH
        Set status file path (default: /var/run/lsyncd.status).

### Icinga2 configuration

Define a new service for all Linux hosts with `vars.lsyncd`, for example:

```
apply Service "Lsyncd status" {
  import "generic-service"
  check_command = "lsyncd"
  vars.lsyncd_statfile = "/var/log/lsyncd/lsyncd.stat"
  command_endpoint = host.vars.client_endpoint
  assign where host.vars.client_endpoint && host.vars.os == "Linux" && host.vars.lsyncd
}
```

## Author

Oliver Hitz <oliver@net-track.ch>
