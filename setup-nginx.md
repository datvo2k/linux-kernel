# Build nginx server for test

```
bash nginx/install-nginx.sh
```
After build nginx, edit `nginx.conf` file as sample

### wth 1000 users

```
wrk -t 4 -c 1000 -d 60s --latency --timeout 10s http://<ip-server>/health


echo "GET http://<ip-server>/1MB.bin" | \
vegeta attack \
  -duration=60s \
  -rate=10000/s \
  -workers=50 \
  -connections=1000 \
  -keepalive=true \
  timeout=10s \
  -max-workers=100 | \
vegeta report -type=text
```

```
# watch network
watch -n 1 'ss -s && echo "---" && netstat -s | grep -E "overflow|drop" | head -5'

# watch cpu
mpstat -P ALL 1

# watch network drops
watch -n1 'echo "=== Packet Drops ==="; ethtool -S eth0 | grep drop; echo "=== SYN Overflows ==="; netstat -s | grep overflowed'
```
