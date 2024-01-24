# Submask filtering
ip.addr == 10.10.10.0/24

# Setting a range
tcp.port in {88,389,636,8000..8010}

# Getting rid of a chatter f.e.
!(arp or stp or lldp or cdp or ath.addr==ff:ff:ff:ff:ff or dns or tcp.port in {443,80,20..23})

# Searching for a string everywhere
eth contains "String"

# Filtering conversations
RClick on packet -> Conversation Filter -> TCP

# Filter traffic between specific source and destination
RClick on packet -> Apply as filter -> Selected -> A <-> B

# Saving filteret packets
Menu -> Export Specified Packets

# TCP flags (possible to search for retransmissions)
(tcp.analysis.flags) && !(tcp.analysis.window_update)

# Slow dns responses (first step in slow response analytics)
dns.time > 0.010

# GeoIp country selection
ip.geoip.country_iso == "AT"

# TCP reset (analyzing with problems establishmet of communication, scan search)
tcp.flags.reset == 1

---
# Caputre all traffic with tcpdump
tcpdump -i <interface> -s 65535 -w <file>

# Splitting large pcap file
tcpdump -r old_file -w new_files -C 10

# tcpdump handybook
https://nanxiao.github.io/tcpdump-little-book/

# Rotate and capture with tshark
tshark -a filesize:500000 -b files:10 -i < INTERFACE > -w BASE_FILE_NAME.pcapng

# Tshark tuts
https://tshark.dev/capture/


