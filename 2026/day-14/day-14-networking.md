# Day 14 – Networking Fundamentals & Hands-on Checks

## Quick Concepts

### OSI vs TCP/IP

OSI (7 Layers):
1. Physical
2. Data Link
3. Network
4. Transport
5. Session
6. Presentation
7. Application

TCP/IP (4 Layers):
- Link
- Internet
- Transport
- Application

IP → Internet layer  
TCP/UDP → Transport layer  
HTTP/HTTPS → Application layer  
DNS → Application layer  

Example:
curl https://example.com  
= HTTP (Application) over TCP (Transport) over IP (Internet)

---

## Hands-on Checks

### Identity
hostname -I

Observation:
Displayed my system IP address.

---

### Reachability
ping google.com

Observation:
Packets received, low latency, 0% packet loss.

---

### Path
traceroute google.com

Observation:
Multiple hops; some intermediate routers show timeout (*).

---

### Listening Ports
ss -tulpn

Observation:
SSH service listening on port 22.

---

### DNS Resolution
dig google.com

Observation:
Resolved domain to public IP address.

---

### HTTP Check
curl -I https://google.com

Observation:
HTTP/1.1 200 OK status received.

---

### Connections Snapshot
netstat -an | head

Observation:
Saw LISTEN and ESTABLISHED states.

---

## Mini Port Probe

Test SSH port locally:

nc -zv localhost 22

Result:
Connection successful.

Next check if failed:
- systemctl status ssh
- Check firewall rules

---

## Reflection

Fastest signal command:
ping (quick connectivity check)

If DNS fails:
Check DNS resolution (Application layer).

If HTTP 500:
Check application logs and service status.

---

## Two Follow-Up Checks in Incident

1. systemctl status <service>
2. journalctl -u <service> -n 50
