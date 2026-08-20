# Bettercap — Guía completa de estudio y laboratorio

> Guía práctica y teórica para aprender Bettercap desde cero hasta un uso avanzado en laboratorios y redes propias/autorizadas.

## ¿Qué es Bettercap?

Bettercap es una plataforma de reconocimiento, monitorización y pruebas de seguridad de redes. La versión moderna 2.x está escrita en Go y organiza sus capacidades en módulos: descubrimiento de hosts, sniffing, spoofing ARP/NDP/DNS, Wi‑Fi, BLE, proxies, API REST, Web UI y automatización mediante caplets.

Una buena analogía es pensar en Bettercap como una **consola de control de red**:

- `net.recon` = tus ojos: observa quién aparece en la red.
- `net.probe` = llamar a las puertas: genera tráfico de descubrimiento para encontrar equipos.
- `net.sniff` = el analizador de tráfico.
- `arp.spoof` = modifica temporalmente el “mapa de carreteras” ARP de un laboratorio para colocar tu equipo en medio del tráfico.
- `dns.spoof` = laboratorio para estudiar cómo una respuesta DNS manipulada puede cambiar el destino de un nombre.
- `caplets` = scripts que automatizan sesiones completas.
- Web UI / REST API = panel de control y automatización externa.

## Windows o Linux

Bettercap **funciona oficialmente en Windows, Linux, macOS, Android y Docker**. Existen binarios precompilados para Windows amd64 y Linux amd64.

Para estudiar Ethernet y reconocimiento básico, Windows es perfectamente válido. Para prácticas de seguridad de red más profundas, interfaces inalámbricas, forwarding, packet proxying y acceso directo al hardware, **Linux suele ser el entorno preferible**. Kali Linux es especialmente cómodo porque muchas dependencias y herramientas complementarias ya están integradas.

Docker es útil para experimentar, pero algunos módulos que necesitan acceso directo al hardware no funcionan bien dentro de contenedores.

## Estructura del repositorio

```text
Bettercap/
├── README.md
├── 00-Fundamentos/
│   └── 01-Que-es-Bettercap.md
├── 01-Instalacion/
│   └── 01-Windows-Linux-y-Kali.md
├── 02-Arquitectura-y-Modulos/
│   └── 01-Como-pensar-en-Bettercap.md
├── 03-Reconocimiento/
│   └── 01-Net-Recon-y-Net-Probe.md
├── 04-Sniffing/
│   └── 01-Net-Sniff.md
├── 05-MITM-Laboratorio/
│   └── 01-ARP-y-DNS-Spoofing.md
├── 06-Caplets/
│   └── 01-Automatizacion.md
├── 07-Laboratorios/
│   └── 01-Lab-Completo.md
├── 08-Chuletas/
│   └── 01-Comandos-Esenciales.md
└── 09-Troubleshooting/
    └── 01-Problemas-Comunes.md
```

## Orden recomendado

1. Fundamentos.
2. Instalación y elección de interfaz.
3. Arquitectura y sintaxis interactiva.
4. `net.recon` + `net.probe`.
5. `net.sniff` y filtros BPF.
6. MITM en laboratorio aislado.
7. Caplets.
8. Laboratorio completo.
9. Chuleta y troubleshooting.

## Regla mental fundamental

Bettercap no es un único “ataque”. Es un **motor modular**. Normalmente una sesión se construye así:

```text
INTERFAZ
   ↓
RECONOCIMIENTO
   ↓
OBJETIVOS
   ↓
MÓDULO
   ↓
PARÁMETROS
   ↓
ACTIVAR
   ↓
EVENTOS / PCAP / RESULTADOS
```

La sintaxis típica sigue este patrón:

```text
set modulo.parametro valor
modulo on
modulo off
help modulo
```

Ejemplo inocuo de reconocimiento de tu propio laboratorio:

```text
net.recon on
net.probe on
net.show
```

## Documentación oficial

La referencia principal debe ser siempre la documentación oficial de Bettercap y el repositorio oficial `bettercap/bettercap`, porque los parámetros y módulos pueden cambiar entre versiones.

---

**Uso ético:** realiza prácticas de interceptación, spoofing o manipulación únicamente en redes y equipos propios o para los que tengas autorización explícita.
