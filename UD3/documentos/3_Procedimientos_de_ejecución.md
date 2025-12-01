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

## Configuración primera maquina 
sudo mv /etc/netplan/01-network-manager-all.yaml.backup /etc/netplan/01-network-manager-all.yaml
