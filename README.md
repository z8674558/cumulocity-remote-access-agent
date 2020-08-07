# Cumulocity Remote Access Agent
A simple python agent demonstrating the remote access capabilities of Cumulocity. Main Purpose is to demonstrate and use the Cloud Remote Access in other Agents.
The module 'DeviceProxy' can be re-used and easily embedded into your Python project. It supports multiple parallel connections. It was tested in Python3.


# Prerequisites

* Install Python 2 or Python3 if not already installed. This module has been tested with Python3.
* Import the [Smart REST Template](smartrest.json) to Cumulocity which contains a Response Template for Remote Access Connect Operation.
* Change the [c8yagnet.py](c8yagent.py) header to your C8Y Credentials (device_id, baseurl, tenant, user and password must be set)
* Your Tenant must have subscribed to the feature `cloud-remote-access`. If you don't see that in your subscribed applications ask you Administrator to get it subscribed.
* Your user must have the permission to use the Cloud Remote Access Feature. There is a permission `Remote Access` which needs to be assigned to your user by your Administrator. 
* If you don't see any Tab in Device Management `Remote Access` it is most likely because of one of the above mentioned bullet points.
* To test it you need a local SSH Server e.g. from [Docker Hub](https://hub.docker.com/search?q=openssh&type=image) running and accessible by your C8Y Agent. Please note that if you run your Agent in another docker container that the IP configured in the UI is the host IP address. You can find it out resolving the dns `host.docker.internal` most likely it is `192.168.65.2`
* This is not only limited to SSH Server but also should support VNC and Telnet as well (even it has never been tested :-))

# Run it
* Install all dependencies by:
```sh
pip install -r requirements.txt
```
* Afterwards run:
```sh
python c8yagent.py
```

* In your Device Management you will se a Device  `Remote Access Demo Device <your device_id>` with a Tab `Remote Access`
Create a new Connection by `Add endpoint` and configure the required fields. Also make sure you set the correct credentials like User + PW or Private/Public Keys. The Host Key can be kept empty.
* Click on Connect and check if everything is working as expected.


# Embed Device Proxy Module
Copy the device_proxy.py to your project and import it to your Module which handles Cumulocity Operations
```sh
from device_proxy import DeviceProxy, WebSocketFailureException
```

Make sure you subscribe to the Smart REST Template id
Example:
```sh
subscribe(mqttClient, f's/dc/{template_id}',0)
```
On received and valid Operation initialize the DeviceProxy module with the required and documented arguments. Please note that the Device Proxy supports Basic Auth with Tenant, User, Password and Device Certificates using a token. Either the basic credentials or the token must be set by initializing the DeviceProxy module.

Run the function `DeviceProxy.connect()`.

# Troubleshooting
* Check the status of the Operation in `Control` Tab and the failed Operations with the failure reason.
* Increase the Log Level of the Agent and Debug to the DeviceProxy Module to check what's going on.