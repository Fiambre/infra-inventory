# Ejemplos de consultas complejas
# Demostración de cómo el inventario puede responder preguntas

# Consulta 1: ¿Qué impacto tendría si UREK se cae?
# ==============================
# Pasos:
# 1. Buscar srv-urek en servers/servers.yaml
# 2. Listar todos sus servicios
# 3. Para cada servicio, buscar dependencias en services/services.yaml
# 4. Identificar servicios que dependen de servicios en UREK

# Respuesta esperada:
# Servicios directamente afectados:
# - svc-homeassistant (automatización del hogar)
# - svc-n8n (automatización de workflows)
# - svc-minio (almacenamiento de objetos)
# - svc-portainer (gestión de contenedores)
# - svc-browserless (scraping)
# - svc-organizr (dashboard)
# - svc-mosquitto (MQTT broker)
# - svc-cloudflared (tunnel)
# - svc-jupyterlab (notebooks)
#
# Bases de datos afectadas:
# - db-urek-postgres (PostgreSQL con Home Assistant)
# - db-urek-mongo (MongoDB con N8N y stock data)
# - db-urek-webdb (SQLite)
#
# Servicios externos dependientes:
# - svc-baileys-whatsapp depende de svc-minio


# Consulta 2: ¿Qué servicios dependen de db-urek-mongo?
# ==============================
# Pasos:
# 1. Buscar servicios en services/services.yaml
# 2. Filtrar aquellos que tengan db-urek-mongo en databases array

# Respuesta esperada:
# - svc-n8n (usa MongoDB para su data)


# Consulta 3: ¿Qué servidor tiene más servicios activos?
# ==============================
# Pasos:
# 1. Leer todos los servidores en servers/servers.yaml
# 2. Contar length del array services para cada uno
# 3. Comparar y retornar el máximo

# Respuesta esperada:
# - srv-urek: 9 servicios
# - srv-anak: 2 servicios
# - srv-khun, srv-rak, srv-baam, srv-playa: 0 servicios
#
# Ganador: srv-urek con 9 servicios


# Consulta 4: ¿Qué IPs están asignadas a los servidores activos?
# ==============================
# Pasos:
# 1. Filtrar servidores que no tienen tag "offline"
# 2. Extraer ip y tailscale_ip

# Respuesta esperada:
# UREK:
# - IP principal: 100.70.74.125
# - Tailscale: 100.70.74.125
#
# ANAK:
# - IP principal: 172.19.0.1
# - Tailscale: 100.125.220.30
#
# KHUN:
# - IP principal: 100.105.231.35
# - Tailscale: 100.105.231.35
#
# RAK:
# - IP principal: 100.92.237.92
# - Tailscale: 100.92.237.92


# Consulta 5: ¿Qué servicios son "production" y corren en contenedores?
# ==============================
# Pasos:
# 1. Buscar servicios con ambos tags: "production" y "containerized"
# 2. Filtrar por technology: "docker"

# Respuesta esperada:
# - svc-homeassistant (UREK, home-assistant:stable)
# - svc-n8n (UREK, Docker)
# - svc-minio (UREK, Docker)
# - svc-portainer (UREK, Docker)
# - svc-browserless (UREK, Docker)
# - svc-organizr (UREK, Docker)
# - svc-mosquitto (UREK, Docker)
# - svc-cloudflared (UREK, Docker)
# - svc-jupyterlab (UREK, Docker)
# - svc-openclaw-gateway (ANAK, Docker)
# - svc-baileys-whatsapp (ANAK, Docker)


# Consulta 6: ¿Qué bases de datos usa el servicio N8N?
# ==============================
# Pasos:
# 1. Buscar svc-n8n en services/services.yaml
# 2. Leer el array databases

# Respuesta esperada:
# - db-urek-mongo (MongoDB en UREK, puerto 27017)
#   Contiene databases: n8n, stock_data


# Consulta 7: ¿Qué redes conectan UREK y ANAK?
# ==============================
# Pasos:
# 1. Buscar redes en networks/networks.yaml
# 2. Filtrar redes que contengan srv-urek y srv-anak en servers

# Respuesta esperada:
# - net-tailscale (VPN mesh): contiene ambos UREK y ANAK
# - Las redes Docker (net-docker-urek, net-docker-anak) son separadas
# - net-lan-santiago solo contiene ANAK
