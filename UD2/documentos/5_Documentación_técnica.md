# 5. Documentación Técnica

---

## **5.1. Justificación de la Tecnología**

### **5.1.1. Razón para elegir Suricata como IDS**

Suricata fue seleccionado como motor principal del sistema IDS por los siguientes motivos:

- Motor multihilo capaz de analizar tráfico a alta velocidad, adecuado para la red de Cocacusca S.L., donde conviven equipos administrativos, de producción y servidores.  
- Incluye Deep Packet Inspection, permitiendo detectar patrones de ataque, intrusiones o escaneos.  
- Compatible con reglas estándar como **ET Open Rules**, adaptadas a amenazas frecuentes en PYMEs.  
- Genera logs estructurados (EVE JSON), fácilmente integrables en sistemas de monitorización como Grafana.  
- Es software libre, por lo que no supone coste adicional.

---

### **5.1.2. Razón para elegir Nmap como herramienta de análisis**

Nmap se integra como herramienta de auditoría interna debido a que:

- Identifica puertos expuestos en servidores internos.  
- Detecta servicios y versiones, ayudando a localizar vulnerabilidades potenciales.  
- Permite automatización mediante tareas programadas para escaneos periódicos.  
- Su salida en XML o texto puede centralizarse junto con los logs de Suricata.

---

### **5.1.3. Razón para elegir Grafana como sistema de visualización**

Grafana se seleccionó como plataforma de visualización por las siguientes razones:

- Permite crear paneles en tiempo real que muestran:
  - Alertas generadas por Suricata  
  - Escaneos realizados por Nmap  
  - Actividad potencialmente maliciosa  
- Ofrece un motor de consultas avanzado para correlación de eventos.  
- Soporta múltiples fuentes de datos, facilitando la integración.  
- Es completamente gratuito y open source, ideal para pequeñas empresas.

---

### **5.1.4. Razón para elegir Linux como sistema operativo**

El sistema se basa en un servidor Linux (Ubuntu/Debian) porque:

- Aporta estabilidad en entornos de red y monitorización.  
- Es más seguro frente a malware que Windows Server en infraestructuras sencillas.  
- Permite ejecutar Suricata y Nmap de forma eficiente.  
- No requiere licencias, reduciendo costes.  
- Ofrece flexibilidad en la configuración y gestión de logs.

---

## **5.2. Software Utilizado**

| Componente | Función en el Sistema |
|------------|------------------------|
| **Suricata IDS** | Monitorización del tráfico en tiempo real y detección de intrusiones. |
| **Nmap** | Análisis de puertos, servicios y vulnerabilidades internas. |
| **Grafana** | Panel de visualización para alertas y actividad sospechosa. |
| **Servidor Linux (Ubuntu/Debian)** | Plataforma principal donde se ejecutan todos los servicios. |
| **Motor de logs (Loki / syslog / Elasticsearch opcional)** | Sistema de almacenamiento centralizado de registros. |
| **hping3 / herramientas de prueba** | Simulación de ataques y tráfico anómalo durante la fase de pruebas. |

---

