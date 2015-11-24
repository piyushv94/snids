SNIDS: Simple Network IDS

A simple Intrusion Detection System (IDS), written in Java, using the jpcap library.

To create such a network intrusion detection system, you’ll need access to a machine with superuser privileges that will allow you to sniff and filter traffic. To avoid this problem, in this program you can work on network traces. It should relatively be a straightforward and simple procedure to adapt the system you build to real network traffic on your own machine.

The program "snids" takes two parameters: a regex based rules file and the trace to check for viruses.

A rules file must have one (and only one) host entry which denotes the host that the IDS is running on and arbitrarily many rule entries.

```
<host> ::= host=<ip>\n\n
<rule> ::= name=<string>\n
<tcp_stream_rule>|<tcp_protocol_rule>\n
<tcp_stream_rule> ::= type=stream\n
local_port=(any|<port>)\n
remote_port=(any|<port>)\n
ip=(any|<ip>)\n
(send|recv)=<regexp>\n
<tcp_protocol_rule> ::= type=protocol\n
proto=tcp|udp\n
local_port=(any|<port>)\n
remote_port=(any|<port>)\n
ip=(any|<ip>)\n
<sub_rule>
<sub_rule>*
<sub_rule> ::= (send|recv)=<regexp> (with flags=<flags>)?\n
<string> ::= alpha-numeric string
<ip> ::= string of form [0-255].[0-255].[0-255].[0-255]
<port> ::= string of form [0-65535]
<regexp> ::= Perl Regular Expression
<flags> ::= <flag>*
<flag> ::= S|A|F|R|P|U
```
ip, remote port, recv all refer to the remote site of the network connection. Also a **denotes zero or more repetitions and a ? denotes zero or one repetitions. TCP flag can be one of six values: a) F : FIN - Finish b) S : SYN - Synchronize; c) R : RST - Reset 4) P : PUSH - Push 5) A : ACK - Acknowledgement 6) U : URG - Urgent.**

A rule is one of two types: stream rules and protocol rules.

EXAMPLES

1) For example the following rule looks for the “I love you” virus in email payload.
```
host=192.168.1.1
name=I Love You
type=protocol
proto=tcp
local_port=25
remote_port=any
ip=any
recv="ILOVEYOU"
```

2) Another example involves matching all plaintext POP login sessions to a mail server.
```
host=192.168.1.1
name=Plaintext POP
type=protocol
proto=tcp
local_port=110
remote_port=any
ip=any
send="\+OK.*\r\n"
recv="USER .*\r\n"
send="\+OK.*\r\n"
recv="PASS.*\r\n"
send="\+OK.*\r\n"
```

The program would print out to stdout the virus name found in the trace given as parameter.

Simple test cases are given.

--- BUILD ---

1) Make sure you have 'gcc' and 'make' installed to compile Jpcap.

2) Other software/packages may be necessary (for example, you need 'build-essential' package to install on Ubuntu).

3) Download and install libpcap (ver.0.9.4 or later) if not installed.

4) Move to (co\_path)/lib/jpcap-0.7/src/c path and execute "make" command

5) Move to (co\_path) and execute command "make"

6) Execute command "./test\_cases" to start a test