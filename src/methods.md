# Methods

Agni currently supports 5 methods for the client side to invoke on the target device. 

- hello
- insert
- read
- prepare to receive
- binary transmission

This section will briefly describe the methods and purpose, see the section on client messages for more information regarding the client side implementation and the server messages page for server side.

## Hello

The hello method should be at the start of every new interaction between the msource machine (developer's machine) and the target machine (a Raspberry Pi 4B running Sita). This method essentailly just serves the purpose of ensuring there is a connection between the two device. The client will send a message to the target machine, the target machine will see the message and respond. This ensures that the connection is correct (baud rates are right) and the serial communication is working.


## Insert

Using the insert method will allow the source machine to specify an address and value for the target machine to place. The client will specify the memory address and value to be placed. This can be useful for debugging and troubleshooting. As an example, if the target is a microcontroller such as the AVR Atmega328P, the target machine can send data to the target device to write into memory addresses to activate something such as a hooked up LED circut. Doing this can allow the developer to verify that their write to hardware registers were correct before writing it into code.

## Read

Read is used for getting a value from memory of the target machine. The client machine will specify a memory address that they want to get the value of from the target machine. This can verify an insert, data transmission or be used to check values under other conditions. 

## Prepare to receive and binary transmission

These two methods are tied together. Before performing the transmission, a prepare to receive should occur before.

### Prepare to transmit

The prepare to transmit method's purpose it to kick start the process of transfering binary information in long form to the target machine. An example could be a kernel that the developer intends to chain load. The prepare method will notify the target machine that a large transfer is about to happen. The source machine will also send the size of the transmission that will take place. The target machine will acknowledge and repeat back the size. This is a final verification that the serial connection is correct before starting the transfer.

### Binary Transmission

This method is invoked when sending over large amounts of data to be written into memory. An example being a code routine that will be chain loded. The information sent over this method should be compiled source code. An example, using Sita and Ganapati as an example, a development kernel for the Raspberry Pi 4B can be compiled to a `.img` file and sent over. The target machine will write this into memory.
