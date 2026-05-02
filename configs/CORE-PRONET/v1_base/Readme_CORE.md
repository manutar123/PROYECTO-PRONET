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

```bash
enable
configure terminal
hostname CORE-PRONET
ip domain-name pronet.com
no ip domain-lookup
username admin privilege 15 secret PRONET_2026
crypto key generate rsa