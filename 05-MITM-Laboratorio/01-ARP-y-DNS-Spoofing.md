# 05 — MITM en laboratorio: ARP y DNS spoofing

> Este capítulo está pensado exclusivamente para una red aislada con máquinas propias o autorizadas.

## 1. Qué significa MITM

En un ataque Man-in-the-Middle, un tercer equipo consigue situarse lógicamente entre dos extremos de una comunicación.

Ejemplo conceptual:

```text
SIN MITM

Victima ───────────── Gateway

CON MITM DE LABORATORIO

Victima ─── Bettercap ─── Gateway
```

La clave no es “capturar paquetes mágicamente”. La clave es conseguir que el tráfico **pase por tu equipo**.

## 2. ARP: recordatorio

ARP relaciona direcciones IPv4 con direcciones MAC dentro de la red local.

Ejemplo normal:

```text
192.168.56.1  →  08:00:27:AA:AA:AA
```

Los hosts mantienen estas asociaciones en una caché ARP.

## 3. Qué hace `arp.spoof`

Bettercap puede enviar respuestas ARP construidas para alterar temporalmente las asociaciones ARP de equipos seleccionados.

En un laboratorio, el objetivo pedagógico es observar:

- cómo cambia la caché ARP;
- cómo el tráfico puede pasar a través de una máquina intermedia;
- cómo funcionan las defensas contra ARP spoofing;
- qué ocurre al detener el módulo.

## 4. Nunca uses toda la subred por defecto al practicar

Define un objetivo concreto.

Ejemplo de laboratorio:

```text
Victima:  192.168.56.102
Gateway:  192.168.56.1
Atacante: 192.168.56.103
```

Dentro de Bettercap:

```text
set arp.spoof.targets 192.168.56.102
arp.spoof on
```

Detener:

```text
arp.spoof off
```

## 5. `fullduplex`

Bettercap permite controlar si intenta afectar a ambos lados de la comunicación.

```text
set arp.spoof.fullduplex true
```

Conceptualmente:

```text
half duplex:
se altera principalmente la percepción de un extremo

full duplex:
se intenta alterar la percepción de víctima y gateway
```

No actives opciones por memorizar comandos: observa primero las tablas ARP y entiende qué flujo quieres estudiar.

## 6. Forwarding

Para que una práctica MITM no rompa necesariamente la conexión, el tráfico debe poder continuar hacia su destino.

Bettercap dispone del parámetro:

```text
arp.spoof.forwarding
```

La idea mental es:

```text
RECIBO → OBSERVO → REENVÍO
```

Si no se reenvía:

```text
RECIBO → BLOQUEO
```

## 7. Comprobar la caché ARP

### Windows

```powershell
arp -a
```

### Linux

```bash
ip neigh
```

Haz una captura antes y después de activar el laboratorio para entender qué entrada cambia.

## 8. DNS spoofing

DNS spoofing consiste en responder a una consulta DNS con una dirección diferente a la legítima.

Ejemplo completamente controlado:

```text
lab.test → 192.168.56.200
```

En Bettercap:

```text
set dns.spoof.domains lab.test
set dns.spoof.address 192.168.56.200
dns.spoof on
```

Detener:

```text
dns.spoof off
```

Para recibir consultas DNS de otros hosts, Bettercap normalmente debe estar ya en una posición desde la que pueda observarlas, por ejemplo mediante un escenario ARP de laboratorio.

## 9. Laboratorio DNS seguro

Crea una VM servidor con una página que diga:

```text
SERVIDOR DNS LAB
```

Después:

1. usa un dominio ficticio como `lab.test`;
2. redirígelo únicamente dentro de tu red aislada;
3. observa la consulta DNS con Wireshark;
4. compara la respuesta esperada y la manipulada;
5. detén `dns.spoof` y `arp.spoof`.

## 10. Qué debes aprender realmente

No te quedes con:

```text
arp.spoof on
```

Debes poder explicar:

```text
¿Por qué ARP permite esto?
¿Qué entrada cambia?
¿Por qué un switch no lo evita por sí solo?
¿Por qué HTTPS sigue protegiendo contenido?
¿Qué hace el forwarding?
¿Qué ocurre al detener el spoofing?
¿Qué defensas existen?
```

## 11. Defensas que debes conocer

- Dynamic ARP Inspection.
- DHCP Snooping.
- segmentación de red.
- aislamiento de clientes Wi‑Fi.
- tablas ARP estáticas en escenarios concretos.
- detección de cambios MAC/IP anómalos.
- HTTPS/TLS.
- DNSSEC, según el escenario.

Entender la defensa es parte de entender el ataque.
