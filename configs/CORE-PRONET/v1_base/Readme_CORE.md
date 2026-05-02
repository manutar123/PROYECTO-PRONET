# 🚀 CORE-PRONET: Configuración Base (v1_base)

Este directorio contiene la configuración fundacional del enrutador principal **CORE-PRONET** (ISR 4331). 

## 📌 Propósito de la Configuración Modular
Debido a las limitaciones inherentes del simulador Cisco Packet Tracer (como el bloqueo de comandos en bloque por pasos interactivos y bugs en la asignación de puertos en módulos de expansión), esta configuración ha sido segmentada en **5 archivos modulares**. 

Este método, conocido como *Phased Provisioning* (Aprovisionamiento por fases), garantiza un despliegue libre de errores de sintaxis y permite aislar problemas rápidamente.

---

## 📂 Estructura de Archivos y Orden de Ejecución

Para desplegar el CORE con éxito, los siguientes scripts deben ser copiados y pegados en la terminal del router estrictamente en este orden:

### 1️⃣ Identidad y Criptografía (`Identidad_ssh.cfg`)
Configura el nombre del equipo, dominio, usuario administrador y prepara el terreno para el acceso remoto.
> **⚠️ IMPORTANTE:** El último comando (`crypto key generate rsa`) es interactivo. Al pegarlo, el router pausará. Debes escribir **`2048`** y presionar **ENTER** para continuar.

2️⃣ Gestión y Plano de Control (Gestion.cfg)
Asegura los puertos de consola y VTY mediante SSHv2, establece los mensajes de advertencia (Banner) y configura la interfaz Loopback que servirá como Router-ID en OSPF.

3️⃣ Enrutamiento Interno - Puertos Capa 3 (Puertos_L3.cfg)
Despliega el direccionamiento IP nativo (Dual-Stack IPv4/IPv6) hacia el interior de la red. Incluye el enlace hacia AD_FIBRA y las subinterfaces troncales hacia el SW_CORE (VLAN 10 NOC y VLAN 99 Gestión).

4️⃣ Salida WAN - Bypass y SVIs (SVis.cfg)
Implementa un bypass técnico para evadir el bug de Capa 2 en los módulos de expansión de Packet Tracer. Obliga la creación de las VLANs de tránsito WAN (130 y 134) asignándolas directamente a los puertos, levanta las SVIs como Gateways y configura las Rutas Estáticas Flotantes hacia Internet.

5️⃣ Pruebas y Validación (Validacion.cfg)
Batería de comandos estandarizados para certificar la operatividad total de la Capa 1 a la Capa 3, asegurando que los enlaces levantaron y las tablas de enrutamiento instalaron las métricas correctas.

