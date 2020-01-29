BITZENY-SEEDER
==============

bitzeny-seeder is a crawler for the BitZeny network, which exposes a list of reliable nodes via a built-in DNS server.

INSTALLATION
------------

```bash
cd && \
sudo apt-get update && \
sudo apt-get install -y build-essential libboost-all-dev libssl-dev && \
git clone https://github.com/fujiwaraizuho/bitzeny-seeder.git && \
cd bitzeny-seeder && \
make -j$(nproc)
```

REMOVE
------

```
sudo killall dnsseed ; \
cd && sudo rm -f dnsseed.dat && sudo rm -f dnsseed.dump && sudo rm -f dnsstats.log && \
cd && cd bitzeny-seeder/ && sudo rm -f dnsseed.dat && sudo rm -f dnsseed.dump && sudo rm -f dnsstats.log && \
cd && rm -rf bitzeny-seeder/ && \
sudo netstat -nulp | grep 53
```

USAGE
-----

On the system `zny.seed.fujishan.jp`, you can now run dnsseed with root privileged to use port 53 (`UDP`)
```bash
sudo ./dnsseed -h zny.seed.fujishan.jp -n zny.ns1.fujishan.jp -m contact@fujishan.jp
```

Assuming you want to run a dns seed on `zny.seed.fujishan.jp`, you will need an authorative NS record in `fujishan.jp`'s domain record, pointing to for example `zny.seed.fujishan.jp`:

```bash
dig -t NS zny.seed.fujishan.jp
```

```
;; ANSWER SECTION:
zny.seed.fujishan.jp. 86400 IN	NS	zny.ns1.fujishan.jp.
```

If you want the DNS server to report SOA records, please provide an e-mail address (with the `@` part replaced by `.`) using `-m`.

RUNNING AS NON-ROOT
-------------------

Typically, you'll need root privileges to listen to port 53 (name service). One solution is using an iptables rule (Linux only) to redirect it to a non-privileged port:

```bash
iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-port 5353
```

If properly configured, this will allow you to run dnsseed in userspace, using the -p 5353 option.
