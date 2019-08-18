lxc launch images:alpine/3.10 redis

lxc info redis --show-log

lxc exec redis -- /bin/ash

echo "https://nl.alpinelinux.org/alpine/latest-stable/community" | tee -a /etc/apk/repository

apk update && apk upgrade 

apk --no-cache --update add redis 

sed 's/^bind /# bind /' -i /etc/redis.conf 

sed -i 's#daemonize yes#daemonize no#i' /etc/redis.conf 

/etc/init.d/redis start

rc-update add redis

netstat -ln | grep 6379
netstat -lnp | grep redis

## On Host
iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 6379 -j DNAT --to ${CONTAINER_IP}:6379

sudo bash -c 'iptables-save > /etc/iptables/rules.v4' # iptables-restore  /etc/iptables/rules.v4
