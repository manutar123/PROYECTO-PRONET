##  Evolución del Proyecto y Estrategia de Versionamiento (Roadmap)

### Ajuste Metodológico: Transición a CLI (Versión 1)
En la fase actual del proyecto PRONET, se ha ejecutado una modificación metodológica estratégica: la transición del uso inicial de scripts automatizados (Python/Netmiko) a una implementación basada exclusivamente en **Líneas de Comando Puras (CLI)**. Esta decisión nos alinea de manera realista con el flujo de trabajo y las capacidades nativas del simulador desplegado en esta primera etapa.

### ⚠️ Justificación Técnica y Limitantes de Software
El ajuste metodológico obedece a limitaciones técnicas insalvables detectadas durante el despliegue del entorno de simulación:

* **Incompatibilidad de Motores de Captura:** La versión 9.0.0 de Cisco Packet Tracer ha descontinuado o bloqueado por diseño el soporte nativo para adaptadores de red virtuales (Loopbacks de Microsoft) mediante WinPcap/Npcap en sistemas operativos Windows 10/11.
* **Bloqueo de Interfaz (PT-Cloud):** A raíz de las restricciones de seguridad y arquitectura mencionadas, la herramienta de interconexión (PT-Cloud) es incapaz de actuar como puente de comunicación entre el sistema operativo host (donde se ejecutan los scripts de Python) y los routers simulados en la topología.
* **Priorización de Avance:** Para evitar el estancamiento del despliegue tratando de forzar una herramienta de naturaleza académica a ejecutar funciones avanzadas de DevNet actualmente bloqueadas, se optó por la configuración y validación manual de los equipos.

### 🗺️ Estrategia de Implementación por Versiones
La adopción de un modelo de entrega iterativo es una decisión de ingeniería que nos permite sortear obstáculos técnicos sin comprometer las metas operativas y de escalabilidad del ISP. El ciclo de vida de PRONET se estructura de la siguiente manera:

#### Versión 1 (V1) - Funcionalidad y Diseño Base (Fase Actual)
* **Entorno:** Cisco Packet Tracer 9.0.0.
* **Enfoque:** Validar de forma exhaustiva el diseño lógico, la asignación del plan de direccionamiento IPv4/IPv6 y el levantamiento del enrutamiento jerárquico (OSPF, BGP, NAT) utilizando CLI.
* **Objetivo Estratégico:** Garantizar que el diseño topológico subyacente del ISP es robusto, funcional y está listo para producción a nivel lógico.

#### Versión 2 (V2) - Infraestructura como Código (IaC) (Próxima Fase)
* **Entorno:** Emuladores de grado profesional (GNS3, EVE-NG o Containerlab).
* **Enfoque:** Migrar la topología previamente estabilizada y validada en la V1 para implementar la automatización total del aprovisionamiento mediante scripts en Python (Netmiko/NAPALM) y herramientas de gestión como Ansible.
* **Objetivo Estratégico:** Escalar PRONET a un nivel de operación realista de Service Provider, aplicando principios modernos de NetDevOps y programabilidad de redes.