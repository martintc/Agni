
# Client Messages

## The header

The header for the client messages are similar to the headers for the target (or server). The main difference is that instead of a status field, the source (or client) has a method field.

| Field  | Size (bytes) |
|:-------|:-------------|
| Method | 1            |
| Size   | 8            |

Like the headers for the target, the size specifies the size of the message body with 8 bytes to be able to determine a large enough value. The size represents the number of bytes that are in the message body.

The method expects a certain value depending on the method to be send. Below is a table the methods Agni provides and the values to represent that method.

| Method              | Value (hex) |
|:--------------------|:------------|
| Hello               | 0x01        |
| Insert              | 0x02        |
| Read                | 0x03        |
| Prepare to transmit | 0x04        |
| Binary transmission | 0x05        |

## Hello

The Hello method is for establishing communication and they everything is working properly between the source and target devices. The source should send a message with the Hello method as the method in the header, a size of 5 in the 8 byte size block and the word "Hello." The following is an example in hex of what this should look like:

```
0x01 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x05 0x48 0x65 0x6C 0x6C 0x6F
```

The source should expect to get back a similar message from the target.

## Insert

The insert method is used for telling the target to insert data at a specific address. The method byte should be `0x02`, the size should be 16. The reason for 16 is in the message body, the first 8 bytes should declare the address in memory to place data at on the target device and the last 8 bytes should be the value, or data, to put at the address.

```
0x02 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x10 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x01 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x01
```

Above is an example of an insert in hex. To break it down:

```
0x02
```
Is the byte specifying that this packet is calling the insert method.


```
0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x10
```
The size here is 16 in hex, telling the target to expect to receive 16 bytes for the message body.

```
0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x01
```
These next 8 bytes are in the message body and specify the memory address of the target to place data at. In this example, data is to be placed in address 1 of the target.

```
0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x01
```
The last 8 bytes in the body is the value to place at the specified address. In this example, the value 1 will be place at address 1.

## Read

The read method allows the source to ask the target to send back what is store at the address asked for. The value in the size field should be 8 for 8 bytes in the body that will compose the value at an address.

## Ready to transmit

The read to transmit is used to signal the target that a large transfer of data is about to take place and to serve as a way to make sure the connection is good between both source and target. The ready to transmit packet send over the size of the data to be transmitted. Suppose a kernel image is to be chain loaded onto a target device over this protocol. That kernel image is about 8 megabytes in size. The size of that kernel image will be in the message body. The header size will still be 8 bytes since that is the largest theoretical value a 64-bit system can hold. The source should expect to get a packet back from the target signaling Ok as the status and repeat the size back. 

## Binary Transmission

This method is invoked after verification has been done via the ready to transmit. The size in the size field should be the size of the data to transmit and the body should be the data broken down into 1 byte chunks.
