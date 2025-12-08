# *Cumplimiento legal y buenas prácticas en seguridad informática*
---
---
## Cumplimiento normativo

### Reglamento General de Protección de Datos (RGPD)

#### Artículo 32 – Seguridad del tratamiento
> “El responsable y el encargado del tratamiento aplicarán medidas técnicas y organizativas apropiadas para garantizar un nivel de seguridad adecuado al riesgo.”

Cocacusca almacena datos personales de clientes (nombres, direcciones, correos electrónicos y datos de pago). La implantación de un sistema IDS constituye una medida técnica adecuada para cumplir este requisito.

---

#### Artículo 33 – Notificación de violaciones de seguridad
> “En caso de violación de la seguridad de los datos personales, el responsable notificará en un plazo de 72 horas.”

El sistema propuesto permite:
- Detectar brechas de seguridad en tiempo real  
- Registrar fecha, hora y tipo de incidente  
- Generar evidencia para notificación ante autoridades  

---

#### Artículo 34 – Comunicación a los interesados
> “Cuando sea probable que la violación suponga un alto riesgo para los derechos y libertades, se comunicará al interesado.”

El IDS proporciona:
- Alertas automáticas para respuesta inmediata  
- Registro de potenciales afectados  
- Documentación precisa del incidente  

---

## Ley de Servicios de la Sociedad de la Información y del Comercio Electrónico (LSSI-CE)

### Artículo 12 – Deber de seguridad
Los prestadores de servicios están obligados a implementar las medidas técnicas necesarias para garantizar la seguridad de los datos y los servicios digitales.

---

## Esquema Nacional de Seguridad (ENS)

Aunque no resulta obligatorio para empresas privadas, establece buenas prácticas aplicables en materia de seguridad TIC, entre ellas:

- op.mon.1: Sistema de monitorización  
- op.mon.2: Sistema de detección de intrusiones  
- op.exp.8: Registro de actividad  

---

## Normativa PCI DSS (Payment Card Industry Data Security Standard)

Dado que Cocacusca procesa pagos mediante tarjeta, son aplicables los siguientes requisitos del estándar PCI DSS:

- Req. 10: Rastrear y monitorizar todos los accesos a recursos de red  
- Req. 11.4: Uso de sistemas de detección o prevención de intrusiones  
