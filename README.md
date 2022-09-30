# TCP version of Enigma-72 exercise

This document describes an exercise on TCP socket programming, used in the
course [IDATA2304 Computer networks](https://www.ntnu.edu/studies/courses/IDATA2304),
[NTNU](https://www.ntnu.edu/), Campus Ålesund.

## Intention

The intention is to practice TCP socket programming.

## Description

Remember the [UDP-version of the "Enigma 72" exercise](https://github.com/ntnu-datakomm/enigma-72)? Well, this is the
TCP version of it.

The point is to experience how the programming is similar and how is it different when we use TCP protocol for data
transfer with sockets. Remember, what the differences between UDP and TCP are? Did I hear you saying "UDP is
connection-less and sends datagrams while TCP sets ups a connection and exchanges data using streams"? You are
absolutely right! :)

## The task at hand

The objective is exactly the same as you had in the original "Enigma 72" exercise - you receive a sentence from
the server. Then you need to find out two things about the sentence - whether it is a statement or a question, and how
many words does it contain? You send your answer to the server. The server then replies with either `ok` or `error`.

**The task in this exercise: implement a TCP client that works according to the protocol!**

## Modifications in the protocol

To make the task easier, slight modifications are introduced in the protocol. The main difference: each message sent by
either the server or the client is terminated with a terminating symbol `/`. Here you can see one advantage of UDP. With
UDP datagrams you did not need to worry about finding message boundaries. One message is encapsulated in one datagram,
that's it. When using TCP streams, you may receive the messages in several chunks, not all at once. You need to make
sure you split the bytes from the stream into messages correctly.

In summary, the protocol is as follows:

1. The client establishes connection to the server. For testing: use server running at the address `129.241.152.12`, TCP
   port number `1234`.
2. The client then asks for a task sending a message `task/` to the server. Pay attention to the `/` symbol - that is a
   terminator symbol used for all the messages sent by both the client and the server. (No relation with this
   [terminator](https://en.wikipedia.org/wiki/The_Terminator#/media/File:Terminator1984movieposter.jpg))
3. The server then sends a sentence to the client, again - followed by the `/` terminator. An example
   sentence: `What is the meaning of life?/`
4. The client needs to decide the sentence type (`statement` or `question`) and count the number of words.
5. The client then sends the response to the server in the format `<type> <wordCoumt>/`. In this case, the client would
   send the following to the server: `question 6/`
6. The server will send `ok/` to the client if the answer was correct or `error/` if the answer was not correct.
7. The client can repeat steps 2-6 as many times as desirable.
8. In the end the client closes the socket.

## Extra challenge

If you want an extra challenge, implement the server side of the solution as well! Here you will need multi-threading as
well. If you need some hints, you can take a look at
the [source of the teacher-made TCP task server](https://github.com/ntnu-datakomm/server-side/tree/main/socket-assignment-server)
. Clone the project and start the exploration at
the [`TcpServerRunner` class](https://github.com/ntnu-datakomm/server-side/blob/main/socket-assignment-server/src/main/java/no/ntnu/TcpServerRunner.java)
. BTW, this project contains code for both the UDP Task server and TCP Task server. Did you notice that the
[`Logic` class](https://github.com/ntnu-datakomm/server-side/blob/main/socket-assignment-server/src/main/java/no/ntnu/Logic.java)
is shared among both?

## Hints

You may want to read the messages from the stream byte by byte.

Also, you can take a look at
the [TCP Task server code](https://github.com/ntnu-datakomm/server-side/tree/main/socket-assignment-server), perhaps you
get some hints there. At the end of the day - handling of TCP streams (sending and receiving data) is the same on both
ends: the server and the client. Only the connection establishment is different.

## Testing

There is a Task-server running on a virtual server with IP address `129.241.152.12`, TCP port `1234`. You can connect
your client to it for testing.

P.S. Noticed that the port number is the same for UDP Task server and TCP Task server? That is not a problem, because
TCP ports and UDP ports are two different things. In this case, there are two processes running on the server - one is
listening on UDP port `1234` and the other is listening on TCP port `1234`. They are not conflicting in any way.

## Hand-in

This is an optional exercise, no need to hand in anything. But in case you have solved the exercise, it could be nice to
have a chat with the teachers during one of the lab sessions - show your solution, discuss the design choices. Both you
and the teachers can learn from each other! :)
