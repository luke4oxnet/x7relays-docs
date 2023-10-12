# x7relays - przekaźniki Ethernet

Urzadzenie x7relays posiada 4 niezależne przekaźniki, dopuszczalne obciążenie każdeg toru to 16A.


<!-- For full documentation visit [mkdocs.org](https://www.mkdocs.org). -->

## Sterowanie przez REST

REST API umożliwa:

 * sterowanie przekaźnikami
 * odczyt stanu przekaźników
 * sterowanie izolowanymi wyjściami cyfrowymi
 * odczyt stanu wejść optoizolowanych
 * odczyt informacji  urządzeniu
 * aktualizację oprogramowania wbudowanego

### Sterowanie przekaźnikami 


Przekaźniki są numerowane o 1 do 4.

Włączanie lub wyłączanie przekaźnika

Włączenie przekaźnika 1: 
```bash
curl http://192.168.5.10/relays.shtml/?relay1=1
```

Wyłączenie przekaźnika 1: 

```bash 
curl http://192.168.5.10/relays.shtml/?relay1=0
```

### Odczyt stanu przekaźników

```bash
curl http://192.168.5.10/relays.json
```
Przykładowa odpowiedź:
```json
{"relay1":{"state":false},"relay2":{"state":false},"relay3":{"state":true},"relay4":{"state":false}}
```

## Setrowanie przez MQTT

Przykładowy harmonogram (schedule.json)
```json
{
"schedule": [
                    {
                        "priority": 0,
                        "fromDate": "null",
                        "toDate": "null",
                        "tasks": [
                            {
                                "hour": "7:36:00",
                                "daysOfWeek": [1, 2, 3, 4, 5, 6,7],
                                "command": {
                                    "commandId": null,
                                    "peripherialAddress": null,
                                    "channelAddress": "relay1",
                                    "commandType": "RELAY",
                                    "commandPayload": {
                                        "state": true
                                    }
                                }
                            },
                            {
                                "hour": "7:40:00",
                                "daysOfWeek": [1, 2, 3, 4, 5, 6,7],
                                "command": {
                                    "commandId": null,
                                    "peripherialAddress": null,
                                    "channelAddress": "relay1",
                                    "commandType": "RELAY",
                                    "commandPayload": {
                                        "state": false
                                    }
                                }
                            }
                        ]
                    }
            ]
}
```

Wysłanie harmonogramu do urządzenia przez mqtt.

```bash
cat schedule.json | mosquitto_pub -h 192.168.1.31 -t "x7relays-poc/60b18501-f7a1-582d-89f6-fc1d096d234b/config" -s
```



