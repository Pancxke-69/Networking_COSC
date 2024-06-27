[TUNNEL EXAMPLE]
telnet in:
/> ssh user@10.50.23.21 -R 11601:localhost:23
Blue Host:
/> ssh user@localhost  -p 1602 -L 11603:localhost:2222
/> ssh user@localhost -p 1603 -L 11604:172.16.10.121:2223
/> ssh user@localhost -p 11604 -L 11605:192.168.10.69:22
/> ssh user@localhost -p 11605 -D 9050

[CFT TUNNELS]
---------------------------------------------
lin-ops  |    t1   |   t2   |   t3    |  t4
         |    23   |   2222 |   5555  |  23
---------------------------------------------
telnet in:
/> ssh user@<lin-ops> -R 11201:localhost:22
  > # SSH is disabled, so we need to telnet in and establish a reverse tunnel
lin-ops:
/> ssh user@localhost -p 11201 11202:t2:2222
  > # Establish a tunnel to t2 through port '2222'(ssh) using the tunnel above as a jump point
/> ssh user@localhost -p 11202 11203:t3:5555
  > # Establish a tunnel to t3 through port '5555'(ssh) using the tunnel above as a jump point
/> ssh user@localhost -p 11203 11204:t4:23
  > # Establish a tunnel to t4 through port '23' to allow telnet to t4 from lin-ops directly
telnet in:
/> ssh user@t3 -R 11211:localhost:22
  > # Once telnetted in, establish a remote tunnel that opens port '22' on t4 from t3 to t4
lin-ops:
/> ssh user@localhost -p 11203 11204:localhost:11211
  > # Combine the local and reverse tunnels into one tunnel so we can reference it. It takes port '11203' which is the port from the t3 tunnel and combines it with port '11211' from the reverse tunnel
/> ssh user@localhost -p 11204 -D 9050
  > # Establish a Dynamic tunnel so we can use proxychains on t4
