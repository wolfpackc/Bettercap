# 08 — Chuleta de comandos esenciales

## Arranque

```bash
sudo bettercap
sudo bettercap -h
sudo bettercap -eval "help net.recon; q"
sudo bettercap -eval "ui on"
```

## Ayuda

```text
help
help net.recon
help net.probe
help net.sniff
help arp.spoof
help dns.spoof
help caplets
help api.rest
help ui
```

## Variables

```text
get <parametro>
set <parametro> <valor>
```

Ejemplo:

```text
get net.show.sort
set net.show.sort seen desc
```

## Reconocimiento

```text
net.recon on
net.recon off
net.probe on
net.probe off
net.show
net.clear
net.show.meta 192.168.56.102
```

## Orden y filtros de hosts

```text
set net.show.sort ip asc
set net.show.sort seen desc
set net.show.limit 10
set net.show.filter Windows
net.show
```

## Sniffing

```text
net.sniff on
net.sniff off
net.sniff stats
```

Captura local:

```text
set net.sniff.local true
```

Verbose:

```text
set net.sniff.verbose true
```

PCAP:

```text
set net.sniff.output ./captura.pcap
```

Filtros BPF:

```text
set net.sniff.filter "icmp"
set net.sniff.filter "udp port 53"
set net.sniff.filter "tcp port 80"
set net.sniff.filter "host 192.168.56.102"
```

## ARP spoofing — solo laboratorio autorizado

```text
set arp.spoof.targets 192.168.56.102
arp.spoof on
arp.spoof off
```

Full duplex:

```text
set arp.spoof.fullduplex true
```

## DNS spoofing — dominio ficticio de laboratorio

```text
set dns.spoof.domains lab.test
set dns.spoof.address 192.168.56.200
dns.spoof on
dns.spoof off
```

## Caplets

```text
caplets.show
caplets.paths
caplets.update
```

Ejecutar:

```bash
sudo bettercap -caplet fichero.cap
```

## Web UI

```bash
sudo bettercap -eval "ui on"
```

## Concatenar comandos

```text
net.recon on; net.probe on; net.show
```

## Comandos externos útiles en Linux

```bash
ip addr
ip route
ip neigh
ip link
tcpdump -D
```

## Comandos externos útiles en Windows

```powershell
ipconfig
arp -a
Get-NetAdapter
route print
```

## Secuencia mental de trabajo

```text
1. interfaz
2. gateway
3. subred
4. net.recon
5. net.probe
6. net.show
7. elegir objetivo
8. configurar módulo
9. activar módulo
10. observar
11. desactivar
12. comprobar limpieza
```

## Cierre limpio de laboratorio

```text
dns.spoof off
arp.spoof off
net.sniff off
net.probe off
net.recon off
q
```
