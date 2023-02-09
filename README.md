# bambu-lab-observer
Bambu Lab observer, receiving events from Bambu Lab printers and building a .CSV file listing all printed timings

IN PROGRESS

## Configuration

### Configuration file

Minimal configuration (with mandatory fields) is (for recent firmwares) :

```json
{
    "BambuLabsIP" : [ "192.168.8.130" ],
    "MQTTPassword": "XXXXXXXXX"
}
```

Full configuration (showing all possible fields - any combinations of fields is possible : no need to define everything, see table below for default values for unconfigured fields) : 

```json
{
    "BambuLabsIP" : [ "192.168.8.130" ],
    "CSVFileName" : "bambu-lab-observer.csv",
    "LogLevel" : "INFO",
    "Topics" : [ "device/+/report" ],
    "MQTTUsername" : "bblp", 
    "MQTTPassword": "XXXXXXXXX",
    "MQTTTLS" : true,
    "MQTTQoS" : 0,
    "PolicyAboutRegisterStartOfPrint" : "FIRST_MESSAGE_TIMESTAMP"
}```

### Available fields

Details of configuration fields :

| Field                       | Mandatory | Default value             | Explanations                                                                |
|-----------------------------|-----------|---------------------------|-----------------------------------------------------------------------------|
| `BambuLabsIP`                 | **YES** | -                         | Array of IPs addresses that need to be monitored like `[ "192.168.1.10", "192.168.1.11" ]`. Check your router, or the network panel of the settings on the Bambu screen                    |
| `CSVFileName`                     | NO  | `bambu-lab-observer.csv`  | Name (and path if needed) of the CSV file in which the prints are going to be reported                    |
| `LogLevel`                        | NO  | INFO                      | Log levels. Possible values : `DEBUG`, `INFO`, `WARN`, `ERROR` , `NONE`     |
| `Topics`                          | NO  | `device/+/report`         | Array of topics to be monitored. `+` means "any value". Example : `[ "#" ]` (all topics), or `[ "device/<AMD_ID_1>/report", "device/<AMD_ID_2>/report" ]`                    |
| `MQTTUsername`                    | NO  | `bblp`                    | At this time all Bambu Lab printers have that `bblp` default username              |
| `MQTTPassword`                | **YES** | -                         | Since X1C firmware `01.04.00.00 (20230207)`, secured auth is required to access the X1C MQTT server, hence `TLS` to be activated (see next parameter) and `MQTTPassword` to be provided. You can find / reset the password in the Network settings of the printer (just below the `LAN mode` feature)  |
| `MQTTTLS`                         | NO  | true                      | Now activated by default, see previous point                                             |
| `MQTTQoS`                         | NO  | `0`                       | Default value for MQTT QoS. `0` should be fine, keep in mind that higher values may have side  effects on the MQTT server located inside the printer (as messages will be buffered there in case of the network being down, and so on)                    |
| `PolicyAboutRegisterStartOfPrint` | NO  | `FIRST_MESSAGE_TIMESTAMP` | Policy about "how to track the beginning of one print. Either `FIRST_MESSAGE_TIMESTAMP` : beginning will be after the first MQTT message is sent by the printer (but if you stop and restart the program during the print, the counting will restart at 0), either `GCODE_START_TIMESTAMP`, which means the start time will be extracted from the GCODE information sent by the printer (in this case, you can restart the program during the print, BUT it may be inaccurate as it will be the time at which you hitted "print" in Bambu Studio)                    |
