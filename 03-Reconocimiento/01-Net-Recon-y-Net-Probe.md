# 03 — Reconocimiento: `net.recon` y `net.probe`

## Objetivo

Antes de analizar o manipular tráfico necesitas saber **qué existe en la red**.

Bettercap separa dos ideas:

- `net.recon`: observa y mantiene una lista de endpoints descubiertos.
- `net.probe`: envía paquetes de descubrimiento para provocar respuestas.

## Analogía

Imagina un edificio:

- `net.recon` = recepcionista que anota quién entra y sale.
- `net.probe` = alguien que llama a cada puerta para comprobar si hay alguien.

## `net.recon`

Activa descubrimiento pasivo/semipasivo de hosts:

```text
net.recon on
```

Mostrar hosts:

```text
net.show
```

Detener:

```text
net.recon off
```

Limpiar caché:

```text
net.clear
```

Ver metadata:

```text
net.show.meta 192.168.56.102
```

La metadata puede incluir información obtenida mediante mDNS, UPnP, puertos y otros mecanismos.

## `net.probe`

Genera sondas dentro de la subred actual:

```text
net.probe on
```

Detener:

```text
net.probe off
```

Bettercap puede utilizar mecanismos como:

- mDNS;
- NBNS;
- UPnP;
- WSD.

## Flujo típico de laboratorio

```text
net.recon on
net.probe on
sleep 5
net.show
```

Después:

```text
net.probe off
net.recon off
```

## Ordenar resultados

```text
set net.show.sort seen desc
net.show
```

Mostrar únicamente los 10 más recientes:

```text
set net.show.limit 10
net.show
```

Filtrar por texto:

```text
set net.show.filter Windows
net.show
```

## Qué debes interpretar

No memorices únicamente IP y MAC. Pregúntate:

```text
¿Es el gateway?
¿Es mi máquina?
¿Es una VM?
¿Es un móvil?
¿Qué fabricante sugiere la MAC?
¿Qué hostname tiene?
¿Qué servicios anuncia?
¿Cuándo fue visto por última vez?
```

## Reconocimiento Bettercap vs Nmap

Bettercap y Nmap se complementan.

### Bettercap

Muy bueno para:

- descubrir endpoints durante una sesión;
- observar dispositivos que aparecen/desaparecen;
- mantener contexto de red;
- alimentar otros módulos.

### Nmap

Más especializado en:

- escaneo exhaustivo de puertos;
- detección de servicios;
- scripts NSE;
- fingerprinting profundo.

Flujo profesional típico:

```text
Bettercap → localizar hosts
Nmap      → estudiar servicios
Wireshark → estudiar paquetes
```

## Ejercicio práctico

En una red Host-Only aislada con 3 VMs:

1. Inicia Bettercap.
2. Activa `net.recon`.
3. Ejecuta `net.show`.
4. Enciende otra VM.
5. Observa si aparece.
6. Activa `net.probe`.
7. Compara los hosts antes y después.
8. Apaga `net.probe` y `net.recon`.

El objetivo no es atacar nada: es entender cómo Bettercap construye su visión de la red.
