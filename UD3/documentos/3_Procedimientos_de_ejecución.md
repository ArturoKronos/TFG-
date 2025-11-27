## PASO 0 — Preparación básica del sistema
```bash
sudo apt update && sudo apt upgrade -y

```
```bash
sudo apt install -y curl wget git apt-transport-https software-properties-common

```
Dependencias comunes para Suricata, Promtail y compilación ligera
## PASO 1 — Instalar Suricata y preparar logs
```bash
sudo apt install -y suricata

```
```bash
suricata --version
```
Deberías ver la versión instalada.
### 2. Activar Suricata como servicio
```bash
sudo systemctl enable --now suricata
sudo systemctl status suricata --no-pager
```
Esto confirmará que Suricata está activo.
### 3. Configurar salida de alertas en JSON
Suricata necesita guardar las alertas en un fichero que luego Promtail enviará a Loki.
Edita el fichero principal de configuración:
```bash
sudo nano /etc/suricata/suricata.yaml

```
Busca la sección *outputs:* y reemplázala o añade lo siguiente:
```bash
outputs:
  - eve-log:
      enabled: yes
      filename: /var/log/suricata/eve.json
      types:
        - alert:
            payload: yes
            payload-printable: yes
            packet: yes
        - http
        - dns
        - tls
        - flow
        - drop

```
### 4. Reiniciar Suricata para aplicar cambios
```bash
sudo systemctl restart suricata

```
Verifica que el fichero *eve.json* existe:
```bash
ls -l /var/log/suricata/eve.json
sudo tail -n 5 /var/log/suricata/eve.json

```
## PASO 2 — Instalar Docker y Docker Compose

### 1. Instalar Docker 
Ejecuta en la terminal:
```bash
sudo apt install -y ca-certificates curl gnupg lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" \
  | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

```
Verifica que Docker está instalado:
```bash
docker --version
docker compose version

```
Nota: Para usar Docker sin sudo ejecuta:
```bash
sudo usermod -aG docker $USER

```
### 2. Crear carpeta de trabajo
Vamos a organizar todo en un solo directorio:
```bash
mkdir -p ~/tfg-ids && cd ~/tfg-ids

```
### 3. Crear fichero docker-compose.yml

Copia y pega el siguiente contenido en ~/tfg-ids/docker-compose.yml:
```bash
version: '3.8'
services:
  loki:
    image: grafana/loki:2.8.2
    container_name: loki
    volumes:
      - ./loki-config.yaml:/etc/loki/local-config.yaml:ro
    command: -config.file=/etc/loki/local-config.yaml
    ports:
      - "3100:3100"

  promtail:
    image: grafana/promtail:2.8.2
    container_name: promtail
    volumes:
      - /var/log:/var/log:ro
      - ./promtail-config.yaml:/etc/promtail/config.yml:ro
    command: -config.file=/etc/promtail/config.yml

  grafana:
    image: grafana/grafana:10.1.4
    container_name: grafana
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin
      - GF_INSTALL_PLUGINS=grafana-simple-json-datasource
    ports:
      - "3000:3000"
    volumes:
      - grafana-data:/var/lib/grafana

volumes:
  grafana-data:

```
### 4. Crear fichero loki-config.yaml
Copia y pega en ~/tfg-ids/loki-config.yaml:
```bash
auth_enabled: false

server:
  http_listen_port: 3100

ingester:
  lifecycler:
    address: 127.0.0.1
    ring:
      kvstore:
        store: inmemory
  chunk_idle_period: 5m
  max_chunk_age: 1h

schema_config:
  configs:
    - from: 2020-10-24
      store: boltdb-shipper
      object_store: filesystem
      schema: v11
      index:
        prefix: index_
        period: 24h

storage_config:
  boltdb_shipper:
    active_index_directory: /loki/index
    cache_location: /loki/cache
    shared_store: filesystem
  filesystem:
    directory: /loki/chunks

limits_config:
  enforce_metric_name: false
  max_entries_limit_per_query: 5000


```
### 5. Crear fichero promtail-config.yaml
Copia y pega en ~/tfg-ids/promtail-config.yaml:
```bash
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://loki:3100/loki/api/v1/push

scrape_configs:
  - job_name: suricata
    static_configs:
      - targets: ['localhost']
        labels:
          job: suricata
          __path__: /var/log/suricata/eve.json

    pipeline_stages:
      - json:
          expressions:
            ts: timestamp
            evt: event_type
            alert: alert.signature
            src_ip: src_ip
            dest_ip: dest_ip
            src_port: src_port
            dest_port: dest_port
      - labels:
          ts:
          evt:
          alert:
          src_ip:
          dest_ip:
          src_port:
          dest_port:

```
### 6. Levantar los contenedores
En la carpeta ~/tfg-ids ejecuta:
```bash
docker compose up -d
docker compose ps

```
Si al hacer esto da fallo, posiblemente el fallo sea por el *SUDO* enotnces necesitaremos ejecutar lo siguiente: 
- Cerramos la maquina y reiniciamos.
- Volveremos a abrirla y haremos lo siguiente:
```bash
sudo docker ps 
```
```bash
sudo usermod -aG docker $USER
```
```bash
docker ps 
```
Despues comprobaremos los permisos actuales:
```bash
ls -l /var/run/docker.sock

```
