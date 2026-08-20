# 00 — Fundamentos: qué es Bettercap y cómo pensar en él

## 1. Definición útil

Bettercap es una herramienta modular para observar, analizar y manipular tráfico de red en entornos autorizados. Su mayor fortaleza no es un módulo concreto, sino que permite encadenar descubrimiento, captura, spoofing, proxying y automatización dentro de una misma sesión.

## 2. Bettercap no es Wireshark

Wireshark está centrado en **analizar paquetes**. Bettercap puede analizar tráfico, pero además puede:

- descubrir hosts;
- provocar tráfico de descubrimiento;
- cambiar su posición lógica dentro de una red de laboratorio;
- responder a ciertos protocolos;
- ejecutar proxies;
- automatizar tareas;
- exponer una API y una Web UI.

### Analogía

Imagina una carretera:

- Wireshark = una cámara que observa vehículos.
- Nmap = alguien que recorre casas y comprueba qué puertas están abiertas.
- Bettercap = un centro de control capaz de observar, descubrir, redirigir ciertos flujos y automatizar acciones.

## 3. Modelo modular

Una sesión está formada por módulos y variables.

Ejemplo:

```text
help net.recon
net.recon on
net.show
net.recon off
```

Los parámetros se consultan o modifican normalmente con `get` y `set`:

```text
get net.show.sort
set net.show.sort seen desc
```

## 4. Estado y eventos

Bettercap mantiene información sobre:

- interfaz;
- gateway;
- endpoints;
- módulos activos;
- variables de sesión;
- eventos generados.

Por eso es importante pensar en Bettercap como una **sesión viva**, no como un comando que se ejecuta y termina.

## 5. Flujo mental de trabajo

```text
1. ¿Qué interfaz estoy usando?
2. ¿Qué red estoy viendo?
3. ¿Qué hosts existen?
4. ¿Qué módulo necesito?
5. ¿Qué parámetros debo limitar?
6. ¿Qué resultado quiero observar?
7. ¿Cómo detengo y limpio la práctica?
```

## 6. Comandos conceptuales esenciales

```text
help
help <modulo>
set <parametro> <valor>
get <parametro>
<modulo> on
<modulo> off
q
```

Los comandos pueden concatenarse con `;`:

```text
net.recon on; net.probe on; net.show
```

## 7. Caplets

Un caplet es un archivo `.cap` que contiene comandos de Bettercap. Conceptualmente es parecido a un `.rc` de Metasploit.

Ejemplo mínimo:

```text
net.recon on
net.probe on
```

Esto permite convertir una secuencia manual en una receta repetible.

## 8. Qué conviene dominar primero

Antes de tocar MITM conviene dominar:

- interfaces y subredes;
- ARP;
- DNS;
- gateway;
- switching;
- routing;
- DHCP;
- filtros BPF;
- diferencia entre tráfico local, tráfico dirigido al gateway y broadcast.

Sin esto, Bettercap se convierte en memorizar comandos sin entender qué está ocurriendo.

## 9. Regla de seguridad de laboratorio

Para practicar módulos que alteran tráfico utiliza una red aislada con máquinas virtuales o dispositivos propios. Define siempre objetivos concretos en vez de aplicar módulos a toda la subred.
