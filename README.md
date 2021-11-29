# Splunk Add-on for Perforce
This add-on will get perforce log files enabling users to have a better overview of the system by analysing provided metrics.

**Stay tuned: more to come**

## Features
* Get audit and performance data (Network and Database) from Perforce or Helix Core Server to build metrics
* Ingested data will have different sourcetypes depending on the event type as depicted in table below
* CIM compliance to ease integration with other services 
    > **TODO**

| **Eventtype** | **Sourcetype**              | **Description**                                                           |
|----------------|-----------------------------|---------------------------------------------------------------------------|
| `6`            | `hx:audit`                  | Audit events (p4 sync, p4 archive, etc)                                   |
| `7`            | `hx:track:usage`            | Performance usage tracking                                                |
| `8`            | `hx:track:rpc`              | Network performance tracking (incl. send/receive errors and duplex stats) |
| `9`            | `hx:track:db`               | Database performance tracking (incl. lock times, peek)                    |
| `14`           | `hx:track:networkestimates` | Network estimates                                                         |
| `16`           | `hx:auth`                   | Login events                                                              |

> **TODO** Update table above

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
* Splunk Universal or Heavy Forwarder installed and configured to monitor above log files from the Helix Core Server

### Installation
* Install the app in the UF/HF
* In your forwarder enable data input from `inputs.conf`

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
`index=helix`

## References
* [Structured server logs](https://www.perforce.com/perforce/doc.current/manuals/p4sag/Content/P4SAG/structure-logging-using.html)
* Restart perforce server via [p4dctl command](https://www.perforce.com/manuals/p4sag/Content/P4SAG/p4dctl.commands.html?Highlight=p4dctl)
* Describe the schema of structured log record types via command [p4 logschema](https://www.perforce.com/manuals/cmdref/Content/CmdRef/p4_logschema.html)
* [P4AUDIT variable](https://www.perforce.com/manuals/cmdref/Content/CmdRef/P4AUDIT.html)
* [Enable auditing](https://www.perforce.com/manuals/p4sag/Content/P4SAG/auditing-user-file-access.html)

## License
This project is licensed under [Apache-2.0](LICENSE)
