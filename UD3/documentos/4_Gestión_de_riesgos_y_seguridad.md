## 4. Gestión de Riesgos y Seguridad

### 4.1 Matriz de Riesgos

| Riesgo                     | Descripción                               | Impacto | Probabilidad | Medidas de Mitigación                    |
|---------------------------|-------------------------------------------|---------|--------------|-------------------------------------------|
| Fallos en Suricata        | Configuración incorrecta de reglas/logs   | Alto    | Media        | Snapshots, pruebas incrementales          |
| Integración Suricata-Grafana | Logs no se muestran correctamente      | Medio   | Media        | Verificación paso a paso, pruebas de datos |
| Sobrecarga de logs        | Generación excesiva de datos              | Alto    | Alta         | Rotación de logs, limitar reglas activas   |
| Errores en Nmap           | Escaneos mal diseñados o programados      | Medio   | Media        | Escaneos conservadores en red aislada     |
| Fallos en máquinas virtuales | Corrupción o error crítico en VM       | Alto    | Baja         | Snapshots y copias de seguridad periódicas |

---

### 4.2 Gestión de Seguridad

La gestión de seguridad del proyecto garantiza que el sistema de detección de intrusos se mantenga robusto y fiable, protegiendo la integridad del entorno de pruebas y los datos generados.

#### Aislamiento de la red de pruebas
Todas las máquinas virtuales (servidor con Suricata, cliente, servidor objetivo) funcionan en una **red interna aislada**, evitando interferencias con la red real.

#### Reglas de Suricata estables
Inicialmente se utilizan reglas estándar probadas (ET Open Rules), reduciendo falsos positivos/negativos antes de aplicar reglas personalizadas.

#### Rotación y almacenamiento seguro de logs
Los logs de Suricata y Nmap se almacenan de forma centralizada y se gestionan mediante rotación periódica para evitar saturación del disco.

#### Pruebas controladas con Nmap
Los escaneos se ejecutan con parámetros conservadores dentro de la red aislada, simulando tráfico atacante sin comprometer servicios clave.

#### Snapshots y copias de seguridad
Antes de realizar cambios críticos en Suricata, Grafana o la base de datos, se generan **snapshots** que permiten revertir rápidamente el sistema si ocurre un fallo.

#### Monitoreo continuo con Grafana
Se utilizan dashboards en tiempo real que muestran actividad de red, alertas generadas y métricas clave, permitiendo identificar anomalías de forma inmediata.

#### Documentación y control de cambios
Cada regla, script o configuración modificada en Nmap, Suricata o Grafana queda documentada, asegurando trazabilidad y evitando configuraciones inseguras en el futuro.

