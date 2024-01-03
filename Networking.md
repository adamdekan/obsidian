To add a route in Linux, you can use the `ip route add` command. Given your scenario, if you want to add a route to the 10.11.11.0/24 network via the eth0 interface, here's the command:

```sh
sudo ip route add 10.11.11.0/24 dev eth0
```

Explanation:
- `ip route add`: This is the command to add a route.
- `10.11.11.0/24`: The destination network in CIDR notation.
- `dev eth0`: Specifies that the route should use the eth0 interface.

After executing this command, your Linux system should be able to communicate with devices on the 10.11.11.0/24 network.

Remember to replace `eth0` with the actual name of your network interface if it's different. Also, keep in mind that this route will be temporary and will not persist across reboots. To make it permanent, you'll need to configure your network settings accordingly, usually in a configuration file like `/etc/network/interfaces` or using network manager tools.