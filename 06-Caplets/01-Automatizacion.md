# 06 — Caplets: automatizar Bettercap

## Qué es un caplet

Un caplet es un archivo `.cap` que contiene comandos de Bettercap ejecutados secuencialmente.

Piensa en él como una receta reproducible:

```text
configurar → activar → observar → detener
```

## Caplet mínimo

Archivo `recon-lab.cap`:

```text
net.recon on
net.probe on
```

Ejecutarlo:

```bash
sudo bettercap -caplet recon-lab.cap
```

## Por qué son importantes

Los caplets permiten:

- repetir sesiones;
- evitar errores al teclear;
- documentar configuraciones;
- compartir laboratorios;
- automatizar tareas;
- crear una metodología consistente.

## Caplets instalados

Dentro de Bettercap:

```text
caplets.show
```

Rutas:

```text
caplets.paths
```

Actualizar colección oficial:

```text
caplets.update
```

## Ejemplo de reconocimiento controlado

```text
# recon-lab.cap

net.recon on
net.probe on
set net.show.sort seen desc
```

Después, en la sesión:

```text
net.show
```

## Ejemplo de captura local

```text
# sniff-local.cap

set net.sniff.local true
set net.sniff.verbose false
set net.sniff.filter "icmp or udp port 53"
set net.sniff.output ./lab-capture.pcap
net.sniff on
```

Detén la captura manualmente cuando termines:

```text
net.sniff off
```

## Variables y orden

Un error clásico es activar un módulo antes de configurarlo.

Mal:

```text
net.sniff on
set net.sniff.filter "icmp"
```

Mejor:

```text
set net.sniff.filter "icmp"
net.sniff on
```

Regla:

```text
SET → ON
```

## Caplet de laboratorio con ticker

Los caplets pueden combinar módulos y comandos periódicos.

Conceptualmente:

```text
net.recon on
net.probe on
set ticker.commands "net.show"
set ticker.period 10
ticker on
```

Así puedes refrescar información periódicamente.

## Diseñar buenos caplets

Un buen caplet debería:

1. explicar su propósito;
2. limitar objetivos;
3. configurar antes de activar;
4. evitar defaults peligrosamente amplios;
5. ser fácil de detener;
6. guardar resultados cuando tenga sentido.

## Plantilla

```text
# Nombre:
# Objetivo:
# Entorno:
# Interfaz esperada:
# Resultado esperado:

# 1. Configuración

# 2. Activación

# 3. Observación

# 4. Limpieza manual documentada
```

## Caplets vs scripts Bash

Caplet:

```text
controla Bettercap desde dentro
```

Bash/Python:

```text
controla Bettercap desde fuera
```

Para automatizaciones más complejas puedes utilizar la API REST.

## Ejercicio

Crea tres caplets:

```text
01-recon.cap
02-sniff-icmp.cap
03-sniff-dns.cap
```

Haz que cada uno tenga un único objetivo claro. Después ejecútalos y comprueba que sabes explicar cada línea sin mirar documentación.
