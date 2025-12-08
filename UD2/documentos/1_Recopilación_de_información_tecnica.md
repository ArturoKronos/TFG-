## *Planificación de la implantación del sistema de monitorización e IDS en Cocacusca*
---
# Infraestructura actual de Cocacusca Antigüedades

---

## Hardware

### Servidor web
- **Modelo:** Dell OptiPlex 7040  
- **CPU:** Intel i5-6500 @ 3.2GHz (4 cores)  
- **RAM:** 16GB DDR4  
- **Disco:** SSD 240GB + HDD 1TB  
- **Sistema operativo:** Ubuntu Server 22.04 LTS  

### Hardware disponible para IDS
- **Modelo:** HP EliteDesk 800 G2 (sin uso actual)  
- **CPU:** Intel i5-6500 @ 3.2GHz (4 cores)  
- **RAM:** 8GB DDR4  
- **Disco:** SSD 120GB  
- **Sistema operativo:** A instalar (Ubuntu Server 24.04 LTS)  

---

## Red

- **Router:** Movistar Fibra HGU (FTTH 600Mbps)  
- **Switch:** TP-Link TL-SG108 (8 puertos Gigabit)  
- **Topología:** Estrella simple  
- **Rango IP interna:** 192.168.1.0/24  
- **IP servidor web:** 192.168.1.100 (estática)  

---

## Software actual

- WordPress 6.4  
- WooCommerce 8.5  
- MySQL 8.0  
- Apache 2.4  
- PHP 8.2  
- SSH habilitado (puerto 22)  

---

## Tráfico estimado

- **Visitas web diarias:** 500-800  
- **Transacciones diarias:** 5-15  
- **Tráfico mensual:** ~50GB  
- **Picos de tráfico:** Black Friday, Navidad (~3x tráfico normal)  

---

## Amenazas detectadas (histórico de logs)

- 127 intentos fallidos de SSH el último mes (diferentes IPs)  
- 43 escaneos de puertos detectados en logs de Apache  
- 2 intentos de SQL injection bloqueados por plugin de WordPress  
- 1 ataque DDoS menor (20 min, automitigado por ISP)  



