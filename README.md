# sysadmin-toolkit

Scripts de automatización para administración de sistemas. PowerShell + Python + Bash.

Herramientas que uso (y que he construido) para tareas reales de soporte N2: gestión de usuarios en AD, monitorización de sistemas, diagnóstico de red, backup verification y parseo de logs.

---

## Quick start

```bash
git clone https://github.com/SamLorenzoSanc/sysadmin-toolkit.git
cd sysadmin-toolkit
```

Cada script tiene `--help`. No necesitas leer documentación para usarlo.

```powershell
# Crear usuarios en AD desde un CSV
.\ad-tools\New-BulkUsers.ps1 -CsvPath .\examples\users.csv -OU "OU=Empresa,DC=local"

# Auditoría rápida de un AD
.\ad-tools\Get-ADHealthCheck.ps1 -ExportPath .\reports\
```

```bash
# Diagnóstico de red completo
python3 net-tools/netcheck.py --target 10.0.0.0/24 --report

# Monitor de salud del sistema
python3 monitoring/syshealth.py --alert-email admin@empresa.com

# Verificar integridad de backups
python3 backup-tools/verify_backup.py --path /mnt/backups --last 7d
```

---

## Herramientas incluidas

### ad-tools/ — Active Directory (PowerShell)

| Script | Qué hace |
|--------|----------|
| `New-BulkUsers.ps1` | Crea usuarios en lote desde CSV (nombre, OU, grupos, contraseña temporal) |
| `Get-ADHealthCheck.ps1` | Auditoría: cuentas inactivas, contraseñas expiradas, grupos vacíos |
| `Disable-InactiveUsers.ps1` | Desactiva cuentas sin login en X días (con log y rollback) |
| `Export-ADReport.ps1` | Exporta informe de AD a CSV/HTML para documentación |
| `Sync-GroupMembership.ps1` | Sincroniza miembros de grupo desde un CSV maestro |

### net-tools/ — Diagnóstico de red (Python)

| Script | Qué hace |
|--------|----------|
| `netcheck.py` | Escaneo de subred: ping sweep, puertos abiertos, DNS check |
| `dns_audit.py` | Verifica registros DNS vs inventario (detecta huérfanos) |
| `vpn_monitor.py` | Monitoriza túneles VPN y alerta si caen |
| `bandwidth_test.py` | Test de ancho de banda entre dos puntos con reporte |

### monitoring/ — Monitorización (Python + Bash)

| Script | Qué hace |
|--------|----------|
| `syshealth.py` | CPU, RAM, disco, servicios críticos — alerta por email/Telegram |
| `service_watchdog.sh` | Reinicia servicios caídos automáticamente (systemd) |
| `log_parser.py` | Parsea logs de sistema y genera resumen de errores/warnings |
| `disk_alert.py` | Alerta cuando un disco supera umbral de uso |

### backup-tools/ — Backups (Python)

| Script | Qué hace |
|--------|----------|
| `verify_backup.py` | Verifica integridad de backups (hash + tamaño + antigüedad) |
| `backup_report.py` | Genera informe de estado de backups de los últimos N días |
| `rsync_wrapper.py` | Wrapper de rsync con logging, reintentos y notificación |

### m365-tools/ — Microsoft 365 (PowerShell)

| Script | Qué hace |
|--------|----------|
| `Get-M365LicenseReport.ps1` | Informe de licencias: asignadas, disponibles, por usuario |
| `New-SharedMailbox.ps1` | Crea buzón compartido con permisos desde plantilla |
| `Export-InactiveM365Users.ps1` | Lista usuarios M365 sin actividad en X días |

---

## Estructura del repo

```
├── ad-tools/                # Active Directory (PowerShell)
│   ├── New-BulkUsers.ps1
│   ├── Get-ADHealthCheck.ps1
│   └── ...
├── net-tools/               # Diagnóstico de red (Python)
│   ├── netcheck.py
│   ├── dns_audit.py
│   └── ...
├── monitoring/              # Monitorización (Python + Bash)
│   ├── syshealth.py
│   ├── service_watchdog.sh
│   └── ...
├── backup-tools/            # Gestión de backups (Python)
│   ├── verify_backup.py
│   └── ...
├── m365-tools/              # Microsoft 365 (PowerShell)
│   ├── Get-M365LicenseReport.ps1
│   └── ...
├── examples/                # CSVs de ejemplo, configs
│   ├── users.csv
│   └── config.yaml
├── tests/                   # Tests unitarios
├── docs/                    # Documentación extra
│   └── SETUP_LAB.md         # Cómo montar un lab local para probar
├── requirements.txt
├── .gitignore
├── LICENSE
└── README.md
```

---

## Entorno de pruebas

Todos los scripts se han probado en un lab local:

- **Windows Server 2022** (VM) — Active Directory, DNS, DHCP, GPO
- **Ubuntu 22.04** (VM) — Monitoring, net-tools, backup
- **VirtualBox / Hyper-V** — Virtualización del lab
- **pfSense** (opcional) — Firewall y routing del lab

Instrucciones para montar tu propio lab: [`docs/SETUP_LAB.md`](docs/SETUP_LAB.md)

---

## Por qué este repo existe

Cuando trabajas en soporte N2, haces las mismas tareas 50 veces al mes: crear usuarios, revisar backups, comprobar que un servicio sigue vivo, diagnosticar por qué una subred no responde. Puedes hacerlo a mano cada vez, o puedes automatizarlo una vez y dedicar tu tiempo a lo que importa.

Este repo es lo segundo.

---

## Requisitos

**Python >= 3.10**
```bash
pip install -r requirements.txt
```

**PowerShell 7+ con módulos:**
```powershell
Install-Module ActiveDirectory
Install-Module ExchangeOnlineManagement
Install-Module Microsoft.Graph
```

---

## Contribuir

Si trabajas en soporte y tienes un script que te ahorra tiempo, abre un PR. La única regla es que el script tenga `--help` y sea legible.

---

*Hecho por [Samuel Lorenzo](https://github.com/SamLorenzoSanc)
