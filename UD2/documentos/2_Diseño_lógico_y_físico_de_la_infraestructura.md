## *Diseño lógico y físico de la infraestructura*
---
### Arquitectura del Sistema IDS - Cocacusca Antigüedades

---

### Arquitectura lógica del sistema

                INTERNET
                   │
                   ▼
          ┌────────────────┐
          │ Router Movistar│
          │  (Gateway)     │
          └────────┬───────┘
                   │
          ┌────────┴───────┐
          │  Switch TP-Link│
          │   (8 puertos)  │
          └──┬─────┬───┬───┘
             │     │   │
     ┌───────┘     │   └──────────┐
     │             │              │
     ▼             ▼              ▼
┌─────────┐  ┌──────────┐  ┌──────────┐
│Servidor │  │ IDS/SIEM │  │ PCs      │
│   Web   │  │ Sistema  │  │ Oficina  │
│ Apache  │  │ Suricata │  │          │
│WordPress│  │Elastics. │  │          │
│ MySQL   │  │ Grafana  │  │          │
└─────────┘  └──────────┘  └──────────┘
192.168.1.100 192.168.1.50 192.168.1.x

---

## Componentes del sistema IDS

### 1. Suricata (Motor de detección)
- **Función:** Inspección profunda de paquetes (DPI)  
- **Modo:** IDS (detección, no bloqueo)  
- **Interfaz:** Modo promiscuo en `enp0s3`  
- **Reglas:** Emerging Threats Open (actualización diaria)  
- **Logs:** `/var/log/suricata/eve.json` (formato JSON)  

### 2. Filebeat (Recolector de logs)
- **Función:** Envío de logs Suricata a Elasticsearch  
- **Modo:** Agente ligero  
- **Frecuencia:** Tiempo real (streaming)  
- **Formato salida:** JSON enriquecido  

### 3. Elasticsearch (Motor de almacenamiento)
- **Función:** Base de datos distribuida para eventos  
- **Índices:** `filebeat-*` (rotación diaria)  
- **Retención:** 90 días  
- **Búsquedas:** Lucene query DSL  
- **API:** RESTful sobre HTTPS  

### 4. Grafana (Visualización)
- **Función:** Dashboards interactivos  
- **Paneles:** 4 principales (timeline, tipos, IPs, total)  
- **Actualización:** 30 segundos  
- **Acceso:** Web HTTPS puerto 3000  
- **Alertas:** SMTP email + webhook opcionales  

### 5. Nmap (Auditoría)
- **Función:** Escaneo de vulnerabilidades programado  
- **Frecuencia:** Semanal automatizado  
- **Alcance:** Red interna completa  
- **Reports:** Almacenados en `/var/log/auditorias/`  

---

## Flujo de datos

Tráfico Red → Suricata (análisis) → eve.json
↓
Filebeat (recolección)
↓
Elasticsearch (almacenamiento)
↓
Grafana (visualización)
↓
Usuario / Alertas Email


---

## Diseño físico - Diagrama de red

                     ISP
                      │
                [Módem/Router]
                 192.168.1.1
                      │
                [Switch GbE]
                      │
    ┌─────────────────┼─────────────────┐
    │                 │                 │
[Servidor Web] [IDS Server] [Workstations]
192.168.1.100 192.168.1.50 192.168.1.10-20
│ │
│ [Puerto Span/
│ Modo Promiscuo]
│ │
└─────────[Tráfico Copiado]────┘

---

## Configuración de red

| Dispositivo          | IP             | Máscara    | Gateway      | DNS     |
|---------------------|----------------|-----------|-------------|---------|
| Servidor Web         | 192.168.1.100  | /24       | 192.168.1.1 | 8.8.8.8 |
| IDS Server           | 192.168.1.50   | /24       | 192.168.1.1 | 8.8.8.8 |
| Workstation Admin    | 192.168.1.10   | /24       | 192.168.1.1 | 8.8.8.8 |



