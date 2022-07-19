# Introduction

Agni is protcol designed to be used for transfering information between a developer's laptop and a target environment. Specifically, it is designed to be a protocol for Sita and Ganapati. Sita is a basic kernel written in C for the Raspberry Pi 4B that provides a UART driver and a way to interact with a developer's machine over a serial connection. Sita will allow a developer to insert into memory, read from memory and load code into memory over serial connection. Ganapati is a client program on a developer machine that will interface with Sita, using the Agni protocol. The Agni protocol is designed to be simple and too the point. 

On the client side, Agni provides a header with two fields, followed by a message body. For server side, the Agni protocol has a header with a two field followed also by a message body. 
