# 📊 TCPDump Packet Analysis - SYN Flood Incident

This document analyzes the TCP packet capture obtained during the suspected denial-of-service event.

## 🔍 Summary of Observations

The packet capture shows a high volume of incomplete TCP handshakes between a single external IP address (`203.0.113.0`) and the internal web server (`192.0.2.1`) on port 443 (HTTPS).

### Key Indicators:

- Repeated TCP packets with the `[SYN]` flag from `203.0.113.0` to `192.0.2.1`
- No corresponding ACK packets to complete the handshake
- The same source port (`54770`) is used consistently, suggesting spoofing or automation
- Logs span multiple seconds with sustained traffic volume

## 🧪 Sample Log Entries

```
203.0.113.0 -> 192.0.2.1 TCP 54770→443 [SYN]
203.0.113.0 -> 192.0.2.1 TCP 54770→443 [SYN]
203.0.113.0 -> 192.0.2.1 TCP 54770→443 [SYN]
...
```

These are typical of a **TCP SYN Flood**, where the attacker initiates many connections but never completes them.

## ⚠️ Technical Impact

- The server allocates memory and resources for each incoming SYN
- Since no ACK is received, these connections remain half-open
- The connection table fills up, leading to refusal of new connections
- End-users experience timeouts or receive `504 Gateway Timeout` errors

## 📌 Conclusion

The pattern and behavior strongly confirm a **TCP SYN Flood DoS attack**. The attack was likely automated and intended to exhaust server resources and interrupt service availability.
