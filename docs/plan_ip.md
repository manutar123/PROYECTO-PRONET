# 🌐 Proyecto PRONET - Plan Maestro de Direccionamiento IP

**Versión:** 1.2  

## 🛠 1. Macro-Segmentación (Secciones Estratégicas)
Esta tabla define los bloques globales sobre los cuales se construye toda la infraestructura interna de PRONET mediante técnicas de VLSM.

| SECCIÓN | USO / DESCRIPCIÓN | SEGMENTO IPv4 | MÁSCARA | PREFIJO IPv6 (/64) | CAPACIDAD |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **1. FIBRA** | Clientes FTTH (Corp/Res) | `10.1.0.0 /18` | 255.255.192.0 | `2001:DB8:FACE:10::/60` | 16,382 IPs |
| **2. RADIO** | Clientes WISP (Corp/Res) | `10.1.64.0 /18` | 255.255.192.0 | `2001:DB8:FACE:20::/60` | 16,382 IPs |
| **3. TRANSPORTE** | Enlaces PtP (Backbone) | `10.1.254.0 /23` | 255.255.254.0 | `2001:DB8:FACE:30::/64` | 510 IPs |
| **4. ADMIN** | Gestión, LBs y SVI | `10.1.252.0 /23` | 255.255.254.0 | `2001:DB8:FACE:90::/60` | 510 IPs |

---

## 🛣 2. Sub-segmentación de Servicios y VLANs
Distribución lógica para la aplicación de políticas de Calidad de Servicio (QoS) y seguridad perimetral (ACLs).

| VLAN | SERVICIO | SEGMENTO IPv4 | GATEWAY | PREFIJO IPv6 (/64) |
| :---: | :--- | :--- | :--- | :--- |
| **10** | Servicios NOC / DNS | `10.1.252.128 /25` | 10.1.252.129 | `2001:DB8:FACE:10::/64` |
| **60** | Residencial Fibra | `10.1.4.0 /18` | 10.1.4.1 | `2001:DB8:FACE:60::/64` |
| **50** | Residencial Radio | `10.1.68.0 /18` | 10.1.68.1 | `2001:DB8:FACE:50::/64` |
| **99** | Gestión Admin (OOB) | `10.1.252.0 /24` | 10.1.252.1 | `2001:DB8:FACE:99::/64` |
| **100** | Corporativos Fibra | `10.1.0.0 /22` | 10.1.0.1 | `2001:DB8:FACE:10::/64` |
| **200** | Corporativo Radio | `10.1.64.0 /22` | 10.1.64.1 | `2001:DB8:FACE:20::/64` |

---

## ☁ 3. Salidas a Internet (Pools Públicos WAN)
Recursos de direccionamiento público para NAT/PAT y Peering Dual-Stack.

| PROVEEDOR | ROL | BLOQUE IPv4 | GW (PEER) | IP WAN (CORE) | TRANSPORTE IPv6 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **UFINET** | Principal | `200.10.20.0 /28` | 200.10.20.14 | 200.10.20.13 | `...:30::129` |
| **STARLINK** | Respaldo | `190.50.10.0 /28` | 190.50.10.14 | 190.50.10.13 | `...:30::133` |

### 🎯 Estrategia NAT/PAT
| IP PÚBLICA | USO ESPECÍFICO | MAPEO INTERNO (NAT) |
| :--- | :--- | :--- |
| **200.10.20.1** | DNS / Servicios Críticos | 10.1.252.10 |
| **200.10.20.2** | Corporativos Radio/Fibra | 10.1.0.0/22 - 10.1.64.0/22 |
| **200.10.20.4-12** | Pool Residencial (PAT) | 10.1.4.0/18 - 10.1.68.0/18 |

---

## 🆔 4. Infraestructura de Red (Loopbacks e Identidad)
Router-ID para protocolos OSPF/BGP y gestión remota segura (SSH).

| DISPOSITIVO | IPv4 LOOPBACK 0 | IPv6 LOOPBACK 0 (/128) | FUNCIÓN TÉCNICA |
| :--- | :--- | :--- | :--- |
| **CORE-PRONET** | `10.1.252.1` | `...:FE::1` | Gateway Principal / Borde |
| **ADMIN-RADIO** | `10.1.252.2` | `...:FE::2` | Agregador Segmento WISP |
| **AD-FIBRA** | `10.1.252.3` | `...:FE::3` | Agregador Segmento FTTH |
| **R-UFINET** | `8.8.8.8` | `2001:4860:4860::8888` | Simulación Google DNS |
| **R-STARLINK** | `1.1.1.1` | `2606:4700:4700::1111` | Simulación Cloudflare |
| **R1 - R4** | `10.1.252.11-14` | `...:FE::11-14` | Nodos Distribución Radio |

---

## 🔗 5. Matriz de Enlaces Punto a Punto (Backbone Interno)
Conexiones físicas de alta velocidad entre los nodos de agregación y el núcleo.

| ENLACE | IP EQUIPO A | IP EQUIPO B | SEGMENTO IPv4 | PREFIJO IPv6 |
| :--- | :--- | :--- | :--- | :--- |
| **CORE ↔ ADMIN-RADIO** | .1 (CORE) | .2 (A-RAD) | `10.1.254.0 /30` | `...:30::0/126` |
| **CORE ↔ AD-FIBRA** | .5 (CORE) | .6 (A-FIB) | `10.1.254.4 /30` | `...:30::4/126` |
| **ADMIN-RADIO ↔ R1** | .9 (A-RAD) | .10 (R1) | `10.1.254.8 /30` | `...:30::8/126` |
| **R1 ↔ R2** | .17 (R1) | .18 (R2) | `10.1.254.16 /30` | `...:30::16/126` |

---
*Documentación generada para el entorno de laboratorio Cisco Packet Tracer - PRONET.*