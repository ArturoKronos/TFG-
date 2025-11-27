# Planificación de Actividades

## Fase 1: Análisis Inicial (Semana 1 – Semana 2)

### Actividades
- Reunión inicial con el entorno de trabajo definido.
- Identificación de los activos críticos de Cocacusca S.L. (servidores, web, ERP, red interna).
- Análisis del estado actual de la red y detección de las carencias de seguridad.
- Revisión de los requisitos del proyecto y definición del alcance realista.

### Entregables
- Documento de análisis de necesidades.
- Lista de riesgos actuales de la red.
- Alcance funcional del sistema de detección de intrusiones.

---

## Fase 2: Diseño Lógico y Físico (Semana 2 – Semana 3)

### Actividades
- Elaboración del diagrama lógico: flujos de red, direcciones IP, servicios.
- Diseño físico: arquitectura hardware/virtual, servidores y sensores.
- Selección final de tecnologías: Nmap, Suricata, Grafana, PostgreSQL/InfluxDB, Prometheus.
- Definición del modelo de integración entre Suricata → Base de datos → Grafana.

### Entregables
- Diagramas del diseño lógico.
- Diagrama del diseño físico.
- Documento técnico justificando la arquitectura.

---

## Fase 3: Preparación del Entorno (Semana 3 – Semana 4)

### Actividades
- Instalación del sistema operativo (Ubuntu/Debian).
- Configuración de la red local simulada según el entorno de Cocacusca S.L.
- Instalación y actualización de dependencias necesarias.
- Creación del entorno de pruebas aislado para escaneos Nmap y Suricata.

### Entregables
- Servidor base instalado y operativo.
- Red local de pruebas configurada.
- Documento de instalación inicial del sistema.

---

## Fase 4: Implementación de los Componentes Principales (Semana 4 – Semana 6)

### 4.1 Implementación de Nmap
- Programación de escaneos periódicos.
- Definición de perfiles (full scan, escaneo rápido, puertos vulnerables).
- Exportación automática de resultados en formato JSON o XML.

### 4.2 Implementación de Suricata
- Instalación y activación del motor IDS/IPS.
- Configuración del conjunto de reglas (ET Open Ruleset).
- Activación del logging EVE JSON compatible con Grafana.
- Verificación mediante tráfico de prueba.

### 4.3 Implementación de Grafana
- Instalación y configuración del panel de visualización.
- Integración de logs de Suricata y resultados de Nmap mediante base de datos.
- Creación de dashboards personalizados (alertas, actividad sospechosa, puertos abiertos, tendencias).

### Entregables
- Nmap automatizado y operativo.
- Suricata registrando y clasificando eventos correctamente.
- Grafana mostrando paneles funcionales.
- Primer prototipo del sistema IDS completo.

---

## Fase 5: Integración y Pruebas (Semana 6 – Semana 7)

### Actividades
- Pruebas de detección de intrusiones simuladas.
- Pruebas de rendimiento (carga de logs y latencia de alertas).
- Verificación del flujo completo:  
  **Nmap → Suricata → Logs → Base de datos → Grafana**
- Corrección de fallos y ajustes de reglas.

### Entregables
- Informe de pruebas funcionales.
- Ajustes y correcciones aplicadas.
- Validación interna del sistema.


