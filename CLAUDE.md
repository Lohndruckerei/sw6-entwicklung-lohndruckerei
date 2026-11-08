# SW6 Entwicklung Lohndruckerei

Oberordner für alle echten Shopware-6-Plugins der Lohndruckerei und ihre gemeinsame Umgebung.
Gilt für jede Sitzung in diesem Ordner oder einem Unterordner. Projektspezifisches steht in der
CLAUDE.md des jeweiligen Plugins.

## Aufbau
| Ordner | Inhalt | GitHub |
|---|---|---|
| `sw6-mensura/` | Plugin LdMensura (Konfigurator, Kalkulation) | Lohndruckerei/sw6-mensura |
| `sw6-mensura-frontend/`, `sw6-mensura-logistik/` | Git-Worktrees von sw6-mensura (eigene Branches) | – |
| `sw6-vectura/` | Plugin LdVectura (Versand, Verpackung) | Lohndruckerei/sw6-vectura |
| `sw6-lieferlotse/` | Plugin LdLieferLotse (Lieferanten) | Lohndruckerei/sw6-lieferlotse |
| `ld-admin-design/` | gemeinsames Admin-Design (Quelle, per `sync.sh` verteilt) | Lohndruckerei/sw6-backend-designsystem |
| `docker/` | `docker-compose.yml` und `backups/` (DB-Dumps, nicht versioniert) | dieses Repo |

Jedes Plugin ist ein eigenes Git-Repository und wird für sich versioniert, installiert und
aktualisiert. Dieses Oberverzeichnis versioniert nur CLAUDE.md, README.md und `docker/`.
Spielereien und Tests gehören nach `~/projekte/sw6-tests` (Repo Lohndruckerei/sw6-tests), nicht hierher.

## Docker-Umgebung
- Ein gemeinsamer Container `shopware` (Shopware 6.7, Dockware) für alle Plugins, definiert in
  `docker/docker-compose.yml` (Compose-Projekt `sw6-entwicklung-lohndruckerei`).
- Compose immer in WSL ausführen (`cd ~/projekte/sw6-entwicklung-lohndruckerei/docker && docker compose up -d`),
  weil die Mount-Pfade WSL-Pfade sind.
- Shop und Datenbank liegen in den Volumes `lohndruckerei-sw6-plugins_shop` und `…_db` und überstehen
  ein Neuerstellen des Containers. Vor größeren Eingriffen DB-Dump nach `docker/backups/`.
- Neues Plugin: Ordner hier anlegen, Zeile unter `volumes` in der Compose-Datei ergänzen,
  `docker compose up -d`, dann `bin/console plugin:refresh`.
- Shopware-Befehle: `docker exec shopware bash -c "cd /var/www/html && bin/console …"`
- Shop: http://localhost · Administration: http://localhost/admin

## Bauen im Container
- Die Docker-VM hat wenig Arbeitsspeicher; der volle Admin-Build läuft dann in den Speicher-Engpass.
  Plugins deshalb nur so bauen:
  `docker exec shopware bash -c "cd /var/www/html && SHOPWARE_ADMIN_BUILD_ONLY_EXTENSIONS=1 bin/build-administration.sh"`,
  danach `bin/console assets:install` und `cache:clear`.
- Der Build läuft als www-data. Gehören Dateien in `<plugin>/src/Resources/public` dem WSL-Benutzer,
  schlägt er mit EACCES fehl → vorher `docker exec -u root shopware chmod -R a+rwX <pfad>`.
- Umgekehrt gehören Build-Ergebnisse danach www-data; vor Branch-Wechseln in WSL
  `docker exec -u root shopware chown -R 1000:1000 <plugin>/src/Resources/public`.

## Gemeinsame Regeln
- Admin-Design: siehe globale CLAUDE.md und `ld-admin-design/DESIGN.md`.
- Plugins nur lose koppeln (Events, IDs ohne Fremdschlüssel); jedes Plugin muss allein lauffähig sein.
- Git: je Plugin-Repo committen und pushen nach den Regeln in dessen CLAUDE.md; nie force-pushen.
