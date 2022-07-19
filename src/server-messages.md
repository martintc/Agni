# Server messages

This is the definition of the server side, or target side of the protocol.

## Packet format

The server messages follow a simple format. The packet has a header and a message body. The following is the header.

| Field  | Size (bytes) |
|:-------|:-------------|
| Status | 1            |
| Size   | 8            |

The status can only take one of two values, so a single byte is sufficient for defining the status. The status can be the following:

| Status  | Value |
|:--------|:------|
| Ok      | 0x01  |
| Failure | 0x02  |

Following the status is the size. The size is number of bytes that are expected to be in the body. The choice of 8 bytes is to be sufficient to transfer a large amount of data for the source side and keep the header the same size as the source header. If the target gets a size of 8 bytes to expect but receives only 7 bytes for the body or receives more than 8 bytes for the body, the target should send back a failure message.

## Responding to Hello

The response to an hello initial method call could return a 0x01 for status. The size should be 5 and the message body should read "Hello". The 5 for the size represents the 5 bytes it takes to represent "Hello" in ASCII.

In hex, this response would look like:
```
0x01 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x05 0x48 0x65 0x6C 0x6C 0x6F

```

If a failure occured, the failure status can be sent back with a size of zero.
```
0x02 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00
```

## Responding the insert and reads

The response to insert and reads is simple. Send back the status and a size of zero.

Success case example in hex:
```
0x01 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00
```

Failure case example is  hex:
```
0x02 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00
```

## Responding to ready to transmit

This method is sent when binary data is to be transmitted in bulk and written into memory, such as in the case of receving a kernel to chain load. This is the recommended first step in the process to verify the connection and sizes. The source machine will send a packet that has the size of the expected binary data to send to the target. The response the target is to send back is a success and to repeat the size.

Suppose a source intended to send 1,000 bytes of code. The response from the source machine would look like:

```
0x01 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x08 0x00 0x00 0x00 0x00 0x00 0x00 0x03 0xE8
```

To keep it simple, the size of the body will be 8 bytes since the maximum number of memory address or file size tha can be handle by the target is theoretically 8 bytes. We can use a long data type, or u64, to represent both the size and the message body. So a table with this would look like:

| Field  | Size (bytes) |
|:-------|:-------------|
| Status | 1            |
| Size   | 8            |
| Body   | 8            |

## Responding to binary transmission

At the end of the binary transmission. The target device will send a message formatted as such:

| Field  | Size (bytes) |
|:-------|:-------------|
| Status | 1            |
| Size   | 8            |
| Body   | 8            |

A status code is sent to confirm that the transmission is complete on the target end, or a failure status code if not. The size will be the size of the message body. The message body is expected to be the cound of bytes received. This should match with the initial size the source notified to target of. This can be used as a check to know if to send a success or failure case, it also allows for the source to verify at the end of the transmission that the number of bytes sent are the same number bytes received. It should look similar to the packet the target sent the source as a response to the ready to transmit packet.







