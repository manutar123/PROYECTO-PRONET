# 📋 Matriz Maestra de Identidad y Gestión - PRONET

Esta tabla consolida la identidad lógica de todos los activos de red, segmentada por niveles jerárquicos. Es la referencia oficial para el acceso administrativo (SSH) y la identificación de Router-ID en los protocolos de ruteo.

| Nivel | Equipo (Hostname) | Función Principal | VLAN Gestión | IP Loopback 0 (/32) | IPv6 Loopback 0 (/128) | IP SVI / Puerto (VLAN 99 - /25) |
| :--- | :--- | :--- | :---: | :--- | :--- | :--- |
| **Core** | `CORE-PRONET` | Borde / NAT / Ruteo Global | 99 | `10.1.252.1` | `2001:DB8:FACE:FE::1` | `10.1.252.254` (GW Subint) |
| **Core** | `SW-CORE` | Distribución Central | 99 | N/A (Capa 2/3) | N/A | `10.1.252.131` (SVI) |
| **Core** | `SRV-NOC-PRONET` | Cerebro Operativo (NOC) | 10 | N/A | N/A | `10.1.252.130` (VLAN 10)* |
| **Borde** | `R-UFINET` | Proveedor Principal | N/A | `8.8.8.8` | `2001:4860:4860::8888` | N/A (Externa) |
| **Borde** | `R-STARLINK` | Proveedor Respaldo | N/A | `1.1.1.1` | `2606:4700:4700::1111` | N/A (Externa) |
| **Agreg** | `SW-AD-RADIO` | Agregador L2 Radio | 99 | N/A (Capa 2) | N/A | `10.1.252.132` (SVI) |
| **Agreg** | `ADMIN-RADIO` | Agregador L3 Radio | 99 | `10.1.252.2` | `2001:DB8:FACE:FE::2` | Acceso por Loopback / Transp. |
| **Agreg** | `AD-FIBRA` | Agregador L3 FTTH | 99 | `10.1.252.3` | `2001:DB8:FACE:FE::3` | Acceso por Loopback / Transp. |
| **Acceso** | `SW-OLT` | Concentrador Fibra L2 | 99 | N/A (Capa 2) | N/A | `10.1.252.133` (SVI) |
| **Malla** | `R1` | Nodo Malla Radio | 99 | `10.1.252.11` | `2001:DB8:FACE:FE::11` | Acceso por Loopback |
| **Malla** | `R2` | Nodo Malla Radio | 99 | `10.1.252.12` | `2001:DB8:FACE:FE::12` | Acceso por Loopback |
| **Malla** | `R3` | Nodo Malla Radio | 99 | `10.1.252.13` | `2001:DB8:FACE:FE::13` | Acceso por Loopback |
| **Malla** | `R4` | Nodo Extensión Radio | 99 | `10.1.252.14` | `2001:DB8:FACE:FE::14` | Acceso por Loopback |
| **Acceso** | `SW1 a SW4` | Switches Finales | 99 | N/A (Capa 2) | N/A | `10.1.252.140` al `.144` (SVI) |

---
**Notas Técnicas:**
* **VLAN 99:** Segmento de Gestión Administrativa (Out-of-Band).
* **Overlap Check:** El gateway `.254` de la subinterfaz `.99` en el Core ha sido validado para evitar conflictos con el rango de Loopbacks.
* **IPv6:** Los prefijos `/128` en Loopbacks garantizan que el Router-ID de OSPFv3 permanezca estable ante cambios en los enlaces físicos.