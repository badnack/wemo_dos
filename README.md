# DoS Vulnerability in the Wemo Switch 28B

The Wemo Switch is a smart socket developed by Belkin. In this blogpost, I consider the Wemo switch 28B (sn 221441K010028B), whereas the firmware version is WeMo_WW_2.00.11057.PVT-OWRT-SNS.

The way the rules are handled in the device's firmware allows an attacker to perform a DoS on the device and corrupt the database in a way that the app cannot recover, making the user unable to set/remove any rules.

In particular, the device stores rules in an internal database, which is sent by the app (in a compressed format), every time a rule is added (or removed). No authentication is required.

This repo contains the exploit to perform the DoS. As you can see, once the new database is accepted by the device, further rules from the app are not accepted, and the user must hard reset the device, losing all the information they previously stored.
