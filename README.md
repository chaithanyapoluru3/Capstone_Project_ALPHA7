# Capstone_Project_ALPHA7

# Performance Optimization of EE Architecture For Software Defined Vehicles

# Introduction
In embedded systems, communication between devices is crucial for data transfer, synchronization, and control. This project focuses on three widely-used communication protocols: CAN (Controller Area Network), I2C (Inter-Integrated Circuit), and SPI (Serial Peripheral Interface).   
<br> CAN is designed for real-time data transfer in noisy environments, widely employed in automotive and industrial systems. I2C, a simple two-wire protocol, facilitates low-speed communication between a master and multiple slave devices, making it ideal for embedded systems like sensors and displays. SPI, a high-speed full-duplex protocol, is used for efficient, low-latency communication between a master and peripherals like memory devices and sensors. 
                         Together, these protocols form the backbone of communication in modern embedded systems, enabling seamless data exchange across various applications.


# Overview of the problem statement
  The increasing complexity of embedded systems in fields like automotive, industrial automation, and consumer electronics demands reliable, efficient, and real-time communication between multiple devices. Systems require communication protocols that can handle various data transmission needs, from high-speed, low-latency operations to robust, fault-tolerant interactions in noisy environments.
   This project addresses the challenge of designing, simulating, and validating three key communication protocols—CAN, I2C, and SPI—using Verilog. Each protocol has its own strengths and specific use cases, but integrating and understanding their functionality through simulations provides crucial insights into their behavior and performance in real world applications.

                        

# Methodology
The methodology involves designing the CAN, I2C, and SPI protocols in Verilog, followed by extensive simulations to validate their functionality and performance through waveform analysis.
                 

## I2C (Inter Integrated Circuit)
I2C stands for Inter-Integrated Circuit. It is a bus interface connection protocol incorporated into devices for serial communication. It was originally designed by Philips Semiconductor in 1982. Recently, it is a widely used protocol for short-distance communication. It is also known as Two Wired Interface (TWI).
Working of I2C Communication Protocol:
•	It uses only 2 bi-directional open-drain lines for data communication called SDA and SCL. Both these lines are pulled high.
•	Serial Data (SDA) – Transfer of data takes place through this pin.
•	Serial Clock (SCL) – It carries the clock signal.
•	I2C operates in 2 modes –
                    Master mode
                    Slave mode
•	Each data bit transferred on SDA line is synchronized by a high to the low pulse of each clock on the SCL line.

![image](https://github.com/user-attachments/assets/17045c9e-fd99-44d8-8631-e049f5d2f161)
According to I2C protocols, the data line cannot change when the clock line is high, it can change only when the clock line is low. The 2 lines are open drain, hence a pull-up resistor is required so that the lines are high since the devices on the I2C bus are active low. The data is transmitted in the form of packets which comprises 9 bits. The sequence of these bits are  –
                                                        Start Condition – 1 bit
                                                        Slave Address – 8 bit
                                                         Acknowledge – 1 bit
          ![image](https://github.com/user-attachments/assets/f8fe90ad-27fc-498f-a6b8-1a8f69e459f2)
          With I2C, data is transferred in messages. Messages are broken up into frames of data. Each message has an address frame that contains the binary address of the slave, and one or more data frames that contain the data being transmitted. The message also includes start and stop conditions, read/write bits, and ACK/NACK bits between each data frame:
 Start Condition: The SDA line switches from a high voltage level to a low voltage level    before the SCL line switches from high to low.
 Stop Condition: The SDA line switches from a low voltage level to a high voltage level after the SCL line switches from low to high.
Address Frame: A 7or10 bit sequence unique to each slave that identifies the slave when the master wants to talk to it.
Read/Write Bit: A single bit specifying whether the master is sending data to the slave (low voltage level) or requesting data from it (high voltage level).
ACK/NACK Bit: Each frame in a message is followed by an acknowledge/no-acknowledge bit. If an address frame or data frame was successfully received, an ACK bit is returned to the sender from the receiving device.

          


                                                        







