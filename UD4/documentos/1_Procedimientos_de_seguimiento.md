## Seguimiento y Procedimiento del Proyecto

### 1. Reuniones de revisión de avance
Se realizan revisiones internas semanales para evaluar:
- Progreso respecto al cronograma.
- Funcionamiento de las herramientas instaladas (Nmap, Suricata, Grafana).
- Identificación de retrasos o desviaciones.
- Necesidad de ajustar tareas o redefinir prioridades.

Cada reunión genera un informe breve de seguimiento.

---

### 2. Control de hitos
El proyecto se evalúa según el cumplimiento de los siguientes hitos principales:

- **Hito 1:** Instalación y configuración del entorno (Ubuntu Server y red virtual).  
- **Hito 2:** Implementación de Suricata y configuración de reglas.  
- **Hito 3:** Integración de logs y visualización en Grafana.  
- **Hito 4:** Automatización de escaneos con Nmap.  
- **Hito 5:** Validación mediante pruebas de ataque controladas.  
- **Hito 6:** Documentación técnica final.  

Cada hito completado queda registrado con fecha y estado.

---

### 3. Seguimiento técnico continuo
Durante la implementación se supervisan aspectos clave como:
- Estado general y rendimiento del sistema.
- Correcta ingestión de logs por parte de Suricata.
- Funcionamiento del dashboard en Grafana.
- Fiabilidad y frecuencia de los escaneos de Nmap.
- Ausencia de errores en servicios del sistema (systemctl, syslog, health checks).

Las anomalías detectadas se documentan y se planifica su corrección.

---

### 4. Validación de resultados
Para confirmar que el sistema cumple los requisitos de seguridad establecidos:
- Se ejecutan escaneos Nmap desde redes simuladas.
- Se generan alertas mediante Suricata ante tráfico sospechoso.
- Se verifica que Grafana muestre correctamente los eventos.
- Se comparan los resultados antes y después de la implementación.

Los resultados se almacenan como evidencia del funcionamiento correcto.

---

### 5. Control documental
Toda la información generada se registra en:
- Diario de seguimiento.
- Documentación técnica del sistema.
- Resultados de pruebas y capturas de dashboards.
- Cambios realizados en reglas o configuraciones.

Este control asegura la trazabilidad completa del proyecto.

---

### 6. Informe final de seguimiento
Al finalizar el proyecto se elabora un informe que incluye:
- Estado final de cada fase.
- Problemas detectados y su resolución.
- Validación del cumplimiento de objetivos.
- Propuestas de mejora para la empresa.

