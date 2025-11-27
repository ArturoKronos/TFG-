## Gestión de Incidencias y Cambios

### 1. Registro de incidencias
Todas las incidencias detectadas se documentan incluyendo:
- Fecha y hora.
- Responsable que la detecta.
- Servicio afectado (Nmap, Suricata, Grafana, red, sistema operativo).
- Descripción detallada del problema.
- Logs relevantes (si existen).
- Nivel de criticidad:
  - **Alta:** afecta la detección de intrusos o provoca caída del sistema.
  - **Media:** reduce funcionalidades pero no invalida el servicio.
  - **Baja:** incidencias menores o estéticas.

Este registro asegura trazabilidad y priorización de acciones.

---

### 2. Clasificación y priorización
Cada incidencia se clasifica según:
- **Impacto:** número de componentes afectados.
- **Urgencia:** si interrumpe procesos críticos.
- **Probabilidad de repetición:** posibilidad de que ocurra nuevamente.

Se determina así si la incidencia se atiende de inmediato o se programa para mantenimiento.

---

### 3. Análisis y resolución
Los pasos para resolver incidencias son:
1. Revisión de logs (Suricata, syslog y otros servicios).  
2. Reproducción del error para comprender su origen.  
3. Aplicación de soluciones técnicas, que pueden incluir:
   - Reconfiguración de reglas de Suricata.
   - Ajuste de dashboards o consultas en Grafana.
   - Corrección de parámetros de Nmap automatizado.
   - Reinicio o reparación de servicios fallidos.
4. Verificación post-solución:
   - La incidencia no reaparece.
   - No se generan efectos secundarios.

---

### 4. Gestión de cambios
Si una incidencia requiere modificación del sistema, se sigue un procedimiento formal:
- **Solicitud de cambio (RFC)** indicando:
  - Descripción del cambio.
  - Justificación.
  - Componentes afectados.
  - Riesgos asociados.
  - Evaluación del impacto.
- Aprobación del cambio (si procede).  
- Implementación controlada en copia o entorno de pruebas.  
- Prueba posterior antes de integrar al entorno principal.

---

### 5. Registro histórico
Se documentan todos los cambios realizados, incluyendo:
- Reglas de Suricata añadidas o modificadas.
- Nuevos paneles o consultas en Grafana.
- Scripts o parámetros de automatización de Nmap.
- Configuraciones del sistema operativo o red.

Este registro facilita auditorías, revisiones y futuras actualizaciones.

---

### 6. Revisión periódica
Mensualmente o al finalizar el proyecto se revisa el historial para:
- Identificar incidencias recurrentes.
- Determinar mejoras en configuraciones.
- Evaluar la necesidad de ampliar las reglas de seguridad.

