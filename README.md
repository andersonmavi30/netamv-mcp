# 🌐 netamv_mcp

🇨🇴 [Español](README.es.md)

**Multi-vendor MCP server for NetDevOps, Network Automation and AI-assisted network operations.**

`netamv-mcp` is an open-source project designed to expose network infrastructure capabilities through the **Model Context Protocol (MCP)**, allowing MCP-compatible clients and AI agents to interact with multi-vendor network environments through a controlled and extensible tool layer.

The project is intended to bridge traditional network operations, Python automation and emerging agentic workflows without coupling the network automation logic to a single AI model, vendor or transport.

---

## 🎯 Project Objective

The goal of `netamv-mcp` is to provide a reusable MCP server for common **network administration, operations, troubleshooting and automation** workflows.

The project is being designed around capabilities such as:

- Device discovery and inventory
- Device facts and platform information
- Interface state and counters
- VLAN information
- ARP and MAC tables
- LLDP/CDP neighbors
- Routing information
- CPU and memory information
- Configuration backups
- Operational troubleshooting
- Connectivity validation
- Pre-check and post-check workflows
- Controlled configuration changes
- Validation and reporting
- Multi-device execution
- Audit-friendly execution records

The long-term objective is to provide a consistent MCP interface while preserving the differences and operational requirements of each network operating system.

---

## 🧠 What MCP Adds

The **Model Context Protocol** acts as the interface between an MCP-compatible client and the automation capabilities exposed by this server.

```text
MCP Client / AI Agent
        │
        │ Model Context Protocol
        ▼
   ┌───────────────┐
   │   netamv-mcp  │
   └───────────────┘
        │
        ├── Discovery / Facts
        ├── Operations
        ├── Troubleshooting
        ├── Validation
        ├── Backups
        └── Controlled Changes
        │
        ▼
 Multi-vendor Network Infrastructure
```

The MCP server is **not the AI model itself**. It exposes tools and structured capabilities that can be consumed by an MCP client, automation workflow or AI agent.

This separation allows the same network automation backend to be reused with different clients or models.

---

## 🌐 Planned Vendor Coverage

The initial multi-vendor roadmap is aligned with common enterprise and NetDevOps platforms:

| Vendor / Platform | Planned Scope |
|---|---|
| Cisco IOS / IOS-XE / NX-OS / IOS-XR | Network operations and automation |
| Arista EOS | Network operations and automation |
| Juniper Junos | Network operations and automation |
| Aruba AOS-CX | Network operations and automation |
| Huawei VRP | Network operations and automation |
| Nokia SR OS | Network operations and automation |
| Dell OS6 / OS9 / OS10 / Enterprise SONiC | Legacy and current Dell networking |
| SONiC | Open networking automation |
| NVIDIA Cumulus Linux / NVUE | Linux-based network automation |
| VyOS | Routing and network automation |

> Vendor support will be implemented progressively. A platform will only be marked as implemented after its tools and connection method are actually available in the repository.

---

## 🧩 Planned Capability Model

The project will separate MCP tools by operational intent instead of exposing one large generic command executor.

### 🔍 Read / Observe

Examples:

- Get device facts
- Get interface status
- Get interface errors
- Get VLANs
- Get ARP table
- Get MAC table
- Get neighbors
- Get routing table
- Get CPU / memory
- Get running configuration

### 🛠️ Operate / Troubleshoot

Examples:

- Ping validation
- Route lookup
- Interface inspection
- Neighbor validation
- Troubleshooting command collections
- Multi-device health checks
- Configuration backup

### ⚙️ Controlled Changes

Future change-capable tools are intended to include safeguards such as:

- Explicit change intent
- Input validation
- Dry-run / preview where supported
- Pre-checks
- Human approval where appropriate
- Post-checks
- Audit logging
- Rollback strategy where supported

---

## 🔒 Safety and Change Control

The project will follow a **read-first, change-controlled** approach.

The initial implementation priority is expected to be read-only and operational tooling before configuration-changing actions are introduced.

Planned principles:

- Read-only tools first
- No hard-coded production credentials
- Explicit separation between read and write operations
- Input validation
- Least-privilege access
- Secrets provided externally
- Human-in-the-loop approval for sensitive changes
- Auditability of executed actions
- Lab validation before production use

> MCP connectivity does not remove normal network change-management requirements.

---

## 🏗️ Planned Architecture

```text
                    ┌─────────────────────────┐
                    │ MCP Client / AI Agent   │
                    └────────────┬────────────┘
                                 │ MCP
                                 ▼
                    ┌─────────────────────────┐
                    │       netamv-mcp        │
                    │                         │
                    │  Tools / Validation     │
                    │  Vendor Abstraction     │
                    │  Safety / Audit Layer   │
                    └────────────┬────────────┘
                                 │
              ┌──────────────────┼──────────────────┐
              │                  │                  │
              ▼                  ▼                  ▼
          SSH / CLI         NETCONF / APIs        gNMI
              │                  │                  │
              └──────────────────┼──────────────────┘
                                 ▼
                    Multi-vendor Network Devices
```

The exact transport will depend on the target platform. The project will not force every vendor through the same protocol when a more appropriate native interface exists.

---

## 🐍 Planned Technical Stack

The implementation is expected to evolve around technologies such as:

- Python
- Model Context Protocol (MCP)
- FastMCP
- Netmiko
- NAPALM where appropriate
- Nornir
- Vendor APIs / SDKs
- NETCONF
- RESTCONF
- REST APIs
- gNMI where appropriate
- JSON / YAML
- Docker

Not every dependency will be required for every vendor.

---

## 📂 Planned Repository Structure

The repository is currently in its initial design phase. A possible structure is:

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

The final structure may evolve as the first MCP tools are implemented.

---

## 🗺️ Development Roadmap

### Phase 1 — Foundation

- MCP server bootstrap
- Project packaging
- Configuration model
- Credential handling strategy
- Logging
- Initial read-only tool contract

### Phase 2 — First Vendor Adapter

- Device connection
- Facts
- Interfaces
- Routing
- Neighbors
- Health checks
- Configuration backup

### Phase 3 — Multi-vendor Abstraction

- Additional vendor adapters
- Shared input/output models
- Standardized operational results
- Vendor-specific exceptions

### Phase 4 — Controlled Automation

- Pre-checks
- Change preview
- Approved configuration actions
- Post-checks
- Rollback patterns
- Audit records

### Phase 5 — Agentic NetDevOps

- Integration with MCP-compatible agents
- Human-in-the-loop workflows
- Multi-step troubleshooting
- Validation-driven remediation
- Reusable NetDevOps workflows

---

## 🤖 AI Is Optional

`netamv-mcp` is designed so the network automation layer can exist independently from a specific AI provider.

Possible consumers include:

- MCP-compatible desktop clients
- AI agents
- Local / open-weight models
- Cloud models
- Custom NetDevOps applications
- Automation workflows

The intelligence layer can change while the MCP tools and network integrations remain reusable.

---

## 📊 Project Status

> 🚧 **Early Development / Architecture Phase**

Current repository state:

- ✅ Repository created
- ✅ MIT License
- ✅ Initial project scope defined
- 🚧 MCP server implementation pending
- 🚧 First tools pending
- 🚧 Vendor adapters pending
- 🚧 Tests and container image pending

The README will be updated as capabilities move from planned to implemented.

---

## 🧪 Lab First

This project is intended to be developed and validated first against **laboratory and controlled network environments**.

Never provide production credentials directly in source code.

Production usage should include:

- External secret management
- Least-privilege accounts
- Logging and audit controls
- Change approval
- Tested rollback procedures

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

## 👨‍💻 Author

**Anderson Martinez Virviescas**

Network Administrator | Firewall Administrator | Network Automation | NetDevOps | Linux | Infrastructure Automation

GitHub: [@andersonmavi30](https://github.com/andersonmavi30)

---

> Connect the tools. Understand the network. Automate with context.
