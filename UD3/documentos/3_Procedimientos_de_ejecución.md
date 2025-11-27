## PASO 0 — Preparación básica del sistema
```bash
sudo apt update && sudo apt upgrade -y

```
```bash
sudo apt install -y curl wget git apt-transport-https software-properties-common

```
Dependencias comunes para Suricata, Promtail y compilación ligera
## PASO 1 — Instalar Suricata y preparar logs
```bash
sudo apt install -y suricata

```
```bash
suricata --version
```
Deberías ver la versión instalada.
### 2. Activar Suricata como servicio
```bash
sudo systemctl enable --now suricata
sudo systemctl status suricata --no-pager
```
Esto confirmará que Suricata está activo.
## 3. Configurar salida de alertas en JSON
Suricata necesita guardar las alertas en un fichero que luego Promtail enviará a Loki.
Edita el fichero principal de configuración:
```bash
sudo nano /etc/suricata/suricata.yaml

```
Busca la sección *outputs:* y reemplázala o añade lo siguiente:
```bash
outputs:
  - eve-log:
      enabled: yes
      filename: /var/log/suricata/eve.json
      types:
        - alert:
            payload: yes
            payload-printable: yes
            packet: yes
        - http
        - dns
        - tls
        - flow
        - drop

```
