# 🖧 Matriz de Conectividad y Transporte Físico (L1, L2 y L3) - PRONET

Esta matriz detalla la interconexión física y lógica de la infraestructura, asegurando la coherencia entre el cableado, las VLANs y el direccionamiento IP.

| Enlace (Origen -- Destino) | Interfaz Origen | Interfaz Destino | Red / VLAN Asignada | Tipo de Enlace | IP / Config Origen | IP / Config Destino |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **CORE -- SW-CORE** | `g0/0/0` | `g1/0/1` | VLANs 10, 99, Transp. | Trunk (802.1Q) | Subinterfaces (.254) | N/A (Switchport Trunk) |
| **CORE -- R-UFINET** | `g0/1/1` | `g0/0` | 10.1.254.128/30 | L3 (Transporte WAN) | 10.1.254.130 | 10.1.254.129 |
| **CORE -- R-STARLINK** | `g0/1/0` | `g0/1` | 10.1.254.132/30 | L3 (Transporte WAN) | 10.1.254.134 | 10.1.254.133 |
| **CORE -- AD-FIBRA** | `g0/0/1` | `g0/0/0` | 10.1.254.4/30 | L3 (Transporte Interno) | 10.1.254.5 | 10.1.254.6 |
| **SW-CORE -- LAPTOP_ADMIN** | `g1/0/10` | `Fa0` | VLAN 99 | Acceso (Gestión) | N/A (Switchport) | 10.1.252.130/25 (PC) |
| **SW-CORE -- SRV-NOC-PRONET** | `g1/0/23, 24` | `g0, g1` | VLAN 10 | Acceso (NOC) | N/A (Switchport) | 10.1.252.130/26 (NIC) |
| **SW-CORE -- SW-AD-RADIO** | `g1/0/21, 22` | `g1/0/21, 22` | Po1 (Todas las VLANs) | Trunk LACP (Active) | N/A (EtherChannel) | N/A (EtherChannel) |
| **SW-AD-RADIO -- ADMIN-RADIO** | `g1/0/1` | `g0/1/0` | VLAN Transporte* | Acceso / Ruteado | N/A (Pasa tráfico L2) | 10.1.254.2/30 (L3 a Core) |
| **ADMIN-RADIO -- R1** | `g0/0/0` | `g0/0/0` | 10.1.254.8/30 | L3 (Transporte PtP) | 10.1.254.9 | 10.1.254.10 |
| **ADMIN-RADIO -- R3** | `g0/0/1` | `g0/0/0` | 10.1.254.12/30 | L3 (Transporte PtP) | 10.1.254.13 | 10.1.254.14 |
| **R1 -- R2** | `g0/1/1` | `g0/0/0` | 10.1.254.16/30 | L3 (Transporte PtP) | 10.1.254.17 | 10.1.254.18 |
| **R1 -- R4** | `g0/1/0` | `g0/0/0` | 10.1.254.24/30 | L3 (Transporte PtP) | 10.1.254.25 | 10.1.254.26 |
| **R2 -- R3** | `g0/1/0` | `g0/0/1` | 10.1.254.20/30 | L3 (Transporte PtP) | 10.1.254.21 | 10.1.254.22 |
| **R1 -- SW1** (Nodos 3 y 4 igual) | `g0/0/1` | `g0/1` | VLAN 50 (Residencial) | Trunk (802.1Q) | Subint / Gateway | N/A (Switchport Trunk) |
| **AD-FIBRA -- SW-OLT** | `g0/0/1` | `g1/0/1` | VLAN 60, 100 | Trunk (802.1Q) | Subint / Gateway | N/A (Switchport Trunk) |
| **SW-OLT -- CLI-FIBRA** | `g1/0/2` | `g0/1` | VLAN 60 | Trunk / Acceso | N/A (Switchport) | N/A (Switchport) |
| **SW-OLT -- RCORP-B** | `g1/0/3` | `g0/0/0` | VLAN 100 (Corp Fibra) | Acceso | N/A (Switchport) | IP Pública / Privada |
| **SW-OLT -- RCORP-A-SUC** | `g1/0/4` | `g0/0/0` | VLAN 100 (Corp Fibra) | Acceso | N/A (Switchport) | IP Pública / Privada |
| **R2 -- RCORP-A** (Principal) | `g0/0/1` | `g0/0/0` | VLAN 200 (Corp Radio) | Acceso / Subint | Subint / Gateway | IP Pública / Privada |

---

### 📝 Notas Adicionales
* **ADMIN-RADIO a CORE:** La conectividad lógica Punto a Punto utiliza la red `10.1.254.0/30`. El tráfico atraviesa de forma transparente la infraestructura de switches mediante una VLAN de tránsito dedicada.
* **LACP (EtherChannel):** El enlace entre `SW-CORE` y `SW-AD-RADIO` utiliza agregación de enlaces para mayor ancho de banda y redundancia.
* **Nomenclatura:** Las interfaces cumplen con los estándares de hardware para los modelos **IR8340**, **ISR 4331** y **Catalyst 3650** utilizados en el simulador.