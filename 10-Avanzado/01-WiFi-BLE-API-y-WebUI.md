# 10 — Panorama avanzado: Wi‑Fi, BLE, API REST y Web UI

Este capítulo sirve como mapa de progresión. Antes de entrar aquí deberías dominar interfaz, subred, ARP, DNS, filtros BPF, `net.recon`, `net.probe`, `net.sniff` y caplets.

## 1. Wi‑Fi

Bettercap incorpora módulos específicos para redes 802.11. En este campo el sistema operativo y el adaptador importan muchísimo más que en Ethernet.

### Requisitos conceptuales

Debes entender:

- modo managed;
- modo monitor;
- canales;
- BSSID;
- ESSID;
- estaciones;
- tramas 802.11;
- diferencia entre asociación y autenticación;
- WPA2/WPA3 a nivel conceptual.

### Entorno recomendado

Para laboratorios Wi‑Fi utiliza Linux/Kali con un adaptador USB compatible con modo monitor. Windows no es el entorno que elegiría para aprender esta parte.

### Qué estudiar en Bettercap

Busca en `help` los módulos `wifi.*` disponibles en tu versión y estudia primero únicamente reconocimiento y observación de tu propio punto de acceso de laboratorio.

Regla:

```text
primero observar → después comprender → solo después experimentar
```

## 2. BLE

Bettercap también dispone de capacidades Bluetooth Low Energy.

Antes de usar módulos BLE conviene entender:

- advertisements;
- servicios GATT;
- characteristics;
- UUID;
- central/peripheral;
- scanning vs conexión.

Utiliza exclusivamente dispositivos propios de laboratorio.

## 3. HID

Bettercap incluye soporte relacionado con dispositivos HID en hardware compatible.

Este campo depende mucho del hardware y no debería ser tu prioridad inicial. Estúdialo después de dominar Ethernet y Wi‑Fi.

## 4. CAN bus

Bettercap incluye soporte CAN para entornos de automoción/embebidos.

Necesitas conocer antes:

- frames CAN;
- arbitration ID;
- bitrate;
- interfaces SocketCAN;
- DBC;
- OBD-II.

Practica únicamente en simuladores, bancos de pruebas o vehículos/sistemas propios preparados para ello.

## 5. Web UI

La UI oficial es una forma visual de manejar Bettercap.

Arranque habitual:

```bash
sudo bettercap -eval "ui on"
```

Dentro de Bettercap consulta:

```text
help ui
help api.rest
```

La UI es especialmente útil cuando ya entiendes qué hacen los módulos. Si empiezas exclusivamente por botones, puedes ocultarte la arquitectura real.

## 6. API REST

Bettercap dispone de una API REST para consultar el estado de la sesión y automatizar acciones.

Conceptualmente:

```text
SCRIPT PYTHON
      ↓ HTTP
API REST DE BETTERCAP
      ↓
SESIÓN / MÓDULOS
      ↓
EVENTOS / RESULTADOS
```

Esto permite construir:

- paneles;
- automatización de laboratorio;
- recolección de eventos;
- integración con otras herramientas;
- pruebas repetibles.

## 7. Seguridad de la API

No expongas la API REST en interfaces no confiables sin revisar autenticación, dirección de escucha, TLS y credenciales.

Antes de usarla:

```text
help api.rest
```

Revisa especialmente:

- address;
- port;
- username;
- password;
- certificate/key.

## 8. Proxies

Bettercap tiene capacidades de proxy a diferentes niveles.

Antes de estudiarlas debes dominar:

- TCP;
- HTTP;
- HTTPS/TLS;
- certificados;
- SNI;
- forwarding;
- diferencia entre proxy explícito y transparente.

No memorices scripts de proxy sin entender dónde se inserta el proxy en el flujo.

## 9. Ruta avanzada recomendada

```text
Ethernet
   ↓
Reconocimiento
   ↓
PCAP/BPF
   ↓
ARP + DNS en lab
   ↓
Caplets
   ↓
Web UI / API REST
   ↓
Wi‑Fi reconocimiento
   ↓
BLE
   ↓
Proxies
   ↓
CAN/HID según interés
```

## 10. Qué significa realmente “dominar Bettercap”

No significa conocer todos los comandos de memoria.

Significa poder responder:

```text
¿Qué capa estoy tocando?
¿Qué interfaz está usando Bettercap?
¿Qué paquetes genera el módulo?
¿Qué estado modifica?
¿Qué eventos produce?
¿Qué defensa puede impedirlo?
¿Cómo limito el alcance?
¿Cómo revierto la sesión?
¿Cómo puedo automatizarlo?
```

Si puedes responder eso, ya estás pensando como operador y no como alguien que copia comandos.
