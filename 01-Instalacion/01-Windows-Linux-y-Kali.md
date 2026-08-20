# 01 — Instalación: Windows, Linux y Kali

## ¿Funciona en Windows?

Sí. Bettercap soporta oficialmente Microsoft Windows y publica binarios precompilados para Windows amd64. También soporta Linux, macOS, Android y Docker.

## ¿Dónde merece más la pena usarlo?

### Windows

Útil para:

- aprender la consola;
- reconocimiento Ethernet;
- pruebas con interfaces compatibles;
- Web UI;
- estudiar módulos y parámetros.

Limitaciones prácticas habituales:

- ciertas funciones de bajo nivel dependen más del stack y drivers disponibles;
- el acceso a hardware Wi‑Fi en modo monitor/inyección suele ser más incómodo;
- muchos tutoriales y laboratorios de seguridad asumen Linux.

### Linux

Es el entorno más natural para Bettercap porque facilita:

- `libpcap`;
- forwarding;
- acceso a interfaces;
- monitor mode;
- herramientas auxiliares como `ip`, `iw`, `tcpdump`, `wireshark`, `nmap`;
- `libnetfilter-queue` para módulos que dependen de NFQUEUE.

### Kali Linux

Para aprendizaje ofensivo/defensivo en laboratorio suele ser la opción más cómoda.

## Instalación con binario oficial

La forma más sencilla es usar un binario estable desde los releases oficiales.

Después verifica:

```bash
bettercap -version
bettercap -h
```

En Linux normalmente necesitarás privilegios elevados para captura e interacción de bajo nivel:

```bash
sudo bettercap -h
```

## Instalación mediante Go

Si tienes Go correctamente instalado:

```bash
go install github.com/bettercap/bettercap/v2@latest
```

Dependencias habituales del sistema:

```text
pkg-config
libpcap
libusb-1.0-0
libnetfilter-queue   # Linux, para packet.proxy
```

En distribuciones Debian/Kali, para compilar desde código pueden ser necesarios paquetes de desarrollo como:

```bash
sudo apt update
sudo apt install build-essential libpcap-dev libusb-1.0-0-dev libnetfilter-queue-dev
```

## Docker

Existe imagen oficial:

```bash
docker pull bettercap/bettercap
```

Ejemplo típico Linux:

```bash
docker run -it --privileged --net=host bettercap/bettercap -h
```

Pero recuerda: Docker no es ideal para módulos que necesitan acceso directo al hardware. Un caso típico es Wi‑Fi.

## Primer arranque

### Linux

```bash
ip addr
ip route
sudo bettercap
```

### Windows

Antes de lanzar Bettercap, identifica tus interfaces:

```powershell
Get-NetAdapter
ipconfig
```

Después ejecuta Bettercap desde una terminal con los permisos adecuados.

## Elegir interfaz

No asumas nunca que Bettercap ha seleccionado la interfaz correcta.

Antes de empezar una práctica comprueba:

- IP propia;
- máscara/subred;
- gateway;
- nombre de interfaz;
- si es Ethernet, Wi‑Fi, VPN, Host‑Only, NAT, etc.

La pregunta fundamental es:

> ¿Estoy mirando la red que creo que estoy mirando?

## WSL

Aunque Bettercap puede compilarse o ejecutarse en entornos Linux, WSL introduce una capa de virtualización de red. Para aprender comandos puede ser útil, pero para prácticas donde quieres controlar exactamente la interfaz física, ARP o Wi‑Fi es preferible una VM con bridging/USB passthrough o Linux nativo.

## Web UI

Bettercap ofrece interfaz web oficial. En Linux:

```bash
sudo bettercap -eval "ui on"
```

Después puedes consultar `help ui` y `help api.rest` para ver configuración de la UI y API.

## Comprobación mínima tras instalar

Dentro de Bettercap:

```text
help
help net.recon
net.recon on
net.show
net.recon off
```

Si esto funciona, ya tienes una base válida para continuar.
