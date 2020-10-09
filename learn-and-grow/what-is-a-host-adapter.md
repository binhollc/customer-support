# What is a host adapter?

{% embed url="https://youtu.be/49SveRYJpWA" %}

In general terms, a _host adapter_ \(also sometimes called a _host bus adapter_\) is a device which connects a computer \(the host system\) to other peripheral devices. Within the computer, there are many ICs on the motherboard communicating using various protocols, such as PCIe, SATA, SMBus, USB, etc. But within the world of electronics, interesting sensors, actuators, and application-specific ICs use several different standard protocols such as SPI, I2C, 1-WIRE, and SWI. These protocols are not typically supported directly by the motherboard chipset or otherwise exposed, either physically or programmatically, for user access.

So what happens when you want to connect one of the devices that communicates via SPI, I2C, 1-WIRE, or SWI to your computer? Well, you’ll need to use an adapter to interface between one of the protocols on the host computer to the desired new protocol. That’s where the the _Binho Multi-Protocol Host Adapter_ comes in.

![](../.gitbook/assets/hostadapterad_v4.png)

The _Binho Multi-Protocol USB Host Adapter_ is the tool that enables one to connect sensors, actuators, and other devices communicating over SPI, I2C, 1-WIRE, or SWI to your computer via USB. The USB host adapter can receive commands and provide data to the host PC over USB, while also managing connections with target I2C/SPI/1-Wire devices and carrying out the desired interactions as directed by software on the host PC.

### Why would I need a host adapter?

There are many occasions when a host adapter would be a tool of great convenience and utility, throughout all stages of the product development cycle. Essentially these situations can be classified into three groups:

#### 1 Rapid Prototyping & Development

Early in the product development cycle, a host adapter can allow engineers to rapidly build prototypes and perform concept validation before any custom hardware or PCBs have been designed or produced. When custom hardware is a scarce resource, host adapters allow developers to emulate the microcontroller, allowing code to be developed and executed on a PC. This enables engineering team members to develop and verify low-level device drivers and subsystem operation. Essentially this ensures the team is unblocked while waiting for hardware to be ready. Then, upon arrival of the hardware, the team is ready to hit the ground running.

Discrete real-world examples of this can be seen in how self-driving car algorithms are being developed. Engineers directly interface their computers to an automobile's systems and sensors on the vehicle by tapping into the vehicle communication bus \(typically CAN or LIN\) with a USB host adapter. In this manner, all of the necessary data can be gathered and processed on their computers while they train their algorithms that will eventually run on custom-developed on-board automotive computers. This is similar to many industries where high-level software developers are working on AI/ML algorithms. It enables the training of neural nets to be performed on high-end systems with tons of compute while the hardware team is developing the custom hardware that will ultimately be employed in the end product. In this case, the use of host adapters to get the desired sensor data onto computers ensures that software development will not be bottle-necked by hardware development.

#### 2 Automated Testing

As a team progresses in the development cycle, the concept has been proven and the focus has shifted into turning the prototype into a rock-solid high quality product, testing becomes extremely important. Hardware testing can become quite cumbersome, and as the firmware evolves and the PCBs are revised and re-fabricated, it becomes imperative that tests are repeated. As such, best practices in the industry incorporate a high level of testing automation, which is less prone to human error and also reduces the amount of time needed to execute thorough test plans from hours down to minutes. This ensures the developers are spending their time making forward progress rather than needing to spend hours ensuring that their latest change didn’t break a feature that’s been working for months.

This one can be divided into two sub-categories:

_A\) Automated Hardware Testing_ --- each time there is a new build/ revision of the board, a test can be performed to verify key hardware performance parameters are within specification. Using a scripted test with a host adapter allows engineers on the hardware team to vet the boards before handing them over to firmware engineers in an unknown state without the need for the firmware team to develop a test mode first.

_B\) Automated Firmware Testing_ -- Best practice in industry includes Continuous Integration, where each time a change to the codebase has been pushed to the code repository, it is automatically recompiled. At a minimum, the result of the compilation needs to be error free, however, the best teams are then extending their CI process to deploy the new firmware build to actual hardware, where a series of scripted tests can be performed and executed. Just because firmware compiles does not mean that it’s working as expected or that some functionality wasn’t broken when running on an embedded system. Host adapters are a perfect way to stimulate various test conditions.

#### 3 Production Line Programming & Provisioning

Aside from the flashing of firmware on to the microcontroller, many devices also have some on-board memory \(EEPROM, FRAM, or FLASH\) that needs to be pre-loaded to a certain state. This could something as simple as programming a specific pair of values to be used as the USB VID/PID values in a USB device, or loading audio/visual assets onto a product with an interactive UI design. Host adapters can be used to Erase, Program, and Verify the data on these memory chips. Additionally, these adapters can be used to provision certain devices which require a one-time setup process during manufacturing, such as assigning unique serial numbers or MAC addresses or provisioning a CryptoAuthentication IC with secret keys in order to obtain the desired level of hardware security.

Good host adapters are gang-able \(meaning that you can attach many of them to the same host computer and control them in parallel\) so it will be easy to scale up the throughput / efficiency of the programming/provision process so that it’s not a bottleneck in your product assembly procedure.

