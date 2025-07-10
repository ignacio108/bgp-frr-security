# bgp-frr-security
Virtual Lab using VNX( Virtual Networks over linuX) to test the security of BGP, performing Prefix Hijack and AS_PATH Forgery

![Topology](img/Topology_BGP_lab.png)


![Topology](img/BGP_Topology2.png)
Temas a implementar (borrador)

ECMP entre los dos AS frontera 

configuración completa de Routinator (ROAs y prefijos).

A Route Origin Authorization (ROA) is a cryptographically signed object that states which Autonomous System (AS) is authorized to originate a particular IP address prefix or set of prefixes.

Routinator como RPKI RTR client

Resource Public Key Infrastructure (RPKI) data. It validates the Route Origin Attestations contained in the data and makes them available to your BGP routing workflow.

Configuración de iBGP full mesh para AS 64515 y AS 64513

Confguración ruta estática AS 64514

Realizar los ataques de Prejix Hijack y AS_PATH Forgery desde el router malicioso

Proponer ataques desde el AS 200

Documentación extra:

https://reposit.haw-hamburg.de/bitstream/20.500.12738/17557/1/MA_Implementation%20and%20Evaluation%20of%20BGPsec%20for%20the%20FRRouting%20Suite.pdf
https://www.rfc-editor.org/rfc/rfc8205
https://docs.frrouting.org/en/latest/bgp.html#
https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-189r1.ipd.pdf


Instalación routinator:
https://routinator.docs.nlnetlabs.nl/en/stable/installation.html


## Modificar /etc/sysctl.conf para evitar problemas al lancar el escenario

fs.inotify.max_queued_events = 2097152
fs.inotify.max_user_instances = 2097152
fs.inotify.max_user_watches = 2097152

Luego ejecuta 

sysctl --system

## IP Addresses:
Hosts:
| Host      | Interface    | IPv4            |   Network                              |
|-----------|--------------|-----------------|----------------------------------------|
| rA        | netA         | 10.10.1.2/30    | 10.10.1.0/30                           |
| rA        | netA-B       | 10.1.1.1/30     | 10.1.1.0/30                            |
| rA        | netA-C       | 10.1.1.14/30    | 10.1.1.12/30                           |
| hA        | netA         | 10.10.1.1/30    | 10.10.1.0/30                           |
| rB        | netA-B       | 10.1.1.2/30     | 10.1.1.0/30                           |
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
| rG        | netG-H       | 10.1.3.1/30     | 10.1.3.0/30                            |
| rG        | netG-I       | 10.1.3.5/30     | 10.1.3.4/30                            |
| rG        | netG-K       | 192.168.1.13/30 | 192.168.1.12/30                        |
| rH        | netG-H       | 10.1.3.2/30     | 10.1.3.0/30                            |
| rI        | netI         | 10.10.1.6/30    | 10.10.1.4/30                           |
| rI        | netG-I       | 10.1.3.6/30     | 10.1.3.4/30                            |
| hI        | netI         | 10.10.1.5/30    | 10.10.1.4/30                           |
| rJ        | netE-J       | 192.168.1.10/30 | 192.168.1.8/30                         |
| rJ        | netJ-K       | 192.168.1.17/30 | 192.168.1.16/30                        |
| rJ        | netJ-M       | 172.16.1.1/30   | 172.16.1.0/30                          |
| rK        | netG-K       | 192.168.1.14/30 | 192.168.1.12/30                        |
| rK        | netJ-K       | 192.168.1.18/30 | 192.168.1.16/30                        |
| rK        | netK-M       | 172.16.1.5/30   | 172.16.1.4/30                          |
| rM        | netJ-M       | 172.16.1.2/30   | 172.16.1.0/30                          |
| rM        | netK-M       | 172.16.1.6/30   | 172.16.1.4/30                          |