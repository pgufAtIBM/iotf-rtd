============================================================================
Device Management
============================================================================

The device management feature enhances the Internet of Things Foundation service with new capabilities for managing devices. This introduction will introduce some of the concepts and features of device management.

What does Device Management add?
-----------------------------------

- Control and management of device lifecycles for both individual and batches of devices.
- Device metadata and status information, enabling the creation of device dashboards and other tools.
- Diagnostic information, both for connectivity to the Internet of Things Foundation service, and device diagnostics.
- Device management commands, like firmware update, and device reboot.

At a more technical level, device management adds:

- a device model, that describes the metadata and management characteristics of a device,
- a ReST API which provides a callable interface for managing devices,
- and a device management protocol, used for communication between the management agent on the devices and the Internet of Things Foundation.

Important concepts in device management
-----------------------------------------

Managed and Unmanaged Devices
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Device management creates a distinction between managed and unmanaged devices.

- **Managed Devices** are defined as devices which have a management agent installed. The management agent sends and receives device metadata and responds to device management commands from the Internet of Things Foundation. The device management agent and the Internet of Things Foundation device management service must share an understanding of data formats and communication patterns so they can interpret data correctly.
- **Unmanaged Devices** are any devices which do not have a device management agent. All devices begin their lifecycle as unmanaged devices, and can transition to managed devices by sending a message from a device management agent to the Internet of Things Foundation. Devices without a device management agent installed can never become managed devices.

Device Model
~~~~~~~~~~~~~~~~~
The device model is the combination of device metadata and management characteristics of a device. Devices can contain a lot of metadata, including device identifiers, device type and associated identifiers, and device model extensions. For a list of identifiers, see the device model reference documentation.

Device Management Protocol
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This protocol is used for device management communication between the Internet of Things Foundation and the management agent on a managed device. The device management protocol is based on MQTT and uses a different set of topics than the regular device messaging protocol. The device management agent of a device must implement the device management protocol in order to enable device management functions.

Device Identifiers
~~~~~~~~~~~~~~~~~~~~~~

Manufactured devices come with a variety of identifiers, such as model and serial number. Each identifier can be considered a separate namespace, but the combination of identifiers which uniquely identify a specific device is customer dependent and may be very complex.

In order to accommodate this complexity, the device management standards have defined many types of identifier, and these are represented in the core device model. See the Device Model Attributes reference document for more detailed information on device identifiers and attributes.

Management Agent
~~~~~~~~~~~~~~~~~~~

A management agent is a collection of logic installed on a device which allows it to connect to the Internet of Things Foundation service as a managed device. The management agent understands the metadata and device management commands used in the device management messaging protocol. Without a management agent, devices cannot use the device management messaging protocol and so cannot become managed devices.


The Device Management Lifecycle
-----------------------------------

1. A device and its associated device type are created in the Internet of Things Foundation using the dashboard or REST API.
2. The device connects to the Internet of Things Foundation and uses the 'Manage Device' operation to become a managed device.
3. The device's metadata, as described in the Device Model can now be viewed and manipulated through device operations, for example, firmware update and device reboot.
4. The device can communicate updates through the device-management protocol, such as location or diagnostic information and error codes.
5. In order to provice a way to deal with defunct devices in large device populations, the 'Manage device' operation request has an optional lifetime parameter. This lifetime parameter is the number of seconds within which the device must make another 'Manage device' request in order to avoid being marked as dormant and becoming an unmanaged device.
6. When a device is decommissioned it can be removed from the Internet of Things Foundation using the dashboard or REST API.
