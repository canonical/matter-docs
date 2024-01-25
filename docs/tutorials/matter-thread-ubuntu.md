# Running Matter applications with Thread networking on Ubuntu

This is a tutorial on setting up and running Matter applications that use Thread
for networking on Ubuntu.
We will scope the tutorial to Matter applications built using the [Matter SDK]
and [OpenThread Border Router] (OTBR).

<!-- This guide will walk you through building, running, and controlling the [Matter Lighting App example](https://github.com/project-chip/connectedhomeip/tree/6b01cb977127eb8547ce66d5b627061dc2dd6c90/examples/lighting-app/linux) with Thread networking on Ubuntu. -->

## Prerequisites 

- Two amd64/arm64 machines, with:
  - Ubuntu Server/Desktop 22.04 or newer
  - Thread [Radio Co-Processor (RCP)](https://openthread.io/platforms/co-processor#radio_co-processor_rcp)

In this tutorial, we'll use the following:

- Machine A
  - Ubuntu Desktop 23.10 amd64
  - Nordic Semiconductor nRF52840 dongle, using the OT RPC firmware

- Machine B (Raspberry Pi 4)
  - Ubuntu Server 22.04 arm64
  - Nordic Semiconductor nRF52840 dongle, using the OT RPC firmware
 
Machine A will host the Border Router (OTBR) and Matter Controller, while 
Machine B will act as the Matter device and run the Matter application and an
OTBR Agent.
The second OTBR instance will not act as a Border Router, but rather as an agent
which complements the Matter application for Thread networking capabilities.

<!-- TODO: add diagram -->

```{note}

The API versions among the OTBR running on Machine A and OTBR agent running on
Machine B must match. 

In this tutorial, we've used the following:

| Component           | Upstream Commit/Version                                                                                                      | API Version                                                                                                              | snapstore channel |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ | ----------------- |
| Matter lighting app | connectedhomeip [`6b01cb9`](https://github.com/project-chip/connectedhomeip/commit/6b01cb977127eb8547ce66d5b627061dc2dd6c90) | -                                                                                                                        | -                 |
| OTBR snap           | ot-br-posix [`thread-reference-20230119`](https://github.com/openthread/ot-br-posix/tree/thread-reference-20230119)          | [6](https://github.com/openthread/openthread/blob/thread-reference-20230119/src/lib/spinel/spinel.h#L380)                | latest/edge       |
| OTBR RCP firmware   | ot-nrf528xx [`00ac6cd`](https://github.com/openthread/ot-nrf528xx/tree/00ac6cd0137a4f09288b455bf8d7aa72d74062d1)             | [6](https://github.com/openthread/openthread/blob/9af0bfa60e373d81a5576b298d6664045870a375/src/lib/spinel/spinel.h#L420) | -                 |


```


## 1. Set up Border Router on Machine A

Refer to {doc}`/docs/how-tos/Setup OpenThread Border Router on Ubuntu` to set up and configure OTBR.

Then form a Thread network, using the following commands:
```bash
sudo openthread-border-router.ot-ctl dataset init new
sudo openthread-border-router.ot-ctl dataset commit active
sudo openthread-border-router.ot-ctl ifconfig up
sudo openthread-border-router.ot-ctl thread start
```

<!-- TODO: explain what the commands do -->

These steps could also be performed with the Web GUI, served by default at [https://localhost:80](https://localhost:80).
Please refer to the instructions [here](https://openthread.io/guides/border-router/web-gui.md) to configure and form, join, or check the status of a Thread network using the GUI.

The Thread network is now ready for new joiners.
Now, head over to Machine B to setup the Matter application.

## 2. Run OTBR Agent on Machine B

The OTBR Agent is required for adding Thread networking capabilities to the
Matter application. 
The Matter app communicates with OTBR Agent via the DBus Message Bus.

Similar to Machine A, set up and configure OTBR by following: {doc}`/docs/how-tos/Setup OpenThread Border Router on Ubuntu`. However, this time, we don't form a new network.


## 3. Run Matter Application on Machine B

<!-- TODO
- Show two options:
  - Running the pi gpio commander
  - Running lighting example app
-->

## 4. Commission and Control the Matter Application from Machine A

<!-- TODO:
- get the dataset credentials
- point to chip tool how to: {doc}`/docs/how-tos/Commission and Control Matter Devices with Chip Tool`
-->


## References


<!-- links -->
[OpenThread Border Router]: https://openthread.io/guides/border-router
[Matter SDK]: https://github.com/project-chip/connectedhomeip
[Chip Tool Snap]: https://snapcraft.io/chip-tool
