# Splunk Add-on for Perforce
This add-on will get perforce log files enabling users to have a better overview of the system by analysing provided metrics.

## Features
* Get audit and performance data (Network and Database) from Perforce Helix Core Server to build metrics
* Ingested data will have different sourcetypes depending on the event type as depicted in table below
* CIM compliance to ease integration with other services

| **Eventtype**  | **Sourcetype**              | **Description**                                                           |
|----------------|-----------------------------|---------------------------------------------------------------------------|
| `4`,`5`        | `hx:errors`                 | Error events (errors-failed, errors-fatal)                                |
| `6`            | `hx:audit`                  | Audit events (p4 sync, p4 archive, etc)                                   |
| `7`            | `hx:track:usage`            | Performance usage tracking                                                |
| `8`            | `hx:track:rpc`              | Network performance tracking (incl. send/receive errors and duplex stats) |
| `9`            | `hx:track:db`               | Database performance tracking (incl. lock times, peek)                    |
| `10`           | `hx:user`                   | User events; one record every time a user runs p4 logappend               |
| `11`           | `hx:triggers`               | Trigger events                                                            |
| `12`           | `hx:events`                 | Server events (startup, shutdown, checkpoint, journal rotation, etc.)     |
| `14`           | `hx:track:networkestimates` | Network estimates                                                         |
| `15`           | `hx:integrity`              | Major events that occur during replica integrity checking                 |
| `16`           | `hx:auth`                   | Login events                                                              |
| `17`           | `hx:route`                  | Log the full network route of authenticated client connections            |


## Getting Started
### Requirements
* Structured server logs enabled on Helix Core Server. In particular, make sure the server is recording information into the following comma separated value files
    * `audit.csv`
    * `auth.csv`
    * `errors.csv`
    * `events.csv`
    * `integrity.csv`
    * `route.csv`
    * `track.csv`
    * `triggers.csv`
    * `user.csv`
* Splunk Universal or Heavy Forwarder installed and [properly configured](https://docs.splunk.com/Documentation/Forwarder/8.2.2/Forwarder/Installanixuniversalforwarder#After_you_install:_Start_and_configure_the_universal_forwarder)

### Installation
Splunk System Administrators are requested to:
* Configure a new index (e.g. `helix`) which will be populated with events coming from the Helix Core Server
* Install this app in the UF/HF
* Configure the forwarder `inputs.conf`
    * make sure monitored log files exist in your Helix Core Server
    * modify index name if different from `helix`
    * enable data input

### Usage
As soon as your data is indexed, build and enjoy your own analytics.

## Troubleshooting
Helix Core Server commands 

```bash
# Add P4AUDIT environment parameter at start
$ vi /etc/perforce/p4dctl.conf.d/master.conf 
    + P4AUDIT   =     ../logs/audit.csv
# Restart the whole server
$ p4dctl restart master
# Check current configuration
$ p4 configure show
# Perform login if p4 commands return nothing
$ p4 login
```

Check data is flowing in Splunk via SPL
`index=<YOUR_INDEX>` (e.g. `index=helix`)

## References
* [Structured server logs](https://www.perforce.com/perforce/doc.current/manuals/p4sag/Content/P4SAG/structure-logging-using.html)
* Restart perforce server via [p4dctl command](https://www.perforce.com/manuals/p4sag/Content/P4SAG/p4dctl.commands.html?Highlight=p4dctl)
* Describe the schema of structured log record types via command [p4 logschema](https://www.perforce.com/manuals/cmdref/Content/CmdRef/p4_logschema.html)
* [P4AUDIT variable](https://www.perforce.com/manuals/cmdref/Content/CmdRef/P4AUDIT.html)
* [Enable auditing](https://www.perforce.com/manuals/p4sag/Content/P4SAG/auditing-user-file-access.html)

## License
This project is licensed under [Apache-2.0](LICENSE)
