# Definición de objetivos y fases del proyecto

---

## **Objetivo General**

Diseñar e implantar una solución de seguridad integral capaz de analizar el tráfico de red, detectar intrusiones y visualizar alertas en tiempo real, reforzando la protección de los servidores internos y la red local de **Cocacusca S.L.**

---

## **Objetivos Específicos**

### **Aumentar la visibilidad del tráfico de red**
Mediante un sistema IDS capaz de identificar comportamientos anómalos en la red corporativa.

### **Detectar vulnerabilidades en servidores y equipos internos**
A través de escaneos periódicos y automatizados con **Nmap**.

### **Implementar Suricata como sistema IDS**
Para realizar inspección profunda del tráfico y detectar amenazas conocidas o patrones sospechosos.

### **Centralizar todos los logs de seguridad**
Evitando su almacenamiento disperso y facilitando el análisis forense y la organización del histórico.

### **Incorporar Grafana como herramienta visual**
Permitiendo visualizar en tiempo real:
- Alertas  
- Eventos generados por Suricata  
- Escaneos realizados con Nmap  
- Tendencias y métricas de actividad sospechosa  

### **Incrementar la seguridad de la plataforma web y del ERP**
Reduciendo el riesgo de accesos no autorizados, ataques de red y vulnerabilidades no detectadas.

### **Generar documentación técnica completa**
Para facilitar mantenimiento, ampliaciones futuras y auditorías internas.

---

## **Fases del Proyecto**

---

## **Fase 1 — Análisis del entorno y requisitos**
- Estudio detallado de la red actual.  
- Identificación de activos críticos (ERP, web corporativa, red de oficina, producción).  
- Revisión de los riesgos actuales derivados de la falta de monitorización.  
- Definición del alcance del sistema IDS.  

---

## **Fase 2 — Diseño de la arquitectura de seguridad**
- Diseño lógico (flujo de tráfico, direccionamiento IP, servicios).  
- Diseño físico (servidores, contenedores, ubicación del IDS).  
- Definición del punto donde se capturará el tráfico de red.  
- Selección de una red interna Docker para aislamiento.  

---

## **Fase 3 — Implementación de Suricata, Nmap y servidor de logs**
- Instalación del servidor base en Cocacusca S.L.  
- Configuración de **Suricata** en modo IDS para monitorizar el tráfico.  
- Creación e implementación de un contenedor **Nmap** para escaneos programados.  
- Configuración del sistema de almacenamiento centralizado de logs.  

---

## **Fase 4 — Integración y pruebas del sistema de seguridad**
- Integración de logs generados por Suricata y Nmap.  
- Pruebas internas simulando ataques y escaneos.  
- Ajuste de reglas de Suricata según comportamiento real de la red.  
- Validación del rendimiento en la infraestructura de Cocacusca.  

---

## **Fase 5 — Implementación de Grafana y sistema de alertas**
- Creación de dashboards personalizados según las necesidades de la empresa.  
- Visualización del estado de la red, eventos y métricas.  
- Configuración de alertas críticas mediante correo corporativo.  
- Pruebas de notificaciones ante incidentes detectados.  

---

## **Fase 6 — Documentación, formación y despliegue final**
- Elaboración de documentación técnica completa.  
- Redacción de un manual de uso para el personal técnico.  
- Instrucciones específicas para la resolución de incidentes detectados por Suricata.  
- Despliegue final del sistema en el entorno de producción.  

---

