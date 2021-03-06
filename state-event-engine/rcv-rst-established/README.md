# Handling of TCP Segments with the RST-bit Set in the ESTABLISHED State

## Description
This set of tests focuses on the handling of RST-segments in the `ESTABLISHED` state.

[RFC 0793](https://tools.ietf.org/html/rfc0793) requires RST-segments to be accepted if and only if
`RCV.NXT <= SEG.SEQ < RCV.NXT+RCV.WND` holds.

For mitigating blind attacks, [RFC 5961](https://tools.ietf.org/html/rfc5961#section-3)
requires the RST-segments only to be accepted if and only if `RCV.NXT = SEG.SEQ` holds.
In case of `RCV.NXT < SEG.SEQ < RCV.NXT+RCV.WND`, a challenge ACK has to be sent.

In FreeBSD, the `sysctl`-variable `net.inet.tcp.insecure_rst` can be used to
select if procedures described in [RFC 0793](https://tools.ietf.org/html/rfc0793) or
[RFC 5961](https://tools.ietf.org/html/rfc5961#section-3) are followed.
The default is to follow [RFC 5961](https://tools.ietf.org/html/rfc5961#section-3).

## Status

| Name                                                                                                                                                                                                                                    | Result FreeBSD 11.0 | Result FreeBSD Head |
|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------:|:-------------------:|
|[rcv-rst-established-outside-left-secure-ipv4](rcv-rst-established-outside-left-secure-ipv4.pkt "Ensure that the reception of a TCP RST with SEG.SEQ=RCV.NXT-1 in the ESTABLISHED state does not affect the TCP connection")             | Unknown             | Passed              |
|[rcv-rst-established-outside-left-secure-ipv6](rcv-rst-established-outside-left-secure-ipv6.pkt "Ensure that the reception of a TCP RST with SEG.SEQ=RCV.NXT-1 in the ESTABLISHED state does not affect the TCP connection")             | Unknown             | Passed              |
|[rcv-rst-established-left-edge-secure-ipv4](rcv-rst-established-left-edge-secure-ipv4.pkt "Ensure that the reception of a TCP RST with SEG.SEQ=RCV.NXT in the ESTABLISHED state destroys the TCP connection")                            | Unknown             | Passed              |
|[rcv-rst-established-left-edge-secure-ipv6](rcv-rst-established-left-edge-secure-ipv6.pkt "Ensure that the reception of a TCP RST with SEG.SEQ=RCV.NXT in the ESTABLISHED state destroys the TCP connection")                            | Unknown             | Passed              |
|[rcv-rst-established-right-edge-secure-ipv4](rcv-rst-established-right-edge-secure-ipv4.pkt "Ensure that the reception of a TCP RST with SEG.SEQ=RCV.NXT+RCV.WND-1 in the ESTABLISHED state triggers the sending of a challenge ACK")    | Unknown             | Passed              |
|[rcv-rst-established-right-edge-secure-ipv6](rcv-rst-established-right-edge-secure-ipv6.pkt "Ensure that the reception of a TCP RST with SEG.SEQ=RCV.NXT+RCV.WND-1 in the ESTABLISHED state triggers the sending of a challenge ACK")    | Unknown             | Passed              |
|[rcv-rst-established-outside-right-secure-ipv4](rcv-rst-established-outside-right-secure-ipv4.pkt "Ensure that the reception of a TCP RST with SEG.SEQ=RCV.NXT+RCV.WND in the ESTABLISHED state does not affect the TCP connection")     | Unknown             | Passed              |
|[rcv-rst-established-outside-right-secure-ipv6](rcv-rst-established-outside-right-secure-ipv6.pkt "Ensure that the reception of a TCP RST with SEG.SEQ=RCV.NXT+RCV.WND in the ESTABLISHED state does not affect the TCP connection")     | Unknown             | Passed              |
|[rcv-rst-established-outside-left-insecure-ipv4](rcv-rst-established-outside-left-insecure-ipv4.pkt "Ensure that the reception of a TCP RST with SEG.SEQ=RCV.NXT-1 in the ESTABLISHED state does not affect the TCP connection")         | Unknown             | Passed              |
|[rcv-rst-established-outside-left-insecure-ipv6](rcv-rst-established-outside-left-insecure-ipv6.pkt "Ensure that the reception of a TCP RST with SEG.SEQ=RCV.NXT-1 in the ESTABLISHED state does not affect the TCP connection")         | Unknown             | Passed              |
|[rcv-rst-established-left-edge-insecure-ipv4](rcv-rst-established-left-edge-insecure-ipv4.pkt "Ensure that the reception of a TCP RST with SEG.SEQ=RCV.NXT in the ESTABLISHED state destroys the TCP connection")                        | Unknown             | Passed              |
|[rcv-rst-established-left-edge-insecure-ipv6](rcv-rst-established-left-edge-insecure-ipv6.pkt "Ensure that the reception of a TCP RST with SEG.SEQ=RCV.NXT in the ESTABLISHED state destroys the TCP connection")                        | Unknown             | Passed              |
|[rcv-rst-established-right-edge-insecure-ipv4](rcv-rst-established-right-edge-insecure-ipv4.pkt "Ensure that the reception of a TCP RST with SEG.SEQ=RCV.NXT+RCV.WND-1 in the ESTABLISHED state destroys the TCP connection")            | Unknown             | Passed              |
|[rcv-rst-established-right-edge-insecure-ipv6](rcv-rst-established-right-edge-insecure-ipv6.pkt "Ensure that the reception of a TCP RST with SEG.SEQ=RCV.NXT+RCV.WND-1 in the ESTABLISHED state destroys the TCP connection")            | Unknown             | Passed              |
|[rcv-rst-established-outside-right-insecure-ipv4](rcv-rst-established-outside-right-insecure-ipv4.pkt "Ensure that the reception of a TCP RST with SEG.SEQ=RCV.NXT+RCV.WND in the ESTABLISHED state does not affect the TCP connection") | Unknown             | Passed              |
|[rcv-rst-established-outside-right-insecure-ipv6](rcv-rst-established-outside-right-insecure-ipv6.pkt "Ensure that the reception of a TCP RST with SEG.SEQ=RCV.NXT+RCV.WND in the ESTABLISHED state does not affect the TCP connection") | Unknown             | Passed              |
