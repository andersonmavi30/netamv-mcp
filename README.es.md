# 🌐 netamv-mcp

🇺🇸 [English](README.md)

**Servidor MCP multi-vendor para NetDevOps, Network Automation y operaciones de red asistidas por IA.**

`netamv-mcp` es un proyecto open source diseñado para exponer capacidades de infraestructura de red mediante el **Model Context Protocol (MCP)**, permitiendo que clientes compatibles con MCP y agentes de IA interactúen con entornos de red multi-vendor a través de una capa de herramientas controlada y extensible.

El proyecto busca conectar la operación tradicional de redes, la automatización con Python y los nuevos workflows agentic sin acoplar la lógica de automatización a un único modelo de IA, fabricante o protocolo de transporte.

---

## 🎯 Objetivo del proyecto

El objetivo de `netamv-mcp` es proporcionar un servidor MCP reutilizable para tareas comunes de **administración, operación, troubleshooting y automatización de redes**.

El proyecto está siendo diseñado alrededor de capacidades como:

- Descubrimiento e inventario de dispositivos
- Facts e información de plataforma
- Estado y contadores de interfaces
- Información de VLANs
- Tablas ARP y MAC
- Vecinos LLDP/CDP
- Información de routing
- CPU y memoria
- Backups de configuración
- Troubleshooting operativo
- Validación de conectividad
- Flujos de pre-check y post-check
- Cambios controlados de configuración
- Validación y reportes
- Ejecución multi-dispositivo
- Registros de ejecución orientados a auditoría

La meta a largo plazo es proporcionar una interfaz MCP consistente preservando las diferencias y requerimientos operativos de cada sistema operativo de red.

---

## 🧠 Qué aporta MCP

El **Model Context Protocol** funciona como la interfaz entre un cliente compatible con MCP y las capacidades de automatización expuestas por este servidor.

```text
Cliente MCP / Agente de IA
        │
        │ Model Context Protocol
        ▼
   ┌───────────────┐
   │   netamv-mcp  │
   └───────────────┘
        │
        ├── Discovery / Facts
        ├── Operación
        ├── Troubleshooting
        ├── Validación
        ├── Backups
        └── Cambios controlados
        │
        ▼
 Infraestructura de red multi-vendor
```

El servidor MCP **no es la IA como tal**. Expone herramientas y capacidades estructuradas que pueden ser consumidas por un cliente MCP, un workflow de automatización o un agente de IA.

Esta separación permite reutilizar el mismo backend de automatización con diferentes clientes o modelos.

---

## 🌐 Cobertura de fabricantes planificada

El roadmap inicial multi-vendor está alineado con plataformas comunes de entornos enterprise y NetDevOps:

| Fabricante / Plataforma | Alcance planificado |
|---|---|
| Cisco IOS / IOS-XE / NX-OS / IOS-XR | Operación y automatización de red |
| Arista EOS | Operación y automatización de red |
| Juniper Junos | Operación y automatización de red |
| Aruba AOS-CX | Operación y automatización de red |
| Huawei VRP | Operación y automatización de red |
| Nokia SR OS | Operación y automatización de red |
| Dell OS6 / OS9 / OS10 / Enterprise SONiC | Networking Dell legacy y actual |
| SONiC | Automatización de open networking |
| NVIDIA Cumulus Linux / NVUE | Automatización de red basada en Linux |
| VyOS | Routing y automatización de red |

> El soporte se implementará progresivamente. Una plataforma solo se marcará como implementada cuando sus tools y método de conexión existan realmente en el repositorio.

---

## 🧩 Modelo de capacidades planificado

El proyecto separará las tools MCP por intención operativa en lugar de exponer un único ejecutor genérico de comandos.

### 🔍 Lectura / Observación

Ejemplos:

- Obtener facts
- Consultar interfaces
- Consultar errores
- Obtener VLANs
- Obtener tabla ARP
- Obtener tabla MAC
- Obtener vecinos
- Consultar routing
- Consultar CPU / memoria
- Obtener running configuration

### 🛠️ Operación / Troubleshooting

Ejemplos:

- Validación mediante ping
- Route lookup
- Inspección de interfaces
- Validación de vecinos
- Recolección de comandos de troubleshooting
- Health checks multi-dispositivo
- Backup de configuración

### ⚙️ Cambios controlados

Las futuras tools capaces de modificar configuración están pensadas con protecciones como:

- Intención de cambio explícita
- Validación de inputs
- Dry-run / preview donde exista soporte
- Pre-checks
- Aprobación humana donde corresponda
- Post-checks
- Audit logging
- Estrategia de rollback donde exista soporte

---

## 🔒 Seguridad y control de cambios

El proyecto seguirá un enfoque **read-first y change-controlled**.

La prioridad inicial será implementar tools de lectura y operación antes de introducir acciones que modifiquen configuración.

Principios planificados:

- Tools read-only primero
- Sin credenciales productivas hard-coded
- Separación explícita entre operaciones de lectura y escritura
- Validación de inputs
- Acceso de mínimo privilegio
- Secretos suministrados externamente
- Human-in-the-loop para cambios sensibles
- Auditoría de acciones ejecutadas
- Validación en laboratorio antes de producción

> Utilizar MCP no elimina los requerimientos normales de change management de una red.

---

## 🏗️ Arquitectura planificada

```text
                    ┌─────────────────────────┐
                    │ Cliente MCP / Agente IA │
                    └────────────┬────────────┘
                                 │ MCP
                                 ▼
                    ┌─────────────────────────┐
                    │       netamv-mcp        │
                    │                         │
                    │ Tools / Validación      │
                    │ Abstracción de vendors  │
                    │ Seguridad / Auditoría   │
                    └────────────┬────────────┘
                                 │
              ┌──────────────────┼──────────────────┐
              │                  │                  │
              ▼                  ▼                  ▼
          SSH / CLI         NETCONF / APIs        gNMI
              │                  │                  │
              └──────────────────┼──────────────────┘
                                 ▼
                   Dispositivos de red multi-vendor
```

El transporte exacto dependerá de la plataforma. El proyecto no forzará todos los fabricantes a utilizar el mismo protocolo si existe una interfaz nativa más apropiada.

---

## 🐍 Stack técnico planificado

La implementación está pensada para evolucionar alrededor de tecnologías como:

- Python
- Model Context Protocol (MCP)
- FastMCP
- Netmiko
- NAPALM donde aplique
- Nornir
- APIs / SDKs de fabricantes
- NETCONF
- RESTCONF
- REST APIs
- gNMI donde aplique
- JSON / YAML
- Docker

No todas las dependencias serán necesarias para todos los fabricantes.

---

## 📂 Estructura planificada del repositorio

Actualmente el proyecto está en su fase inicial de diseño. Una posible estructura es:

```text
netamv-mcp/
│
├── src/
│   └── netamv_mcp/
│       ├── server.py
│       ├── tools/
│       ├── vendors/
│       ├── models/
│       ├── validation/
│       └── security/
│
├── tests/
├── docs/
├── examples/
├── Dockerfile
├── pyproject.toml
├── README.md
├── README.es.md
└── LICENSE
```

La estructura final podrá evolucionar a medida que se implementen las primeras tools MCP.

---

## 🗺️ Roadmap de desarrollo

### Fase 1 — Base

- Bootstrap del servidor MCP
- Empaquetado del proyecto
- Modelo de configuración
- Estrategia de manejo de credenciales
- Logging
- Contrato inicial de tools read-only

### Fase 2 — Primer adapter de fabricante

- Conexión con dispositivos
- Facts
- Interfaces
- Routing
- Vecinos
- Health checks
- Backup de configuración

### Fase 3 — Abstracción multi-vendor

- Nuevos adapters
- Modelos compartidos de entrada/salida
- Resultados operativos estandarizados
- Excepciones específicas por fabricante

### Fase 4 — Automatización controlada

- Pre-checks
- Preview de cambios
- Acciones de configuración aprobadas
- Post-checks
- Patrones de rollback
- Registros de auditoría

### Fase 5 — Agentic NetDevOps

- Integración con agentes compatibles con MCP
- Workflows human-in-the-loop
- Troubleshooting multi-paso
- Remediación basada en validación
- Workflows NetDevOps reutilizables

---

## 🤖 La IA es opcional

`netamv-mcp` está diseñado para que la capa de automatización de red pueda existir de forma independiente de un proveedor de IA específico.

Posibles consumidores:

- Clientes desktop compatibles con MCP
- Agentes de IA
- Modelos locales / open-weight
- Modelos cloud
- Aplicaciones NetDevOps personalizadas
- Workflows de automatización

La capa de inteligencia puede cambiar mientras las tools MCP y las integraciones de red siguen siendo reutilizables.

---

## 📊 Estado del proyecto

> 🚧 **Desarrollo inicial / Fase de arquitectura**

Estado actual del repositorio:

- ✅ Repositorio creado
- ✅ Licencia MIT
- ✅ Alcance inicial definido
- 🚧 Implementación del servidor MCP pendiente
- 🚧 Primeras tools pendientes
- 🚧 Adapters de fabricantes pendientes
- 🚧 Tests e imagen de contenedor pendientes

El README se actualizará a medida que las capacidades pasen de planificadas a implementadas.

---

## 🧪 Primero laboratorio

Este proyecto está pensado para desarrollarse y validarse inicialmente sobre **laboratorios y entornos de red controlados**.

Nunca almacenes credenciales productivas directamente dentro del código fuente.

Para uso productivo se deberían contemplar:

- Gestión externa de secretos
- Cuentas de mínimo privilegio
- Logging y controles de auditoría
- Aprobación de cambios
- Procedimientos de rollback probados

---

## 📄 Licencia

Este proyecto está licenciado bajo la [Licencia MIT](LICENSE).

## 👨‍💻 Autor

**Anderson Martinez Virviescas**

Network Administrator | Firewall Administrator | Network Automation | NetDevOps | Linux | Infrastructure Automation

GitHub: [@andersonmavi30](https://github.com/andersonmavi30)

---

> Conecta las herramientas. Entiende la red. Automatiza con contexto.
