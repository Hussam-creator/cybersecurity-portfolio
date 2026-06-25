
![[Pasted image 20260415115032.png]]

```
./proxy -selfcert
```
![[Pasted image 20260415115043.png]]

On pivot machine
```
.\agent.exe -connect 192.168.141.128:11601 -ignore-cert
```
![[Pasted image 20260415115118.png]]

On proxy
![[Pasted image 20260415115135.png]]

On kali
Select session
```
session
```
![[Pasted image 20260415115149.png]]

Setup pivot
Add to our outing table
```
sudo ip route add 192.168.141.0/24 dev ligolo
```
![[Pasted image 20260415115202.png]]

For double pivot
-> Add a second TUN interface

```shell
sudo ip tuntap add user kali mode tun ligolo-double
sudo ip link set ligolo-double up
```
-> Create a listener
```shell
listener_add --addr 0.0.0.0:11601 --to 127.0.0.1:11601 --tcp
listener_list
```
-> Connect to the proxy server
```shell
./agent.exe -connect <IP of First Pivot Point>:11601 -ignore-cert
```
-> Start a tunnel and add a route
```shell
sudo ip add route <New_Network> dev ligolo-double
```