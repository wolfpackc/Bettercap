# 02 — Arquitectura y módulos

## Bettercap como motor de sesión

Cuando ejecutas Bettercap no lanzas simplemente una utilidad concreta. Entras en un motor interactivo que mantiene estado y permite activar módulos.

```text
Bettercap
│
├── Core
│   ├── variables
│   ├── eventos
│   ├── caplets
│   ├── API REST
│   └── UI
│
├── Ethernet
│   ├── net.recon
│   ├── net.probe
│   ├── net.sniff
│   ├── arp.spoof
│   ├── ndp.spoof
│   └── dns.spoof
│
├── Wi-Fi
├── BLE
├── HID
├── CAN
└── Proxies
```

## Patrón general de cada módulo

Casi todos siguen esta lógica:

```text
1. Leer ayuda
2. Configurar parámetros
3. Activar módulo
4. Observar eventos
5. Desactivar módulo
```

Ejemplo:

```text
help net.recon
net.recon on
net.show
net.recon off
```

## Variables de entorno

Bettercap conoce datos como:

- interfaz activa;
- dirección IPv4;
- MAC;
- gateway;
- red local;
- parámetros de cada módulo.

Esto permite que muchos valores tengan defaults inteligentes.

Pero un profesional no confía ciegamente en los defaults: los comprueba.

## Comandos de ayuda

```text
help
help net.recon
help net.sniff
help arp.spoof
help dns.spoof
```

Puedes usar Bettercap como si tuviera sus propias páginas `man`.

Ejemplo desde shell:

```bash
sudo bettercap -eval "help net.recon; q"
```

## Eventos

Los módulos generan eventos y `events.stream` actúa como una consola central.

Mentalmente:

```text
MÓDULO → EVENTO → events.stream → operador
```

Esto es importante porque Bettercap no trabaja únicamente con salida síncrona. Muchos módulos se quedan funcionando en segundo plano.

## `on` y `off`

Activar un módulo significa iniciar un comportamiento persistente:

```text
net.recon on
```

No termina hasta que:

```text
net.recon off
```

Por eso debes acostumbrarte a pensar siempre en pares:

```text
ON  → inicio
OFF → limpieza
```

## `set` y `get`

Ejemplo:

```text
set net.show.sort seen desc
get net.show.sort
```

La idea importante es que **primero configuras el módulo y después lo enciendes**.

## Ejecución desde línea de comandos

Puedes ejecutar una secuencia sin permanecer en la consola:

```bash
sudo bettercap -eval "net.recon on; net.probe on; sleep 5; net.show; q"
```

Para estudiar, sin embargo, la consola interactiva es mejor porque ves el estado y entiendes qué ocurre.

## Web UI y API

Bettercap no se limita a una CLI. También puede exponer una interfaz web y API REST.

Eso permite construir automatizaciones o integrar Bettercap con herramientas externas.

## Analogía con un sistema operativo

Bettercap puede entenderse como un pequeño sistema operativo especializado:

- la sesión = kernel lógico;
- módulos = servicios;
- variables = configuración;
- eventos = logs;
- caplets = scripts de arranque;
- UI/API = interfaces de administración.

Con esta analogía es más fácil recordar que una acción puede seguir activa aunque tú hayas escrito otro comando después.
