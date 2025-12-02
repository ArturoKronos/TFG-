## Preparación básica del sistema
Máquina de Seguridad (Ubuntu Server):

Aquí irá Suricata, Elasticsearch, Grafana
Esta es tu máquina principal de detección y visualización
Necesitará al menos 4GB RAM, 2 CPU, 30GB disco


Máquina Víctima (Ubuntu Server o Desktop):

Simula un servidor de la empresa
Aquí pondrás servicios como SSH, Apache
Esta será la que "ataquen"
Necesita 2GB RAM, 1 CPU, 20GB disco


Máquina Atacante (Kali Linux o Ubuntu):

Desde aquí simularás los ataques
Usarás Nmap, hydra, etc.
Necesita 2GB RAM, 1 CPU, 20GB disco

## Procedimientos 
Primero de tod debemos crear una red nat donde estaran todas las maquinas que usaremos para este procedimiento, nosotro la llamaremos RedTFG. Una vez creada dicha Red, la pondremos en la configuracion de cada una de las maquinas. 

## Configuración primera maquina server
Escribe:
```bash
sudo nano /etc/netplan/50-cloud-init.yaml
```
Borra todo y escribe esto exactamente (cuidado con los espacios):

```bash
network:
  ethernets:
    enp0s3:
      dhcp4: no
      addresses:
        - 10.0.2.10/24
      routes:
        - to: default
          via: 10.0.2.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4
  version: 2
```
Ahora aplica la configuración:
```bash
sudo netplan apply
```
Verifica la IP:
```bash
ip a
```
*Si esto te da fallo haz lo siguiente:*

Primero, vamos a ver qué hay en ese archivo:
```bash
cat /etc/netplan/01-network-manager-all.yaml
```
Mientras tanto, vamos a arreglar los permisos de ese archivo para quitar el warning:
```bash
sudo chmod 600 /etc/netplan/01-network-manager-all.yaml
```
Ahora necesitamos deshabilitar ese archivo y usar solo el nuestro. Escribe:
```bash
sudo mv /etc/netplan/01-network-manager-all.yaml /etc/netplan/01-network-manager-all.yaml.backup
```
Ahora aplica de nuevo la configuración:
```bash
sudo netplan apply
```
Esto dara un pequeño error necesario, que corregiremos en un segundo:
```bash
sudo mv /etc/netplan/01-network-manager-all.yaml.backup /etc/netplan/01-network-manager-all.yaml
```
Ahora vamos a editar SOLO ese archivo y dejarlo bien configurado:
```bash
sudo nano /etc/netplan/01-network-manager-all.yaml
```
Borra todo lo que haya y pon esto:
```bash
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: no
      addresses:
        - 10.0.2.10/24
      routes:
        - to: default
          via: 10.0.2.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4
```
Ahora borra el otro archivo que creamos antes:
```bash
sudo rm /etc/netplan/50-cloud-init.yaml
```
Aplica la configuración:
```bash
sudo netplan apply
```
Verifica:
```bash
ip a
```
Ahora que tienes internet funcionando, vamos a actualizar Ubuntu antes de instalar nada:
```bash
sudo apt update
```
Luego:
```bash
sudo apt upgrade -y
```
Vamos a instalar Suricata, que es nuestro IDS (Sistema de Detección de Intrusos):
```bash
sudo apt install suricata -y
```
Ahora vamos a configurar Suricata para que escuche en nuestra interfaz de red:
```bash
sudo nano /etc/suricata/suricata.yaml
```
Se abrirá un archivo ENORME. No te asustes, solo vamos a cambiar 2 cosas:
- Presiona Ctrl + W (buscar)
- Escribe: af-packet
- Presiona Enter
Te llevará a una sección que dice algo como:
```bash
af-packet:
  - interface: eth0
```
Cambia eth0 por enp0s3 (tu interfaz).
Suricata necesita reglas para saber qué ataques detectar. Vamos a descargar las reglas gratuitas:
```bash
sudo suricata-update
```
Cuando termine, reinicia Suricata:
```bash
sudo systemctl restart suricata
```
Verifica que está corriendo:
```bash
sudo systemctl status suricata
```
Siguiente paso instalar Elasticsearch
Primero instalamos Java (Elasticsearch lo necesita):
```bash
sudo apt install openjdk-11-jdk -y
```
Verifica que Java se instaló:
```bash
java -version
```
Ahora vamos a instalar Elasticsearch. Primero añadimos el repositorio:
```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
```
```bash
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
```
Actualiza los repositorios:
```bash
sudo apt update
```
Instala Elasticsearch:
```bash
sudo apt install elasticsearch -y
```
<p style="color:red;"><strong>IMPORTANTE:</strong></p>

Durante la instalación te mostrará una contraseña para el usuario <code>elastic</code>.  
<strong>COPIA Y GUARDA ESA CONTRASEÑA</strong> en algún sitio (notepad, papel, etc.).  
La necesitaremos después.

Configurar Elasticsearch
Edita el archivo de configuración:
```bash
sudo nano /etc/elasticsearch/elasticsearch.yml
```
Busca estas líneas (están comentadas con # al principio). Presiona Ctrl + W para buscar cada una:
1. Busca: network.host

Descomenta la línea (quita el #) y ponla así:
```bash
network.host: localhost
```
2. Busca: http.port
Descomenta y déjala así:
```bash
http.port: 9200
```
3. Busca: xpack.security.enabled

Si la encuentras, déjala en true. Si no existe, no pasa nada.

Guardar y salir:

Ctrl + O → Enter
Ctrl + X

Arrancar Elasticsearch
Habilita e inicia el servicio:
```bash
sudo systemctl enable elasticsearch
```
```bash
sudo systemctl start elasticsearch
```
Luego verifica:
```bash
sudo systemctl status elasticsearch
```
Si el servicio no arrancó. Vamos a ver qué pasó:
```bash
sudo journalctl -u elasticsearch -n 50 --no-pager
```
Mientras tanto, vamos a intentar arrancarlo de nuevo:
```bash
sudo systemctl start elasticsearch
```
```bash
sudo systemctl status elasticsearch
```
Ahora vamos a probar que Elasticsearch responde correctamente:
```bash
curl -k -u elastic:TU_CONTRASEÑA https://localhost:9200
```
(Reemplaza TU_CONTRASEÑA por la contraseña que guardaste antes)
Deberías ver algo como:
```bash
{
  "name" : "...",
  "cluster_name" : "elasticsearch",
  "version" : { ... },
  ...
}
```
Instalar Filebeat
Filebeat se encargará de enviar los logs de Suricata a Elasticsearch
Instálalo:
```bash
sudo apt install filebeat -y
```
Configurar Filebeat
Ahora vamos a configurar Filebeat para que lea los logs de Suricata:
Habilita el módulo de Suricata en Filebeat:
```bash
sudo filebeat modules enable suricata
```
Edita la configuración del módulo:
```bash
sudo nano /etc/filebeat/modules.d/suricata.yml
```
Cambia enabled: false por enabled: true y asegúrate que la ruta del log sea correcta:
```bash
- module: suricata
  eve:
    enabled: true
    var.paths: ["/var/log/suricata/eve.json"]
```
Configurar conexión a Elasticsearch
Edita el archivo principal de Filebeat:
```bash
sudo nano /etc/filebeat/filebeat.yml
```
Este archivo es largo. Vamos a buscar y modificar dos secciones:
1. Busca: output.elasticsearch (presiona Ctrl + W)
Verás algo como:
```bash
output.elasticsearch:
  hosts: ["localhost:9200"]
```
Modifícalo para que quede así:
```bash
output.elasticsearch:
  hosts: ["https://localhost:9200"]
  username: "elastic"
  password: "TU_CONTRASEÑA_AQUI"
  ssl.verification_mode: none
```
2. Busca: setup.kibana (presiona Ctrl + W)
Comenta esa sección completa (no la necesitamos ahora). Pon # delante de las líneas o simplemente déjala como está.
Guardar:

Ctrl + O → Enter
Ctrl + X

Cargar configuración y arrancar Filebeat
```bash
sudo filebeat setup -e
```
Cuando termine, inicia Filebeat:
```bash
sudo systemctl enable filebeat
```
```bash
sudo systemctl start filebeat
```
Verifica que está corriendo:
```bash
sudo systemctl status filebeat
```
 Instalar Grafana
Añade el repositorio de Grafana:
```bash
sudo apt-get install -y apt-transport-https software-properties-common wget
```
```bash
sudo mkdir -p /etc/apt/keyrings/
```
```bash
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
```
```bash
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee /etc/apt/sources.list.d/grafana.list
```
Actualiza e instala:
```bash
sudo apt update
```
```bash
sudo apt install grafana -y
```
Inicia Grafana:
```bash
sudo systemctl enable grafana-server
```
```bash
sudo systemctl start grafana-server
```
Verifica que está corriendo:
```bash
sudo systemctl status grafana-server
```

**¿Ves "active (running)" en verde?**

Si sí, ahora necesitamos acceder a Grafana desde tu navegador. Abre tu navegador web (en tu ordenador real, no en la VM) y ve a:
```
http://10.0.2.10:3000
```
# Despliegue y Configuración de Grafana

## Acceso a Grafana

Una vez desplegado Grafana, accede a la página de inicio de sesión:

- **Usuario:** admin  
- **Contraseña:** admin  

Al iniciar sesión, Grafana solicitará cambiar la contraseña. Introduce una nueva (por ejemplo, `tfg2025`) y guárdala.

---

## Conectar Elasticsearch con Grafana

Ya dentro de Grafana:

1. En el menú lateral izquierdo, selecciona **Connections → Data sources**.
2. Haz clic en **Add data source**.
3. Busca y selecciona **Elasticsearch**.
4. Configura la fuente de datos de la siguiente manera:

### Configuración de Elasticsearch

- **Name:** Suricata  
- **URL:** https://localhost:9200  
- **Auth:**  
  - Activar **Basic auth**  
  - Activar **Skip TLS Verify**  
- **Basic Auth Details:**  
  - **User:** elastic  
  - **Password:** (contraseña de Elasticsearch que guardaste)

### Índices

- **Index name:** `filebeat-*`  
- **Time field name:** `@timestamp`  
- **Version:** Selecciona la versión **8.x** o la más reciente.

Finalmente, desplázate al final de la página y haz clic en **Save & test**.

---

## Crear un Dashboard en Grafana

Ahora crearemos un dashboard básico para visualizar las alertas detectadas por Suricata.

1. En el menú lateral izquierdo, selecciona **Dashboards** o el icono **+**.
2. Haz clic en **Create Dashboard → Add visualization**.
3. Selecciona la fuente de datos **Suricata**.
4. En el editor de visualización, en la sección **Query**:

   - Asegúrate de estar en modo **Table** o **Time series**.  
   - En los campos de configuración:
     - **Metric:** Count  
     - **Group by:** Date Histogram en `@timestamp`

5. En el panel derecho, dentro de **Panel options**:

   - **Title:** Alertas de Suricata

6. Haz clic en **Apply** o **Save** en la esquina superior derecha.

El gráfico aparecerá vacío por ahora, ya que todavía no se ha generado tráfico ni alertas.

---

## Guardar el Dashboard

1. Haz clic en **Save dashboard** en la esquina superior derecha.
2. **Name:** Dashboard TFG  
3. Haz clic en **Save**.

## Configuración segunda maquina victima
Configurar IP Fija en la Víctima
Primero vamos a ponerle la IP 10.0.2.20 como planeamos.
Verifica el nombre de la interfaz:
```bash
ip a
```
Ahora edita la configuración de red:
```bash
sudo nano /etc/netplan/01-network-manager-all.yaml

```
Borra todo y pon esto:
```bash
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: no
      addresses:
        - 10.0.2.20/24
      routes:
        - to: default
          via: 10.0.2.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4
```
Guardar:

Ctrl + O → Enter
Ctrl + X

Aplica:
```bash
sudo netplan apply
```
Verifica la IP:
```bash
ip a
```
El servicio de red da un pequeño fallo. Vamos a arreglarlo:
```bash
sudo systemctl enable systemd-networkd
```
```bash
sudo systemctl start systemd-networkd
```
Ahora vuelve a aplicar:
```bash
sudo netplan apply
```
Verifica la IP:
```bash
ip a
```
Una vez hecho esto debemos actualizar el sistema para poder instalar un par de paquetes necesario:
```bash
sudo apt update
```
```bash
sudo apt upgrade -y
```
Cuando termine el upgrade, vamos a instalar servicios que puedan ser atacados:
```bash
sudo apt install apache2 -y
```
```bash
sudo apt install ssh -y
```
 SSH está funcionando (running es lo importante). pero sale "disabled" eso solo significa que no arranca automáticamente al inicio, pero no es problema. Vamos a habilitarlo:
 ```bash
sudo systemctl enable ssh
```
