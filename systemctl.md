Hier sind einige nützliche Tipps und Tricks für die Verwendung von `systemctl`, dem Kommandozeilenwerkzeug zur Verwaltung von Systemdiensten unter systemd:

### 1. Dienste verwalten

- **Starten eines Dienstes:**
  ```bash
  sudo systemctl start <dienstname>
  ```
  Beispiel:
  ```bash
  sudo systemctl start nginx.service
  ```

- **Stoppen eines Dienstes:**
  ```bash
  sudo systemctl stop <dienstname>
  ```
  Beispiel:
  ```bash
  sudo systemctl stop nginx.service
  ```

- **Neustarten eines Dienstes:**
  ```bash
  sudo systemctl restart <dienstname>
  ```
  Beispiel:
  ```bash
  sudo systemctl restart nginx.service
  ```

- **Neuladen der Konfiguration eines Dienstes:**
  ```bash
  sudo systemctl reload <dienstname>
  ```
  Beispiel:
  ```bash
  sudo systemctl reload nginx.service
  ```

- **Status eines Dienstes überprüfen:**
  ```bash
  sudo systemctl status <dienstname>
  ```
  Beispiel:
  ```bash
  sudo systemctl status nginx.service
  ```

### 2. Automatisches Starten von Diensten beim Booten

- **Einen Dienst aktivieren:**
  ```bash
  sudo systemctl enable <dienstname>
  ```
  Beispiel:
  ```bash
  sudo systemctl enable nginx.service
  ```

- **Einen Dienst deaktivieren:**
  ```bash
  sudo systemctl disable <dienstname>
  ```
  Beispiel:
  ```bash
  sudo systemctl disable nginx.service
  ```

### 3. Logdateien anzeigen

- **Logdateien eines Dienstes anzeigen:**
  ```bash
  sudo journalctl -u <dienstname>
  ```
  Beispiel:
  ```bash
  sudo journalctl -u nginx.service
  ```

- **Echtzeit-Logs anzeigen (wie `tail -f`):**
  ```bash
  sudo journalctl -f -u <dienstname>
  ```
  Beispiel:
  ```bash
  sudo journalctl -f -u nginx.service
  ```

### 4. Systemstatus und Boot-Informationen

- **Gesamten Systemstatus anzeigen:**
  ```bash
  sudo systemctl status
  ```

- **Liste aller Dienste und deren Status anzeigen:**
  ```bash
  sudo systemctl list-units --type=service
  ```

- **Boot-Logs anzeigen:**
  ```bash
  sudo journalctl -b
  ```

### 5. Units konfigurieren

- **Eine Unit neu laden (nach Änderung der Unit-Datei):**
  ```bash
  sudo systemctl daemon-reload
  ```

- **Aktive Units anzeigen:**
  ```bash
  sudo systemctl list-units
  ```

- **Fehlgeschlagene Units anzeigen:**
  ```bash
  sudo systemctl --failed
  ```

### 6. Zielzustände (Targets) verwalten

- **In den Rettungsmodus wechseln:**
  ```bash
  sudo systemctl rescue
  ```

- **In den Wiederherstellungsmodus wechseln:**
  ```bash
  sudo systemctl emergency
  ```

- **Das System herunterfahren:**
  ```bash
  sudo systemctl poweroff
  ```

- **Das System neu starten:**
  ```bash
  sudo systemctl reboot
  ```

Diese Befehle und Tipps sollten dir helfen, `systemctl` effektiv zu nutzen und dein System und deine Dienste effizient zu verwalten.