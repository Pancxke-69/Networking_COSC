# Tunnels Example
```
BLH--------T1--------T2--------T3--------T4
```
###### BLH - 10.10.10.10 (SSH 22)
###### T1 - 1.1.1.1 (SSH 2222) (USERNAME STUDENT)
###### T2 - 2.2.2.2 (TELNET ONLY) (USERNAME STUDENT)
###### T3 - 3.3.3.3 (SSH 3333) (USERNAME COMRADE)
###### T4 - 4.4.4.4 (SSH 22) (USERNAME COMRADE)
### Establish Tunnel Between BLH and T1
* `ssh student@1.1.1.1 -L 11201:1.1.1.1:2222 -NT`
  - Notice the number `11201`, this is an important number. It is how you reference this tunnel.
  - What this tunnel is essentially doing is, connecting to `1.1.1.1` and is saving the connection inside of port `11201` on BLH.
  - You can run `ss -ntlp` to check the connections, and you will find port `11201` associated with your loopback.
### Establish a dynamic tunnel on T1 (1.1.1.1)
* `ssh student@127.0.0.1 -p11201 -D9050 -NT`
  - This connects to the tunnel you made from BLH to T1 by referencing the port associated with the tunnel along with the loopback.
  - To add proxychains to any tunnel you type `-D9050` everytime (port 9050 is the default port for proxychains)
##### A Quick Look
* If you want to ssh directly to any device's terminal where a tunnel is established You can ssh directly using the username, loopback, and the port of the tunnel.
* `ssh student@127.0.0.1 -p11201`
### Establish Tunnel to T2 (This one is more steps since only telnet is open)
* Establish a tunnel that points to the telnet port on T2 so you can telnet directly from BLH by not using proxychains.
* `ssh student@127.0.0.1 -p11201 -L 11202:2.2.2.2:23`
  - This is referencing the port to the tunnel between BLH and bridging it to the next device on port 23 for telnet
  - If you didnt do this and used proxychains telnet (ip) (port) then you would lock proxychains into a tunnel, and wouldnt be able to use it for anything else.
* Telnet to T2 using the loopback and the port of the tunnel
  - `telnet 127.0.0.1 11202`
    - This will get you into T2 terminal
* Establish a tunnel that goes backwards and connects T1 to T2 from T2
  - You have to use a reverse tunnel to the port you make for the tunnel goes onto T1
  - `ssh student@1.1.1.1 -p2222 -R 11211:127.0.0.1:22`
    - This is telling T1 to connect to T2 and putting port `11211` on T1.
    - Remember T1's alternate ssh port is `2222` so we have to put `-p2222` so it knows what port to use ssh on
* Establish a tunnel that connects BLH to T2
  - Since you used a reverse tunnel, it is facing a different direction. ---><--- instead of --->--->
  - You have to make them face the same way
  - `ssh student@127.0.0.1 -p11201 -L 11203:127.0.0.1:22`
