.. role:: red
.. role:: bred

Lab 2.3: Testing
----------------

For **gateway-mode** testing, the lab's inbound desktop client includes static
Hosts entries that match the real IPs of the internal web server:

- test0.f5labs.com = 10.20.0.90
- test1.f5labs.com = 10.20.0.91
- test2.f5labs.com = 10.20.0.92
- test3.f5labs.com = 10.20.0.93
- test4.f5labs.com = 10.20.0.94

And a static persistent route that points 10.20.0.0/24 traffic to the BIG-IP
outbound (external) VLAN self-IP (10.30.0.100).

For **targeted-mode** testing, add a static host entry in the local Windows
hosts file (C:\Windows\System32\drivers\etc\hosts) for:

- www.f5labs.com = 10.30.0.200
