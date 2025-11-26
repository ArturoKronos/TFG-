## 2. Diseño lógico y físico de la infraestructura

---

## **Módulo 1: Escaneo y análisis de red con Nmap**

### Funcionalidades principales
- Escaneo de puertos abiertos.  
- Detección de servicios y versiones.  
- Identificación de sistemas operativos.  
- Detección de vulnerabilidades básicas mediante scripts **NSE**.  
- Generación de logs relacionados con actividad sospechosa.  

### Flujo de trabajo
**Nmap** → envía resultados → **Servidor de procesamiento** (Elastic/Logstash o scripts propios) → **Base de datos / Grafana**

---

## **Módulo 2: Detección de intrusiones con Suricata**

### Funcionalidades principales
- Inspección profunda de paquetes (**IDS/IPS**).  
- Detección de patrones de ataque (DoS, escaneos, exploits conocidos).  
- Reglas personalizadas para detectar escaneos Nmap o tráfico malicioso.  
- Exportación de logs en formato **EVE JSON**.  

### Flujo de trabajo
**Suricata (modo IDS)** → genera logs EVE → **Servidor de logs** (o contenedor) → **Grafana**

---

## **Módulo 3: Visualización y alertas con Grafana**

### Dashboards
- Eventos sospechosos detectados por Suricata.  
- Escaneos realizados por Nmap.  
- Mapas de puertos abiertos en la red.  
- Series temporales de intentos de intrusión.  
- Integración con **Loki**, **Elasticsearch** o **Prometheus**.

### Sistema de alertas automáticas
- Envío por correo electrónico.  
- Envío por Telegram.  
- Notificaciones instantáneas ante reglas críticas.  

---

## **Base de datos / Sistema de logs**

### Información almacenada
- Resultados de escaneos **Nmap**.  
- Logs **EVE** de Suricata.  
- Métricas para consulta desde Grafana.  

El acceso está restringido mediante red interna.

---

## **Flujo de Información y Conectividad**

### Direccionamiento IP

| Servicio / Contenedor       | Dirección IP asignada            |
|-----------------------------|----------------------------------|
| Servidor principal          | 192.168.10.10 (estática)         |
| Contenedor Suricata         | 172.20.0.2 (Docker)              |
| Contenedor Nmap             | 172.20.0.3                       |
| Base de datos               | 172.20.0.4                       |
| Grafana                     | 172.20.0.5                       |
| Red objetivo (clientes)     | 192.168.20.0/24 (DHCP)           |

---

## **Comunicación entre servicios**

- **Nmap → Backend:** puerto 5000/TCP (JSON).  
- **Suricata → Servidor de logs:** puerto 9500/UDP (EVE JSON).  
- **Grafana → Base de datos:**  
  - 9200/HTTP (Elasticsearch)  
  - 3100/TCP (Loki)  
- **Grafana → Panel web:** puerto 3000/TCP.  

---

## **VLANs (opcional)**

| VLAN | Descripción |
|------|-------------|
| **VLAN 10** | Administración (Suricata, Grafana, Logs). |
| **VLAN 20** | Red objetivo de escaneo. |
| **VLAN 30** | Visualización y panel de control. |

---


