# SW6 Entwicklung Lohndruckerei

Gemeinsame Entwicklungsumgebung für die Shopware-6-Plugins der Lohndruckerei.

Die Plugins sind eigene Repositories und werden in diesen Ordner geklont:

```bash
cd ~/projekte/sw6-entwicklung-lohndruckerei
git clone git@github.com:Lohndruckerei/sw6-mensura.git
git clone git@github.com:Lohndruckerei/sw6-vectura.git
git clone git@github.com:Lohndruckerei/sw6-lieferlotse.git
git clone git@github.com:Lohndruckerei/sw6-backend-designsystem.git ld-admin-design
```

Danach den Shop starten:

```bash
cd docker
docker compose up -d
```

- Shop: http://localhost
- Administration: http://localhost/admin

Details zu Aufbau, Container und Build stehen in [CLAUDE.md](CLAUDE.md).
