# Documentación de Configuración: R-UFINET (V1.2)

Este repositorio contiene la configuración base y de seguridad para el router de borde **R-UFINET** del proyecto **PRONET**. El objetivo es establecer un entorno de red Dual-Stack (IPv4/IPv6) seguro y funcional para la simulación de servicios ISP.

---

## 🛠️ Desglose Técnico de Comandos

### 1. Gestión y Seguridad del Dispositivo (Control Plane)

* **`hostname R-UFINET`**: Establece la identidad del nodo.
* **`service password-encryption`**: Cifra las contraseñas en el archivo de configuración para evitar la exposición de datos sensibles en texto plano.
* **`username admin privilege 15 secret PRONET_2026`**: Crea un usuario con privilegios totales utilizando un algoritmo de hash fuerte (SHA/MD5).
* **`crypto key generate rsa general-keys modulus 2048`**: Genera las llaves criptográficas necesarias para el cifrado de datos. Se utiliza un módulo de 2048 bits para cumplir con los estándares de seguridad modernos.
* **`ip ssh version 2`**: Habilita la versión 2 del protocolo SSH, deshabilitando la versión 1 que es vulnerable.
* **`ipv6 unicast-routing`**: **Comando crítico.** Habilita al router para procesar y reenviar paquetes IPv6.

---

### 2. Configuración de Interfaces (Data Plane)

#### **Loopback0: Simulación de Servicios**
* **Descripción**: Interfaz virtual lógica que simula el DNS de Google.
* **Propósito**: Proporcionar un destino de prueba siempre activo (`up/up`) que no dependa de fallos físicos en el cableado.
* **Direccionamiento**: `8.8.8.8/32` y `2001:4860:4860::8888/128`.

#### **GigabitEthernet0/0: Transporte WAN**
* **Descripción**: Enlace de salida hacia el Core de la red PRONET.
* **Direccionamiento IPv4**: `200.10.20.14/28`. La máscara `/28` permite un segmento de 14 IPs útiles para futuras expansiones de dispositivos de borde.
* **Direccionamiento IPv6**: `2001:DB8:FACE:FFFF::1/64`. Establece el primer salto para la red IPv6.

#### **Hardening (Interfaces 0/1 y 0/2)**
* Se aplica la política `ADMIN_SHUTDOWN_SECURITY_POLICY`. Los puertos no utilizados se apagan administrativamente (`shutdown`) y se limpian de direcciones IP para mitigar vectores de ataque físicos.

---

### 3. Acceso Remoto Seguro

* **`line vty 0 4`**: Configura las terminales virtuales para acceso remoto.
* **`transport input ssh`**: Restringe el acceso únicamente a SSH, bloqueando Telnet (puerto 23).
* **`login local`**: Obliga al sistema a validar las credenciales contra la base de datos local definida previamente.

---

## 📈 Resultados Esperados (Validación)

Al ejecutar los comandos de verificación (`show`), los resultados técnicos deben coincidir con los siguientes criterios:

### `show ip interface brief`
| Interfaz | Dirección IP | Status | Protocol |
| :--- | :--- | :--- | :--- |
| **GigabitEthernet0/0** | 200.10.20.14 | up | up |
| **GigabitEthernet0/1** | unassigned | administratively down | down |
| **GigabitEthernet0/2** | unassigned | administratively down | down |
| **Loopback0** | 8.8.8.8 | up | up |

### `show ipv6 interface brief`
Se debe observar que tanto la **GigabitEthernet0/0** como la **Loopback0** tienen asignadas sus direcciones globales y una dirección *Link-Local* (iniciando en `FE80::`) generada automáticamente. Ambas deben marcar el estado `up/up`.

### `show cdp neighbors`
Debe mostrar al menos un dispositivo vecino conectado a la interfaz **GigabitEthernet0/0**. En nuestro entorno, el resultado esperado es un router (ej. plataforma IR8300) que confirma la interconexión física con el transporte.

### `show ip route connected`
La tabla de enrutamiento debe mostrar exclusivamente las redes que el router conoce por conexión directa:
* `8.8.8.8/32 is directly connected, Loopback0`
* `200.10.20.0/28 is directly connected, GigabitEthernet0/0`

---

> [!IMPORTANT]  
> **Nota:** Si al ejecutar `show ip interface brief` el protocolo de la **Gi0/0** aparece como `down`, verifique la conexión física o la configuración de la interfaz en el extremo opuesto (Core).