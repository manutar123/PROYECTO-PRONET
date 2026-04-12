# 📡 Plan Maestro de Direccionamiento - ISP PRONET

## 1. Segmentación Base IPv4 (10.1.0.0/16)
| Sección | Uso / Descripción | Red / Máscara | Gateway | Broadcast |
| :--- | :--- | :--- | :--- | :--- |
| **FIBRA** | Clientes FTTH (Total) | 10.1.0.0/18 | 10.1.0.1 | 10.1.63.255 |
| **RADIO** | Clientes WISP (Total) | 10.1.64.0/18 | 10.1.64.1 | 10.1.127.255 |
| **TRANSPORTE** | Enlaces Punto a Punto | 10.1.254.0/23 | 10.1.254.1 | 10.1.255.255 |
| **ADMIN** | Gestión y Loopbacks | 10.1.252.0/23 | 10.1.252.1 | 10.1.253.255 |

## 2. Subsegmentación de Servicios (VLANs)
| VLAN | Servicio | Segmento IPv4 | Prefijo IPv6 (/64) |
| :--- | :--- | :--- | :--- |
| **100** | Corporativos Fibra | 10.1.0.0/22 | 2001:DB8:FACE:10::/64 |
| **60** | Residenciales Fibra | 10.1.4.0/18 | 2001:DB8:FACE:60::/64 |
| **200** | Corporativo Radio | 10.1.64.0/22 | 2001:DB8:FACE:20::/64 |
| **50** | Residencial Radio | 10.1.68.0/18 | 2001:DB8:FACE:50::/64 |
| **99** | Gestión Admin | 10.1.252.0/24 | 2001:DB8:FACE:99::/64 |

## 3. Salida a Internet (UFINET WAN)
- **IPv4 WAN:** 200.10.20.13/28 (Gateway: 200.10.20.14)
- **IPv6 WAN:** 2001:DB8:FACE:FFFF::2/64 (Gateway: 2001:DB8:FACE:FFFF::1)
- **Pool NAT Residencial:** 200.10.20.4 al 200.10.20.12 (PAT)

## 4. Infraestructura (Loopbacks)
| Router | IPv4 LB0 | IPv6 LB0 (/128) |
| :--- | :--- | :--- |
| CORE-PRONET | 10.1.252.1 | 2001:DB8:FACE:FE::1 |
| ADMIN-RADIO | 10.1.252.2 | 2001:DB8:FACE:FE::2 |
| AD-FIBRA | 10.1.252.3 | 2001:DB8:FACE:FE::3 |