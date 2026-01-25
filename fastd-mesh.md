# Mesh VPN with `fastd` and BATMAN

```bash
apt-get install fastd batctl
modprobe batman_adv
echo batman_adv >> /etc/modules-load.d/batman_adv.conf
mkdir -p /etc/fastd/mesh/peers
fastd --generate-key
```

`/etc/fastd/mesh/fastd.conf`:
```
interface "fastd0";
mode tap;
method "salsa2012+umac";
bind 0.0.0.0:10000; # core node
#bind any; # terminal node
secret "...";
mtu 1426;
on up "
  ip link set dev $INTERFACE up
";

include peers from "peers";
```

`/etc/systemd/system/batman-mesh.service`:
```ini
[Unit]
Description=Batman mesh on bat0
After=fastd@mesh.service
BindsTo=fastd@mesh.service
PartOf=fastd@mesh.service


[Service]
Type=oneshot
RemainAfterExit=yes

ExecStart=/bin/sh -c '\
  modprobe batman_adv; \
  ip link add bat0 type batadv 2>/dev/null || true; \
  batctl if add fastd0 2>/dev/null || true; \
  ip link set bat0 mtu 1394 up; \
  ip addr add 10.250.X.Y/16 dev bat0 2>/dev/null || true \
'

ExecStop=/bin/sh -c '\
  batctl if del fastd0 || true; \
  ip link del bat0 || true \
'

[Install]
WantedBy=fastd@mesh.service
```

```bash
systemctl daemon-reload
systemctl enable --now fastd@mesh
```
