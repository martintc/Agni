# Extending the Protocol

Agni is meant to be simple and to be easily expandable and changeable.

## Changing Agni to fit target

Agni can be made to fit the target being targeted. While Agni assuming an environment that can use 64 bit values, the protocol can also be easily changed to bit a different environment. An example would be using Agni with an 8 bit microcontroller. It would make sense to have the size header field use a single byte instead of eight bytes for the size field.

## Adding more to message

While the messages are slim in the protocol definition, the implementor can easily add more or change the messages. An example to this might be adding more information to the body of the target response after getting binary data transmitted such as the address where it was placed in memory. On the source side, it would also be reasonable to set an address for the target to start dumping binary data at in the ready to transmit method. Sending a size and a starting address.
