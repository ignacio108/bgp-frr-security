# bgp-frr-security
Virtual Lab using VNX( Virtual Networks over linuX) to test the security of BGP, performing Prefix Hijack and AS_PATH Forgery

![Topology](img/Topology_BGP_lab.png)


![Topology](img/BGP_Topology_detail.png)

## Description 

- The scenary if composed of various AS. The AS 100 is confederation of the AS with private numeration. 


- ECMP is implemented in the AS 64512 and AS 64516 towards AS 200


### IGP of the Autonomous Systems
Hosts:
| AS        | IGP          | Network         |   Description                                            |
|-----------|--------------|-----------------|----------------------------------------------------------|
| 64513     | OSPFv2       | 10.1.3.0/29     | Area 1, also contains the connection with hI             |
| 64514     | Static       | 10.1.2.1/30     | rF directly attached to rE                               |
| 64515     | EIGRP        | 10.1.1.0/28     | In addition, the IGP Contains the connection with hA     |
| ALL       | OSPFv2       | 192.168.1.0/24  | Area 0 between all AS of the confederation               |


## Attacks 

The malicious router is rA wich form a neigbourd with rB via BGP, in order to perform these Attacks:

However, these attacks might be also performed by AS200

TODO Routinator for ROAs and RPKI

Routinator como RPKI RTR client

A Route Origin Authorization (ROA) is a cryptographically signed object that states which Autonomous System (AS) is authorized to originate a particular IP address prefix or set of prefixes.

Resource Public Key Infrastructure (RPKI) data. It validates the Route Origin Attestations contained in the data and makes them available to your BGP routing workflow.

### Prefix Hijacking

In order to start the Prefix Hijackin execute:

```bash
sudo vnx -f frr-bgp.xml -x loadra_prefix_hijack
```

If you want to go back to the original rA configuration execute:

```bash
sudo vnx -f frr-bgp.xml -x loadra
```

### AS_PATH Forgery

## Modificar /etc/sysctl.conf para evitar problemas al lancar el escenario

```bash
fs.inotify.max_queued_events = 2097152
fs.inotify.max_user_instances = 2097152
fs.inotify.max_user_watches = 2097152
```
Luego ejecuta 
```bash
sysctl --system
```
## IP Addresses:
Hosts:
| Host      | Interface    | IPv4            |   Network                              |
|-----------|--------------|-----------------|----------------------------------------|
| rA        | netA         | 10.10.1.2/30    | 10.10.1.0/30                           |
| rA        | netA-B       | 10.1.1.1/30     | 10.1.1.0/30                            |
| rA        | netA-C       | 10.1.1.14/30    | 10.1.1.12/30                           |
| hA        | netA         | 10.10.1.1/30    | 10.10.1.0/30                           |
| rB        | netA-B       | 10.1.1.2/30     | 10.1.1.0/30                            |
| rB        | netB-D       | 10.1.1.5/30     | 10.1.1.4/30                            |
| rB        | netB-E       | 192.168.1.1/30  | 192.168.1.0/30                         |
| rB        | netB-G       | 192.168.1.5/30  | 192.168.1.4/30                         |
| rC        | netA-C       | 10.1.1.13/30    | 10.1.1.12/30                           |
| rC        | netC-D       | 10.1.1.10/30    | 10.1.1.8/30                            |
| rD        | netB-D       | 10.1.1.6/30     | 10.1.1.4/30                            |
| rD        | netC-D       | 10.1.1.9/30     | 10.1.1.8/30                            |
| rE        | netB-E       | 192.168.1.2/30  | 192.168.1.0/30                         |
| rE        | netE-F       | 10.1.2.2/30     | 10.1.2.0/30                            |
| rE        | netE-J       | 192.168.1.9/30  | 192.168.1.8/30                         |
| rF        | netE-F       | 10.1.2.1/30     | 10.1.2.0/30                            |
| rG        | netB-G       | 192.168.1.6/30  | 192.168.1.4/30                         |
| rG        | netG-I-H     | 10.1.3.1/29     | 10.1.3.0/29                            |
| rG        | netG-K       | 192.168.1.13/30 | 192.168.1.12/30                        |
| rH        | netG-I-H     | 10.1.3.2/29     | 10.1.3.0/29                            |
| rI        | netI         | 10.10.1.6/30    | 10.10.1.4/30                           |
| rI        | netG-I-H     | 10.1.3.3/29     | 10.1.3.0/29                            |
| hI        | netI         | 10.10.1.5/30    | 10.10.1.4/30                           |
| rJ        | netE-J       | 192.168.1.10/30 | 192.168.1.8/30                         |
| rJ        | netJ-K       | 192.168.1.17/30 | 192.168.1.16/30                        |
| rJ        | netJ-M       | 172.16.1.1/30   | 172.16.1.0/30                          |
| rK        | netG-K       | 192.168.1.14/30 | 192.168.1.12/30                        |
| rK        | netJ-K       | 192.168.1.18/30 | 192.168.1.16/30                        |
| rK        | netK-M       | 172.16.1.5/30   | 172.16.1.4/30                          |
| rM        | netJ-M       | 172.16.1.2/30   | 172.16.1.0/30                          |
| rM        | netK-M       | 172.16.1.6/30   | 172.16.1.4/30                          |


## Documentation:

https://reposit.haw-hamburg.de/bitstream/20.500.12738/17557/1/MA_Implementation%20and%20Evaluation%20of%20BGPsec%20for%20the%20FRRouting%20Suite.pdf
https://www.rfc-editor.org/rfc/rfc8205
https://docs.frrouting.org/en/latest/bgp.html#
https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-189r1.ipd.pdf


Routinator Installation guide:
https://routinator.docs.nlnetlabs.nl/en/stable/installation.html