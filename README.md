# iothubmqtt
Shows two ways of connecting to IOT Hub directly over MQTT.<br>
A practical adaptation of the guidelines here: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-mqtt-support

Pre-requisites:
1) Install Azure CLI https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest
2) Install IOT Extensions for Azure CLI<br> `az extension add --name azure-cli-iot-ext`

## X509 Cert

1) Create a device identity with a self-signed cert that is valid for 10 days using <br>
   `az iot hub device-identity create -n <hubname> -d <deviceid> --am x509_thumbprint --valid-days 10 --od .`<br>
   This will generate two files `<deviceid>-cert.pem` and `<deviceid>-key.pem` in the current folder
2) Modify iothubcert.py by setting:<br>
    `device_id=<deviceid>`  <br>
    `iot_hub_name=<hubname>` <br>
3) Run iothubcert.py. It should connect and send one message to IOT Hub over MQTT.
## SAS
1) Create a device identity with a SAS Token:<br>
    `az iot hub generate-sas-token -d <deviceid> -n <hubname>` <br>
    The token is the entire returned value, i.e. <br>`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`
2) Modify iothubsas.py by setting:<br>
    `device_id=<deviceid>` <br>
    `iot_hub_name=<hubname>` <br>
    `sas_token=<value of sas token generated in step 1>` <br> 
3) Run iothubsas.py. It should connect and send one message to IOT Hub over MQTT.