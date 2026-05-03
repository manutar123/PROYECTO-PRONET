# 📑 Guía de Despliegue y Configuración: AD-FIBRA

## 1. Propósito del Nodo
El router **AD-FIBRA** (basado en el chasis **Cisco ISR 4331**) cumple la función crítica de **Agregador L3** para la red **FTTH** (*Fiber To The Home*). Su rol principal es servir de puente entre el núcleo de la red (**CORE-PRONET**) y el concentrador de acceso de fibra (**SW-OLT**), gestionando el tráfico de servicios residenciales y corporativos.

---

## 2. Metodología de Implementación: *Phased Provisioning*
Para garantizar un despliegue exitoso en **Cisco Packet Tracer**, se ha adoptado una metodología de aprovisionamiento por fases. Esta estrategia divide la configuración en módulos lógicos para:

* **Evitar bloqueos del CLI** durante procesos interactivos (como la generación de llaves RSA).
* **Facilitar la identificación y corrección** de errores de sintaxis de forma granular.
* **Mantener la coherencia** con el sistema de control de versiones en **Git**.

---

## 3. Descripción de los Módulos de Configuración
La configuración se ha estructurado en cinco fases fundamentales, diseñadas para ser aplicadas secuencialmente:

### 🛠️ Fase A: Identidad y Seguridad Criptográfica
Establece la personalidad del equipo dentro de la red **PRONET**. Define el nombre oficial del host, el dominio organizacional y las credenciales de administración de alto privilegio. En este punto se genera el certificado **RSA**, requisito previo indispensable para habilitar el motor de comunicación segura.

### 🛡️ Fase B: Hardening y Gestión Remota
Configura el **"Plano de Control"** del dispositivo. Implementa el protocolo **SSH v2** para administración remota, establece avisos legales mediante *banners* y asegura los tiempos de espera de sesión para evitar accesos no autorizados. Asimismo, define la interfaz **Loopback0**, que actúa como la dirección IP persistente del router y su identificador único para los protocolos de enrutamiento.

### 🔗 Fase C: Conectividad Física y Lógica (Dual-Stack)
Define la arquitectura de capas 1, 2 y 3:
* **Transporte:** Activa los puertos nativos enrutados hacia el CORE.
* **Acceso:** Configura el enlace troncal hacia la OLT mediante subinterfaces, permitiendo la segmentación de servicios para clientes **residenciales (VLAN 60)** y **corporativos (VLAN 100)**.
* **Dual-Stack:** Se aplica direccionamiento **IPv4 e IPv6** para asegurar la compatibilidad con los estándares modernos.

### 🧠 Fase D: Inteligencia de Red (Backbone OSPF)
Activa el "cerebro" del router mediante los protocolos **OSPFv2 y OSPFv3**. Este módulo permite que el AD-FIBRA aprenda y comparta rutas de forma dinámica con el resto de la infraestructura. Se incluye la política de **Interfaces Pasivas**, una mejor práctica de seguridad que evita el envío innecesario de actualizaciones de ruteo hacia las redes de los clientes finales.

### ✅ Fase E: Certificación de Salud y Operatividad
Consiste en una batería de comandos de diagnóstico diseñados para verificar:
1.  El estado de las interfaces físicas y virtuales.
2.  La formación de vecindades (*adjacencies*) con el **CORE**.
3.  La correcta instalación de las rutas en la tabla de enrutamiento global.

---

## 4. Notas del Ingeniero Senior
* **Hardware:** Se han optimizado los comandos para el modelo **ISR 4331**, omitiendo comandos de switching innecesarios en puertos nativos de ruteo.
* **Seguridad:** El acceso administrativo está restringido exclusivamente a **SSH**; el acceso vía Telnet ha sido inhabilitado por diseño.
* **Escalabilidad:** El esquema de subinterfaces utilizado en el enlace hacia la OLT permite expandir servicios a futuro sin necesidad de añadir nuevo cableado físico.