# 09 — Troubleshooting: problemas comunes

## 1. Bettercap no ve hosts

Comprueba primero:

```bash
ip addr
ip route
```

En Windows:

```powershell
ipconfig
Get-NetAdapter
```

Preguntas:

- ¿estás en la interfaz correcta?
- ¿la VM está en NAT cuando querías Host-Only/Bridged?
- ¿los hosts están realmente en la misma subred?
- ¿hay una VPN interfiriendo?
- ¿el firewall bloquea tráfico de descubrimiento?

Después prueba:

```text
net.recon on
net.probe on
net.show
```

## 2. `net.show` solo muestra mi equipo o gateway

Puede significar que no hay suficiente tráfico o que estás en una topología que aísla clientes.

Prueba a generar tráfico entre máquinas propias y activa `net.probe`.

## 3. No veo tráfico ajeno con `net.sniff`

Esto suele ser normal.

En una red con switch, tu equipo no recibe automáticamente tráfico unicast entre otros dos hosts.

Comprueba si estás capturando:

- tráfico local;
- broadcast/multicast;
- tráfico dirigido a tu equipo;
- tráfico que realmente atraviesa tu equipo.

Para tráfico generado por la propia máquina:

```text
set net.sniff.local true
```

## 4. El filtro parece no funcionar

Revisa la sintaxis BPF.

Empieza por filtros simples:

```text
icmp
udp port 53
tcp port 80
host 192.168.56.102
```

Después ve aumentando complejidad.

## 5. No se genera el PCAP

Comprueba:

```text
get net.sniff.output
net.sniff stats
```

Usa una ruta donde tengas permiso de escritura.

Ejemplo:

```text
set net.sniff.output ./lab.pcap
```

## 6. El spoofing rompe la conexión del laboratorio

Comprueba:

- objetivo correcto;
- forwarding;
- gateway;
- si el router o switch tiene protecciones ARP;
- si seleccionaste la interfaz correcta;
- si estás mezclando NAT/Host-Only/Bridged.

No intentes “arreglarlo” ampliando el objetivo a toda la subred. Reduce el laboratorio y observa tablas ARP.

## 7. La caché ARP no cambia

Posibles causas:

- protecciones contra ARP spoofing;
- target incorrecto;
- interfaz equivocada;
- víctima en otra VLAN/subred;
- tráfico virtualizado que no sigue la topología que creías.

Comprueba:

### Linux

```bash
ip neigh
```

### Windows

```powershell
arp -a
```

## 8. DNS spoofing no funciona

Recuerda que Bettercap debe poder recibir/observar las consultas DNS del host de laboratorio.

Comprueba además:

- dominio configurado;
- IP configurada;
- caché DNS del cliente;
- DoH/DoT;
- navegador con Secure DNS;
- si la consulta realmente usa UDP/TCP 53 tradicional.

Una práctica moderna puede fallar precisamente porque el cliente usa DNS cifrado. Eso es una lección, no necesariamente un error de Bettercap.

## 9. HTTPS no se deja leer/modificar como HTTP

Es lo esperado. TLS está diseñado para proteger confidencialidad e integridad frente a intermediarios.

Que estés en posición MITM a nivel de red no significa que hayas roto TLS.

## 10. Wi‑Fi no entra en monitor mode

Comprueba que el adaptador y su driver soporten:

- monitor mode;
- cambio de canal;
- inyección, si tu laboratorio la requiere.

No todos los adaptadores USB sirven.

En Linux:

```bash
iw dev
ip link
```

## 11. Bettercap en VM no ve el adaptador USB

Necesitas pasar el USB a la VM y verificar que el host no lo está reteniendo.

Después comprueba dentro de Linux:

```bash
lsusb
ip link
iw dev
```

## 12. Problemas en Windows

Comprueba:

- que usas el binario correcto para Windows amd64;
- permisos de terminal;
- dependencias/captura de paquetes disponibles;
- interfaz seleccionada;
- antivirus/EDR bloqueando captura o ejecución;
- adaptadores virtuales/VPN que alteren la selección de interfaz.

Para prácticas avanzadas de red, si Windows se convierte en una lucha contra drivers, usa Kali/Linux y céntrate en aprender Bettercap, no en pelear con el sistema operativo.

## 13. `caplets.update` falla

Comprueba conectividad a Internet, permisos y rutas de caplets.

Dentro de Bettercap:

```text
caplets.paths
caplets.show
```

## 14. Método universal de depuración

Cuando algo no funcione, reduce el problema:

```text
1. ¿Tengo interfaz?
2. ¿Tengo IP?
3. ¿Tengo gateway?
4. ¿Veo al target?
5. ¿Veo ARP?
6. ¿El módulo está activo?
7. ¿El parámetro está bien configurado?
8. ¿La red/VM permite ese tráfico?
9. ¿La defensa moderna del protocolo evita lo que esperaba?
```

No empieces cambiando diez parámetros a la vez. Cambia uno, observa y documenta.
