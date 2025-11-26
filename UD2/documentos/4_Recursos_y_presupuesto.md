# Recursos y Presupuestos

---

## **1. Recursos Necesarios**

El proyecto requiere una combinación de recursos **hardware**, **software** y **humanos**, orientados a implementar un sistema de monitorización e IDS adaptado a una pequeña empresa como Cocacusca

---

## **1.1. Recursos Hardware**

Para Cocacusca S.L., que ya dispone de servidores básicos pero sin herramientas de monitorización, se propone:

### **Servidor físico o virtual para el IDS**

**Requisitos recomendados:**
- CPU: 4 vCores  
- RAM: 8–16 GB  
- Almacenamiento: 100–200 GB SSD  
- Sistema operativo: Debian o Ubuntu Server  

**Servicios alojados en este servidor:**
- Suricata (IDS)  
- Contenedor Nmap  
- Base de datos / sistema de logs  
- Grafana  

En caso de no contar con hardware propio, puede utilizarse un **VPS económico**.

### **Equipos de prueba**
- 1 PC para pruebas de escaneo  
- 1 Equipo para simular ataques internos (pruebas controladas)

---

## **1.2. Recursos Software**

Todos los recursos software utilizados son **open source**, por lo que no generan gasto de licencia:

- Suricata  
- Nmap  
- Grafana  
- Docker / Podman  
- Elasticsearch, Loki o base de datos equivalente  

---

# **2. Presupuesto Estimado**

Estimación adaptada a Cocacusca S.L., una PYME sin infraestructura compleja.

---

## **2.1. Hardware**

### **Opción A — Servidor físico propio**

| Recurso | Coste estimado |
|--------|----------------|
| Servidor básico (nuevo) | **350–600 €** |
| Ampliación de RAM o SSD (si ya existe servidor) | **80–150 €** |

---

### **Opción B — VPS en la nube**

| Recurso | Coste |
|--------|--------|
| VPS 4 vCPU / 8 GB RAM / 100 GB SSD | **12–20 €/mes** |
| Dominio o DNS (opcional) | **10–15 €/año** |

---

## **2.2. Software**

Al ser **software libre**, los costes de licencia son **0 €**.

---

## **2.3. Recursos Humanos**

Los costes pueden variar si el proyecto lo realiza un alumno en prácticas o personal interno.  
En caso de externalizarlo, esta sería la estimación:

| Actividad | Horas estimadas | Coste estimado (20–30 €/h) |
|-----------|-----------------|-----------------------------|
| Instalación y configuración del servidor | 8–12 h | **160–360 €** |
| Instalación de contenedores (Nmap, Suricata, Grafana) | 6–8 h | **120–240 €** |
| Configuración de reglas y automatización | 10–14 h | **200–420 €** |
| Creación de dashboards y alertas | 8–10 h | **160–300 €** |
| Pruebas, simulaciones y documentación | 10–16 h | **200–480 €** |

### **Total estimado recursos humanos:**  
### **840 € – 1.800 €**

---


