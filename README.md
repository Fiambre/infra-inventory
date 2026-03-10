# Infra Inventory

Fuente de verdad para inventario de infraestructura en formato YAML legible por humanos y consultable por agentes de IA.

## Propósito

Este repositorio centraliza toda la información sobre servidores, servicios, bases de datos y redes en archivos YAML estructurados. Permite:

- Mantener un inventario actualizado de infraestructura
- Responder consultas complejas sobre dependencias
- Facilitar la automatización con agentes de IA
- Documentar relaciones entre componentes

## Estructura

```
infra-inventory/
├─ README.md                 # Este archivo
├─ schemas/                  # Esquemas de validación (opcional)
│ ├─ server.schema.yaml
│ ├─ service.schema.yaml
│ ├─ database.schema.yaml
│ └─ network.schema.yaml
├─ servers/                  # Definiciones de servidores
│ └─ servers.yaml
├─ services/                 # Definiciones de servicios
│ └─ services.yaml
├─ databases/                # Definiciones de bases de datos
│ └─ databases.yaml
└─ networks/                 # Definiciones de redes
  └─ networks.yaml
```

## Convenciones de nombres

### IDs
- Prefijo para tipo de entidad: `srv-` (server), `svc-` (service), `db-` (database), `net-` (network)
- Nombre descriptivo con kebab-case: `srv-trading-01`, `svc-api-rest`, `db-prod-main`
- Único en todo el inventario

### Hostnames
- Nombres DNS o nombres de host reales
- Uso de dominios completos cuando corresponda: `srv-trading.fiambre.dev`

### Tags
- Palabras clave simples sin prefijo
- Minúsculas: `production`, `staging`, `monitoring`

### Relaciones
- Siempre usar el ID de la entidad relacionada
- Referencias cruzadas por nombre o ID

## Ejemplos de consultas

### ¿Qué servidor ejecuta el servicio `svc-homeassistant`?
```yaml
# Consultar en services.yaml el servidor asociado
services:
  id: svc-homeassistant
  server: srv-urek  # Respuesta: srv-urek
```

### ¿Qué base de datos usa `svc-n8n`?
```yaml
# Consultar en services.yaml las bases de datos
services:
  id: svc-n8n
  databases:
    - db-urek-mongo  # Respuesta: db-urek-mongo
```

### ¿Qué servicios corren en `srv-urek`?
```yaml
# Consultar en servers.yaml
servers:
  id: srv-urek
  services:
    - svc-homeassistant
    - svc-n8n
    - svc-minio
    - svc-portainer
    # ... más servicios
```

### ¿Qué IP tiene `srv-anak`?
```yaml
# Consultar en servers.yaml
servers:
  id: srv-anak
  ip: 100.125.220.30  # IP de Tailscale
```

## Uso con agentes de IA

Los agentes pueden responder consultas complejas combinando información de múltiples archivos:

- "¿Qué impacto tendría si `srv-urek` se cae?" → Consultar qué servicios corren allí y sus dependencias
- "¿Qué servicios dependen de `db-urek-postgres`?" → Consultar qué servicios lo listan en `databases`
- "¿Qué servidor tiene más servicios activos?" → Comparar listas de servicios por servidor

## Contribución

1. Mantener el formato YAML válido
2. Usar IDs consistentes y únicos
3. Actualizar las relaciones cuando se muevan servicios
4. Documentar cambios importantes en el README

## Notas

- Todos los YAML deben ser válidos
- No usar caracteres especiales en IDs (solo `[a-z0-9-]`)
- Mantener orden alfabético donde tenga sentido
- Los comentarios ayudan a entender contextos específicos
