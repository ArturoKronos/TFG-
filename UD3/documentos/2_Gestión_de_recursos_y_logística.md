# Gestión de Recursos y Logística

La correcta ejecución del proyecto requiere una planificación coordinada de los recursos humanos, materiales y técnicos, junto con una organización logística que garantice el desarrollo fluido de cada fase.

---

### 2.1 Recursos Humanos

Aunque el proyecto se desarrolla a nivel individual dentro del contexto académico, se identifican los siguientes roles:

#### Responsable del proyecto — Arturo Kronos Fernández Curiel
- Planificación y gestión del desarrollo.
- Instalación y configuración de Nmap, Suricata y Grafana.
- Realización de pruebas, documentación técnica y memoria final.

#### Usuario final de referencia — Cocacusca S.L.
- Empresa ficticia que actúa como modelo de infraestructura.
- Define las necesidades de seguridad simuladas en el proyecto.

---

### 2.2 Recursos Materiales

#### Equipos físicos
- Ordenador personal con capacidad de virtualización.
- Switch o router doméstico (opcional).
- Conexión a internet para descargas y actualizaciones.

#### Infraestructura virtual
- 1 VM principal (servidor de seguridad con Suricata, Grafana y base de datos).
- 1 VM secundaria para simular un cliente o atacante.
- VM adicional opcional como servidor objetivo para pruebas de Nmap.

#### Recursos de software
- Ubuntu Server 22.04 LTS o Debian 12.
- Nmap para análisis de red.
- Suricata como motor IDS.
- Grafana para visualización de eventos.
- PostgreSQL o InfluxDB como base de datos.
- Herramientas auxiliares: SSH, Git/GitHub, Cron, jq o Python.

---

### 2.3 Logística del Proyecto

#### Organización del entorno de pruebas
- La VM principal actúa como servidor de seguridad.
- La VM secundaria simula servicios reales para generar tráfico.
- Suricata procesa los eventos generados.
- Nmap ejecuta escaneos periódicos desde el servidor de seguridad.

#### Adquisición e instalación de herramientas
- Todo el software empleado es open source.
- Instalación desde repositorios oficiales.
- El entorno virtual se construye con VirtualBox, VMware o Proxmox.

#### Gestión del tiempo
- Las tareas se distribuyen por fases semanales.
- Cada fase incluye margen para resolver incidencias técnicas.

---

### 2.4 Gestión de Riesgos y Contingencias

- Fallo en Suricata: uso de snapshots para revertir.
- Errores en Grafana o la base de datos: rollback a versiones estables.
- Exceso de logs: configuración de rotación con logrotate.
- Bajo rendimiento de VM: ajuste de recursos (CPU/RAM).
- Problemas de compatibilidad: uso de versiones LTS estables.

---

### 2.5 Presupuesto Logístico

El proyecto no requiere inversión económica, ya que se basa en herramientas open source y hardware disponible.

**Coste total estimado:** 0 €

