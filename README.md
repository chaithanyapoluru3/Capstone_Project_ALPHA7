# Capstone_Project_ALPHA7

# Implementation and Functional Verification of Communication Protocols 

# Introduction
In embedded systems, communication between devices is crucial for data transfer, synchronization, and control. This project focuses on three widely-used communication protocols: CAN (Controller Area Network), I2C (Inter-Integrated Circuit), and SPI (Serial Peripheral Interface).

<br> CAN is designed for real-time data transfer in noisy environments, widely employed in automotive and industrial systems. I2C, a simple two-wire protocol, facilitates low-speed communication between a master and multiple slave devices, making it ideal for embedded systems like sensors and displays. SPI, a high-speed full-duplex protocol, is used for efficient, low-latency communication between a master and peripherals like memory devices and sensors. 
                         <br>                                           Together, these protocols form the backbone of communication in modern embedded systems, enabling seamless data exchange across various applications.


# Overview of the problem statement
  The increasing complexity of embedded systems in fields like automotive, industrial automation, and consumer electronics demands reliable, efficient, and real-time communication between multiple devices. Systems require communication protocols that can handle various data transmission needs, from high-speed, low-latency operations to robust, fault-tolerant interactions in noisy environments.
   <br> This project addresses the challenge of designing, simulating, and validating three key communication protocols—CAN, I2C, and SPI—using Verilog. Each protocol has its own strengths and specific use cases, but integrating and understanding their functionality through simulations provides crucial insights into their behavior and performance in real world applications.

                        

# Methodology
The methodology involves designing the CAN, I2C, and SPI protocols in Verilog, followed by extensive simulations to validate their functionality and performance through waveform analysis.
                 

## I2C (Inter Integrated Circuit)
I2C stands for Inter-Integrated Circuit. It is a bus interface connection protocol incorporated into devices for serial communication. It was originally designed by Philips Semiconductor in 1982. Recently, it is a widely used protocol for short-distance communication. It is also known as Two Wired Interface (TWI).
### <br>Working of I2C Communication Protocol:
<br> •	It uses only 2 bi-directional open-drain lines for data communication called SDA and SCL. Both these lines are pulled high.
•	Serial Data (SDA) – Transfer of data takes place through this pin.
•	Serial Clock (SCL) – It carries the clock signal.
•	I2C operates in 2 modes –
                    Master mode
                    Slave mode
•	Each data bit transferred on SDA line is synchronized by a high to the low pulse of each clock on the SCL line.

<br> ![image](https://github.com/user-attachments/assets/17045c9e-fd99-44d8-8631-e049f5d2f161)
<br>According to I2C protocols, the data line cannot change when the clock line is high, it can change only when the clock line is low. The 2 lines are open drain, hence a pull-up resistor is required so that the lines are high since the devices on the I2C bus are active low. <br> The data is transmitted in the form of packets which comprises 9 bits. The sequence of these bits are  –
                                                        Start Condition – 1 bit
                                                        Slave Address – 8 bit
                                                         Acknowledge – 1 bit
          <br> ![image](https://github.com/user-attachments/assets/f8fe90ad-27fc-498f-a6b8-1a8f69e459f2)
          <br> With I2C, data is transferred in messages. Messages are broken up into frames of data. Each message has an address frame that contains the binary address of the slave, and one or more data frames that contain the data being transmitted. The message also includes start and stop conditions, read/write bits, and ACK/NACK bits between each data frame:
 <br> **Start Condition** : The SDA line switches from a high voltage level to a low voltage level    before the SCL line switches from high to low.
<br> **Stop Condition** : The SDA line switches from a low voltage level to a high voltage level after the SCL line switches from low to high.
<br> **Address Frame** : A 7or10 bit sequence unique to each slave that identifies the slave when the master wants to talk to it.
<br> **Read/Write Bit** : A single bit specifying whether the master is sending data to the slave (low voltage level) or requesting data from it (high voltage level).
<br> **ACK/NACK Bit** : Each frame in a message is followed by an acknowledge/no-acknowledge bit. If an address frame or data frame was successfully received, an ACK bit is returned to the sender from the receiving device.

## PROTOCOL 2:  SPI (serial peripheral interface)
SPI is a common communication protocol used by many different devices. For example, SD card reader modules, RFID card reader modules, and 2.4 GHz wireless transmitter/receivers all use SPI to communicate with microcontrollers.
                <br> One unique benefit of SPI is the fact that data can be transferred without interruption. Any number of bits can be sent or received in a continuous stream. 
                    <br> Devices communicating via SPI are in a master-slave relationship. The master is the controlling device (usually a microcontroller), while the slave (usually a sensor, display, or memory chip) takes instruction from the master. The simplest configuration of SPI is a single master, single slave system, but one master can control more than one slave (more on this below). 
        <br> ![image](https://github.com/user-attachments/assets/a160f6c4-97f3-46ec-a83d-560bac344278)
        <br> •	MOSI (Master Output/Slave Input) – Line for the master to send data to the slave.
   <br> •	MISO (Master Input/Slave Output) – Line for the slave to send data to the master.
   <br> •	SCLK (Clock) – Line for the clock signal.
   <br> •	SS/CS (Slave Select/Chip Select) – Line for the master to select which slave to send data to.
   ### Working
   <br> **Slave Select**
             <br> The master can choose which slave it wants to talk to by setting the slave’s CS/SS line to a low voltage level. In the idle, non-transmitting state, the slave select line is kept at a high voltage level. Multiple CS/SS pins may be available on the master, which allows for multiple slaves to be wired in parallel. If only one CS/SS pin is present, multiple slaves can be wired to the master by daisy-chaining.
             <br> **MOSI and MISO**
<br> •	The master sends data to the slave bit by bit, in serial through the MOSI line. The slave receives the data sent from the master at the MOSI pin. Data sent from the master to the slave is usually sent with the most significant bit first.
<br> •	The slave can also send data back to the master through the MISO line in serial. The data sent from the slave back to the master is usually sent with the least significant bit first.


## PROTOCOL 3: CAN (controller area network)
Controller Area Network, in short CAN protocol is a serial communication protocol which provides efficient support to mainly automotive real time control systems with a very high level of security, error detection and correction. CAN data frame can be defined as in the fig below. It consists of start of frame (SOF), arbitration field, control field, data field, CRC field, ACK field and end of frame(EOF).
![image](https://github.com/user-attachments/assets/1f8e6abc-947f-4e16-99ca-238565b05f8a)
### Working
<br> •	Transmission of a data frame begins with the start bit (Start of Frame — SOF). It is transmitted by the sender as a dominant level which produces a signal edge from the previous recessive (bus idle) level which is used to synchronize the entire network.
<br> •	 In order for the receivers not to lose synchronism to the sender during transmission of the frame, they compare all recessive-to-dominant signal edges with their preset bit timing. In case of deviation, receivers re-synchronize by the amount of the relevant phase error (re-synchronization).
<br> •	Following the SOF is the identifier (ID). This sets the priority of the data frame, and together with the acceptance filtering it provides for sender-receiver relations in the CAN network that are defined in the communication matrix. Next comes the RTR bit (Remote Transmission Request). It is used by the sender to inform receivers of the frame type (data frame). A dominant RTR bit indicates a data frame.
<br> •	The IDE bit (Identifier Extension bit) which follows serves to distinguish between standard format and extended format. In standard format the identifier has 11 bits, and in extended format 29 bits. The figure “Data Frame in Standard and Extended Format” is available to study the two formats.
<br> •	The DLC (Data Length Code) communicates the number of payload bytes to the receivers. The payload bytes are transported in the data field. A maximum of eight bytes can be transported in one data frame.
<br> •	The payload is protected by a checksum using a cyclic redundancy check (CRC) which is ended by a delimiter bit. Based on the results of the CRC, the receivers acknowledge positively or negatively in the ACK slot (acknowledgement) which also is followed by a delimiter bit.
<br> •	After this the transmission of a data frame is terminated by seven recessive bits (End Of Frame — EOF).

## PROTOCOL 4: UART(Universal Asynchronous Receiver/Transmitter)
It’s not a communication protocol like SPI and I2C, but a physical circuit in a microcontroller, or a stand-alone IC. A UART’s main purpose is to transmit and receive serial data.​
<br> One of the best things about UART is that it only uses two wires to transmit data between devices.​
<br> In UART communication, two UARTs communicate directly with each other. The transmitting UART converts parallel data from a controlling device like a CPU into serial form, transmits it in serial to the receiving UART, which then converts the serial data back into parallel data for the receiving device. Only two wires are needed to transmit data between two UARTs. Data flows from the Tx pin of the transmitting UART to the Rx pin of the receiving UART:​

  ![image](https://github.com/user-attachments/assets/728bcd8e-9f9c-4e86-b342-3ae290312d8d)

  <br> UARTs transmit data asynchronously, which means there is no clock signal to synchronize the output of bits from the transmitting UART to the sampling of bits by the receiving UART. Instead of a clock signal, the transmitting UART adds start and stop bits to the data packet being transferred. ​
<br> When the receiving UART detects a start bit, it starts to read the incoming bits at a specific frequency known as the baud rate. ​
<br> Baud rate is a measure of the speed of data transfer, expressed in bits per second (bps). Both UARTs must operate at about the same baud rate. The baud rate between the transmitting and receiving UARTs can only differ by about 10% before the timing of bits gets too far off.​
<br> Both UARTs must also must be configured to transmit and receive the same data packet structure.​
<br> step1: ![image](https://github.com/user-attachments/assets/ab943bbb-6e5f-43f3-9243-d403b29809a5)
<br> Step2: ![image](https://github.com/user-attachments/assets/837509bc-83f7-41e3-b857-3b914dc5424f)
<br> Step3: ![image](https://github.com/user-attachments/assets/4d619278-3d70-431c-ad84-084d0504095c)
<br> Step4: ![image](https://github.com/user-attachments/assets/a5706ce0-f3c4-4ef9-9fe2-b58e4793554c)


## PROTOCOL 5: FlexRay Protocol
FlexRay is an automotive network communications protocol developed through the FlexRay Consortium to govern on-board automotive computing.
<br> FlexRay supports high data rates, up to 10 Mbit/s, explicitly supports both star and party line bus topologies, and it consist of two independent data channels for fault-tolerance.The bus operates on a time cycle, divided into two parts: the static segment and the dynamic segment. The static segment is pre-allocated into slices for individual communication types, providing a stronger real-time guarantee than its predecessor CAN. The dynamic segment operates more like CAN, with nodes taking control of the bus as available, allowing event-triggered behavior.
####  FlexRay Frame Format
![WhatsApp Image 2025-02-19 at 14 19 37_a6b06379](https://github.com/user-attachments/assets/baf4b67e-27fa-4af3-b2cc-fc0d01124157)

An overview of the general FlexRay frame format is illustrated in above fig. The FlexRay frame format divides into three segments: Header, Payload, and Trailer.





 









          


                                                        







