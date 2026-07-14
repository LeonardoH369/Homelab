# 🏠 Homelab — Infraestructura Multi-Capa con Docker, pfSense, VLAN y Monitoreo

![Status](https://img.shields.io/badge/status-en%20producción-brightgreen)
![Docker](https://img.shields.io/badge/Docker-Portainer-2496ED?logo=docker&logoColor=white)
![pfSense](https://img.shields.io/badge/Firewall-pfSense-212121?logo=pfsense&logoColor=white)
![Network](https://img.shields.io/badge/Switch-Cisco%203750X-1BA0D7?logo=cisco&logoColor=white)

Homelab personal con arquitectura de varias capas (firewall dedicado, switch gestionado, host de contenedores y servidor de virtualización), diseñado para replicar patrones reales de operación empresarial/cloud: segmentación de red, acceso remoto seguro, monitoreo en tiempo real y gestión centralizada de servicios.

> Este proyecto nació de la necesidad de tener un entorno propio para practicar administración de sistemas Linux/Windows Server, redes (incluyendo hardware Cisco real) y observabilidad — habilidades directamente aplicables a roles de Networking, Cloud y Ciberseguridad.

## 📑 Tabla de contenido

- [Arquitectura](#-arquitectura)
- [Componentes de la infraestructura](#-componentes-de-la-infraestructura)
- [Servicios desplegados](#-servicios-desplegados-raspberry-pi-5--docker)
- [Virtualización](#️-virtualización-pc-principal)
- [Redes y segmentación](#-redes-y-segmentación)
- [Retos técnicos](#-retos-técnicos-y-cómo-los-resolví)
- [Estructura del repositorio](#-estructura-del-repositorio)
- [Mejoras futuras](#-mejoras-futuras)
- [Tecnologías](#️-tecnologías)
- [Autor](#-autor)

---

## 📐 Arquitectura

```
Internet
   │
Router ISP
   │
pfSense (Firewall) ── PC dedicada con NIC de doble puerto
   │
Switch Cisco Catalyst 3750X (24 puertos Gigabit) ── administración de red / VLANs
   │
   ├── Raspberry Pi 5 (8GB, IP estática) ── host de contenedores Docker
   │       └── Portainer, Pi-hole, Tailscale, Heimdall, Jellyfin, Navidrome, Grafana
   │
   └── PC Principal ── host de virtualización
           ├── VM Windows Server (Active Directory / GPOs)
           └── VM Ubuntu Server
```

![Diagrama de topología](docs/network-topology.png)

**Red:** la homelab está **segmentada de la red doméstica normal**, con pfSense como punto de control entre ambas y el switch gestionado 3750X manejando el tráfico interno.

---

## 🧱 Componentes de la infraestructura

| Capa | Componente | Función |
|---|---|---|
| **Firewall** | pfSense (PC con NIC dual-puerto) | Filtrado de tráfico, segmentación entre red homelab y red doméstica |
| **Switching** | Cisco Catalyst 3750X — 24 puertos Gigabit | Administración de red interna, gestión vía consola/SSH |
| **Cómputo (contenedores)** | Raspberry Pi 5 (8GB RAM, IP estática) | Host principal de todos los servicios Dockerizados |
| **Cómputo (virtualización)** | PC principal | Corre VMs de Windows Server y Ubuntu Server |
| **Conectividad remota** | Router ISP | Salida a internet |

---

## 📦 Servicios desplegados (Raspberry Pi 5 + Docker)

| Servicio | Función |
|---|---|
| **Portainer** | Gestión visual y administración de contenedores/stacks Docker |
| **Docker Hub** | Registro de imágenes usado para los contenedores del homelab |
| **Pi-hole** | DNS local + bloqueo de publicidad/tracking a nivel de red |
| **Tailscale** | VPN mesh (WireGuard) para acceso remoto seguro sin exponer puertos al público |
| **Heimdall** | Dashboard centralizado de acceso a todos los servicios del homelab |
| **Jellyfin** | Servidor de streaming multimedia |
| **Navidrome** | Servidor de música self-hosted |
| **Grafana** | Visualización de métricas y dashboards de monitoreo |

![Stacks en Portainer](docs/portainer-stacks.png)
![Dashboard de Grafana](docs/grafana-dashboard.png)

---

## 🖥️ Virtualización (PC principal)

- **VM Windows Server** — Active Directory Domain Services (AD DS): usuarios, grupos, OUs y GPOs
- **VM Ubuntu Server** — entorno adicional de pruebas/desarrollo

---

## 🌐 Redes y Segmentación

- Homelab segmentada de la red doméstica principal mediante **pfSense**
- Switch **Cisco Catalyst 3750X** administrando el tráfico interno del homelab
- Raspberry Pi 5 con **IP estática** para garantizar disponibilidad consistente de los servicios

---

## 🔧 Retos técnicos y cómo los resolví

**Conflicto de puertos entre contenedores**
Pi-hole y otro servicio intentaban usar el mismo puerto (8080), lo que causaba que uno de los dos no levantara correctamente. Lo resolví remapeando el puerto expuesto de uno de los contenedores en el `docker-compose.yml`, entendiendo la diferencia entre el puerto interno del contenedor y el puerto publicado al host.

**Acceso inicial al servidor (Raspberry Pi 5)**
Al principio configurar el acceso remoto vía **SSH** al RPi5 fue un reto — desde la configuración inicial de la conexión hasta asegurar que fuera estable para administración remota. Una vez resuelto, esto se convirtió en la base para poder gestionar todo el homelab sin necesidad de monitor/teclado conectados directamente al Pi.

---

## 📁 Estructura del repositorio

```
homelab-docker-infra/
├── README.md
├── docker-compose/
│   └── docker-compose.yml       # Configuración de servicios (secretos redactados)
├── docs/
│   ├── network-topology.png     # Diagrama de arquitectura
│   ├── grafana-dashboard.png
│   └── portainer-stacks.png
└── scripts/                     # Scripts de backup/mantenimiento (si aplica)
```

---

## 🚧 Mejoras futuras

- [ ] Agregar Prometheus + Node Exporter como fuente de métricas para Grafana
- [ ] Automatizar backups de configuración con un script en `scripts/`
- [ ] Documentar reglas de firewall en pfSense (capturas + explicación)
- [ ] Configurar VLANs adicionales en el switch 3750X para aislar aún más los servicios
- [ ] Agregar reverse proxy (Nginx Proxy Manager / Traefik) con certificados TLS internos

---

## 🛠️ Tecnologías

`Docker` `Portainer` `Raspberry Pi 5` `pfSense` `Cisco Catalyst 3750X` `Grafana` `Pi-hole` `Tailscale` `Windows Server` `Active Directory` `Ubuntu Server` `SSH`

---

## 👤 Autor

**Leonardo Hinojosa Castro**
Estudiante de Ingeniería en Desarrollo de Software — Tecmilenio
[LinkedIn](https://www.linkedin.com/in/leonardo-hinojosa-castro-323548224/)
