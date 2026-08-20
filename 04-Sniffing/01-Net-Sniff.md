# 04 — Sniffing con `net.sniff`

## Qué hace

`net.sniff` captura tráfico y puede interpretar determinados protocolos. También puede guardar la captura en un archivo PCAP para analizarla posteriormente con Wireshark.

La idea correcta es separar:

```text
CAPTURAR ≠ INTERPRETAR ≠ MODIFICAR
```

`net.sniff` se centra en captura e interpretación. El tráfico que puedes ver depende completamente de tu posición en la red.

## Activar y detener

```text
net.sniff on
net.sniff off
```

Consultar estadísticas:

```text
net.sniff stats
```

## Capturar tráfico local

Por defecto Bettercap puede omitir tráfico cuyo origen/destino sea tu propia máquina.

Para un laboratorio donde quieres estudiar tráfico local:

```text
set net.sniff.local true
net.sniff on
```

## Verbosidad

```text
set net.sniff.verbose true
```

Con `verbose` activado se envía más información de paquetes a `events.stream`.

Para estudiar suele ser mejor comenzar con:

```text
set net.sniff.verbose false
```

y activar más detalle solo cuando lo necesites.

## Filtros BPF

Bettercap acepta filtros BPF.

Ejemplos inocuos:

### Solo ICMP

```text
set net.sniff.filter "icmp"
```

### Solo DNS

```text
set net.sniff.filter "udp port 53"
```

### Tráfico de una máquina concreta

```text
set net.sniff.filter "host 192.168.56.102"
```

### HTTP de laboratorio

```text
set net.sniff.filter "tcp port 80"
```

## Guardar PCAP

```text
set net.sniff.output ./captura-lab.pcap
net.sniff on
```

Después:

```text
net.sniff off
```

Abre el archivo con Wireshark.

## Bettercap + Wireshark

Esta combinación es muy potente:

```text
Bettercap = genera contexto y captura
Wireshark = disección visual profunda
```

Ejercicio:

1. Activa captura local.
2. Filtra ICMP.
3. Haz `ping` entre dos máquinas de tu laboratorio.
4. Guarda la captura.
5. Ábrela en Wireshark.
6. Localiza Echo Request y Echo Reply.

## Por qué no siempre ves tráfico ajeno

En un switch moderno, tu NIC no recibe automáticamente todo el tráfico de otros hosts.

Puedes ver:

- broadcast;
- multicast;
- tráfico dirigido a tu MAC;
- tráfico que pase realmente por tu equipo;
- tráfico de una interfaz en modo adecuado.

Esta es una de las ideas más importantes para comprender sniffing.

## Analogía

Estar conectado al mismo switch no significa escuchar todas las conversaciones.

Es como vivir en el mismo bloque de pisos: compartir edificio no significa que todas las cartas de tus vecinos pasen por tu buzón.

## Ejercicio recomendado

Haz tres capturas distintas:

```text
A) ICMP
B) DNS
C) TCP puerto 80
```

Después analiza los tres PCAP con Wireshark. El objetivo es aprender a relacionar lo que Bettercap muestra con los paquetes reales.
