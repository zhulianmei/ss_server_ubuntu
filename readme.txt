sudo apt install libev4 libc-ares2 libmbedtls-dev  -y
chmod +x bin/ss-*
cp bin/ss-* /usr/bin && /usr/bin/ss-manager -c /home/ubuntu/ss_server_ubuntu/config_mult_port.json >/home/ubuntu/ss_server_ubuntu/start_ss.log 2>&1 &

echo '
[Install]
WantedBy=multi-user.target
Alias=rc-local.service
' >> /lib/systemd/system/rc-local.service

touch /etc/rc.local
echo '#!/bin/bash
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT
iptables -F
/usr/bin/ss-manager -c /etc/config/config_mult_port.json >/home/ubuntu/start_ss.log 2>&1 &
exit 0
' >>/etc/rc.local
chmod +x /etc/rc.local
systemctl enable rc-local.service
systemctl restart rc-local.service

