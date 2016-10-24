
# Testing the State-Event-Engine of TCP

## Description
The following diagram from  [RFC 793, Section 3.2](https://tools.ietf.org/html/rfc793#section-3.2)
with corrections from [RFC 1122, Section 4.2.2.8](https://tools.ietf.org/html/rfc1122#section-4.2.2.8)
shows the TCP state-event-engine.
```
                                       +---------+ ---------\      active OPEN
                                       |  CLOSED |            \    -----------
                                       +---------+<---------\   \   create TCB
                                         |     ^              \   \  snd SYN
                            passive OPEN |     |   CLOSE        \   \
                            ------------ |     | ----------       \   \
                             create TCB  |     | delete TCB         \   \
                                         V     |                      \   \
                        rcv RST        +---------+            CLOSE    |    \
                 --------------------->|  LISTEN |          ---------- |     |
               /                       +---------+          delete TCB |     |
               |            rcv SYN      |     |     SEND              |     |
               |           -----------   |     |    -------            |     V
          +---------+      snd SYN,ACK  /       \   snd SYN          +---------+
          |         |<-----------------           ------------------>|         |
          |   SYN   |                    rcv SYN                     |   SYN   |
          |   RCVD  |<-----------------------------------------------|   SENT  |
          |         |                  snd SYN,ACK                   |         |
          |         |------------------           -------------------|         |
          +---------+   rcv ACK of SYN  \       /  rcv SYN,ACK       +---------+
               |        --------------   |     |   -----------
               |               x         |     |     snd ACK
               |                         V     V
        CLOSE  |                       +---------+
       ------- |                       |  ESTAB  |
       snd FIN |                       +---------+
               |                CLOSE    |     |    rcv FIN
               V               -------   |     |    -------
          +---------+          snd FIN  /       \   snd ACK          +---------+
          |  FIN    |<-----------------           ------------------>|  CLOSE  |
          | WAIT-1  |---------------------                           |   WAIT  |
          +---------+                      \   rcv FIN               +---------+
rcv ACK of FIN ||                           |  -------                    |  CLOSE 
-------------- | \                          |  snd ACK                    | -------
       x       V   ---    rcv FIN,ACK       V                             V snd FIN
          +---------+  \  -----------  +---------+                   +---------+
          |FINWAIT-2|   |   snd ACK    | CLOSING |                   | LAST-ACK|
          +---------+   \              +---------+                   +---------+
               |          --------------    |                             |
               |                         \  | rcv ACK of FIN              | rcv ACK of FIN
       rcv FIN |                          | | --------------              | --------------
       ------- |                          V V        x                    V       x
       snd ACK  \                      +---------+                   +---------+
                  -------------------->|TIME WAIT|------------------>| CLOSED  |
                                       +---------+   Timeout=2MSL    +---------+
                                                     ------------
                                                      delete TCB 
```
[RFC 1337](https://tools.ietf.org/html/rfc1337) describes the handling of RST segments
in the TIME-WAIT state.
[RFC 5961](https://tools.ietf.org/html/rfc5961) improves the handling of SYS and RST segments.

## Status

|                 | SYN                                | SYN-ACK                                | SYN-FIN                                | ACK                                | FIN                                | FIN-ACK                                | RST                                | RST-ACK                                |
|:----------------|:----------------------------------:|:--------------------------------------:|:--------------------------------------:|:----------------------------------:|:----------------------------------:|:--------------------------------------:|:----------------------------------:|:--------------------------------------:|
|**CLOSED**       | [x](rcv-syn-closed/README.md)      | [x](rcv-syn-ack-closed/README.md)      | [x](rcv-syn-fin-closed/README.md)      | [x](rcv-ack-closed/README.md)      | [x](rcv-fin-closed/README.md)      | [x](rcv-fin-ack-closed/README.md)      | [x](rcv-rst-closed/README.md)      | [x](rcv-rst-ack-closed/README.md)      |
|**LISTEN**       | [x](rcv-syn-listen/README.md)      | [x](rcv-syn-ack-listen/README.md)      | [x](rcv-syn-fin-listen/README.md)      | [x](rcv-ack-listen/README.md)      | [x](rcv-fin-listen/README.md)      | [x](rcv-fin-ack-listen/README.md)      | [x](rcv-rst-listen/README.md)      | [x](rcv-rst-ack-listen/README.md)      |
|**SYN-SENT**     | [x](rcv-syn-syn-sent/README.md)    | [x](rcv-syn-ack-syn-sent/README.md)    | [x](rcv-syn-fin-syn-sent/README.md)    | [x](rcv-ack-syn-sent/README.md)    | [x](rcv-fin-syn-sent/README.md)    | [x](rcv-fin-ack-syn-sent/README.md)    | [x](rcv-rst-syn-sent/README.md)    | [x](rcv-rst-ack-syn-sent/README.md)    |
|**SYN-RCVD**     | [x](rcv-syn-syn-rcvd/README.md)    | [x](rcv-syn-ack-syn-rcvd/README.md)    | [x](rcv-syn-fin-syn-rcvd/README.md)    | [x](rcv-ack-syn-rcvd/README.md)    | [x](rcv-fin-syn-rcvd/README.md)    | [x](rcv-fin-ack-syn-rcvd/README.md)    | [x](rcv-rst-syn-rcvd/README.md)    | [x](rcv-rst-ack-syn-rcvd/README.md)    |
|**ESTABLISHED**  | [x](rcv-syn-established/README.md) | [x](rcv-syn-ack-established/README.md) | [x](rcv-syn-fin-established/README.md) | [x](rcv-ack-established/README.md) | [x](rcv-fin-established/README.md) | [x](rcv-fin-ack-established/README.md) | [x](rcv-rst-established/README.md) | [x](rcv-rst-ack-established/README.md) |
|**FIN-WAIT-1**   | [x](rcv-syn-fin-wait-1/README.md)  | [x](rcv-syn-ack-fin-wait-1/README.md)  | [x](rcv-syn-fin-fin-wait-1/README.md)  | [x](rcv-ack-fin-wait-1/README.md)  | [x](rcv-fin-fin-wait-1/README.md)  | [x](rcv-fin-ack-fin-wait-1/README.md)  | [x](rcv-rst-fin-wait-1/README.md)  | [x](rcv-rst-ack-fin-wait-1/README.md)  |
|**FIN-WAIT-2**   | [x](rcv-syn-fin-wait-2/README.md)  | [x](rcv-syn-ack-fin-wait-2/README.md)  | [x](rcv-syn-fin-fin-wait-2/README.md)  | [x](rcv-ack-fin-wait-2/README.md)  | [x](rcv-fin-fin-wait-2/README.md)  | [x](rcv-fin-ack-fin-wait-2/README.md)  | [x](rcv-rst-fin-wait-2/README.md)  | [x](rcv-rst-ack-fin-wait-2/README.md)  |
|**CLOSE-WAIT**   | [x](rcv-syn-close-wait/README.md)  | [x](rcv-syn-ack-close-wait/README.md)  | [x](rcv-syn-fin-close-wait/README.md)  | [x](rcv-ack-close-wait/README.md)  | [x](rcv-fin-close-wait/README.md)  | [x](rcv-fin-ack-close-wait/README.md)  | [x](rcv-rst-close-wait/README.md)  | [x](rcv-rst-ack-close-wait/README.md)  |
|**CLOSING**      | [x](rcv-syn-closing/README.md)     | [x](rcv-syn-ack-closing/README.md)     | [x](rcv-syn-fin-closing/README.md)     | [x](rcv-ack-closing/README.md)     | [x](rcv-fin-closing/README.md)     | [x](rcv-fin-ack-closing/README.md)     | [x](rcv-rst-closing/README.md)     | [x](rcv-rst-ack-closing/README.md)     |
|**TIME-WAIT**    | [x](rcv-syn-time-wait/README.md)   | [x](rcv-syn-ack-time-wait/README.md)   | [x](rcv-syn-fin-time-wait/README.md)   | [x](rcv-ack-time-wait/README.md)   | [x](rcv-fin-time-wait/README.md)   | [x](rcv-fin-ack-time-wait/README.md)   | [x](rcv-rst-time-wait/README.md)   | [x](rcv-rst-ack-time-wait/README.md)   |

## References
* [RFC 793: *Transmission Control Protocol*](https://tools.ietf.org/html/rfc0793)
* [RFC 1122: *Requirements for Internet Hosts -- Communication Layers*](https://tools.ietf.org/html/rfc1122)
* [RFC 1337: *TIME-WAIT Assassination Hazards in TCP*](https://tools.ietf.org/html/rfc1337)
* [RFC 5961: *Improving TCP's Robustness to Blind In-Window Attacks*](https://tools.ietf.org/html/rfc5961)