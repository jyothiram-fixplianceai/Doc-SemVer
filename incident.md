# FixplianceAI Multi-Tenant Incident Response System
## Complete Visual Guide: Uptime Kuma → Spike.sh → Team Escalation


##  Service Details & Health Check Endpoints

| Service | Endpoint Pattern | Check Type | Purpose |
|---------|-----------------|------------|---------|
| **Web UI** | `https://{tenant}.hub.fixpliance.ai/auditvault` | HTTP GET | Main application interface |
| **Root** | `https://{tenant}.hub.fixpliance.ai/` | HTTP GET | Base domain health |
| **Audit Vault API** | `https://{tenant}.hub.fixpliance.ai/api/auditvault/v1/health` | HTTP GET | Core audit management |
| **Consumer API** | `https://{tenant}.hub.fixpliance.ai/api/consumer/v1/health` | HTTP GET | Data consumption service |
| **Vigilant API** | `https://{tenant}.hub.fixpliance.ai/api/vigilant/v1/health` | HTTP GET | Monitoring & analysis |
| **Node Monitor API** | `https://{tenant}.hub.fixpliance.ai/api/node-monitor/v1/health` | HTTP GET | Infrastructure monitoring |
| **Scheduler API** | `https://{tenant}.hub.fixpliance.ai/api/scheduler/v1/health` | HTTP GET | Job scheduling service |
| **SonarQube** | `https://{tenant}.hub.fixpliance.ai/sonarqube` | HTTP GET | Code quality analysis |
| **Grafana** | `https://{tenant}.hub.fixpliance.ai/grafana/healthz` | HTTP GET | Metrics visualization |
| **ArgoCD** | `https://{tenant}.hub.fixpliance.ai/argocd/healthz` | HTTP GET | GitOps deployment |

**Tenant Variables:**
- `development` - Development environment
- `dev3` - Testing/staging environment  
- `{clientname}` - Production client instances (e.g., `acme`, `techcorp`, `enterprise`)

---

## 📊 End-to-End Incident Response Flow

```mermaid
graph TB
    subgraph "Service Layer - Multi-Tenant"
        A[Client Infrastructure]
        B[development.hub.fixpliance.ai]
        C[dev3.hub.fixpliance.ai]
        D[client1.hub.fixpliance.ai]
        E[client2.hub.fixpliance.ai]
        A --> B
        A --> C
        A --> D
        A --> E
    end
    
    subgraph "Monitoring Layer - Uptime Kuma"
        F[10 Monitors per Tenant]
        G[60-second intervals]
        H[3 retry attempts]
        I[48-second timeout]
        B --> F
        C --> F
        D --> F
        E --> F
        F --> G
        G --> H
        H --> I
    end
    
    subgraph "Detection & Alerting"
        J[Failure Detected]
        K[Build Webhook Payload]
        L[POST to Spike.sh]
        I --> J
        J --> K
        K --> L
    end
    
    subgraph "Incident Management - Spike.sh"
        M[Create Incident]
        N[Parse Monitor Data]
        O[Apply Escalation Policy]
        L --> M
        M --> N
        N --> O
    end
    
    subgraph "4-Step Escalation"
        P[Step 1: SMS<br/>T+0 min<br/>Jyothi Ram]
        Q[Step 2: Slack<br/>T+1 min<br/>#dev3-alerts]
        R[Step 3: Email<br/>T+2 min<br/>Jyothi Ram]
        S[Step 4: Phone<br/>T+3 min<br/>Jyothi Ram]
        O --> P
        P --> Q
        Q --> R
        R --> S
    end
    
    subgraph "Response & Resolution"
        T{Acknowledged?}
        U[Engineer Investigates]
        V[Fix Deployed]
        W[Service Recovers]
        X[Auto-Resolve Incident]
        S --> T
        T -->|Yes| U
        T -->|No| S
        U --> V
        V --> W
        W --> X
    end
    
    style J fill:#ff6b6b
    style M fill:#ff6b6b
    style P fill:#ffd93d
    style Q fill:#6bcf7f
    style R fill:#95e1d3
    style S fill:#ff6b6b
    style X fill:#6bcf7f
```

## 🔧 Uptime Kuma Webhook Payload

### **Custom Body for Multi-Tenant Monitoring:**

```json
{
  "title": "{{msg}}",
  "tenant": "{{monitorJSON['name']}}",
  "service_name": "{{monitorJSON['description']}}",
  "monitor_url": "{{monitorJSON['url']}}",
  "monitor_type": "{{monitorJSON['type']}}",
  "status_message": "{{heartbeatJSON['msg']}}",
  "response_time": "{{heartbeatJSON['ping']}}",
  "timestamp": "{{heartbeatJSON['time']}}",
  "status_code": "{{heartbeatJSON['status']}}",
  "tags": "{{monitorJSON['tags']}}",
  "interval": "{{monitorJSON['interval']}}",
  "environment": "production"
}
```

## 🔄 Service Recovery Flow

```mermaid
stateDiagram-v2
    [*] --> Monitoring: Normal Operation
    
    Monitoring --> FirstFailure: Health Check Fails
    FirstFailure --> Retry1: Wait 60s
    Retry1 --> Retry2: Fail - Wait 60s
    Retry2 --> Retry3: Fail - Wait 60s
    Retry3 --> IncidentCreated: All Retries Failed
    
    IncidentCreated --> SMSAlert: T+0 min
    SMSAlert --> SlackAlert: No ACK - T+1 min
    SlackAlert --> EmailAlert: No ACK - T+2 min
    EmailAlert --> PhoneAlert: No ACK - T+3 min
    
    PhoneAlert --> Acknowledged: Engineer ACKs
    Acknowledged --> Investigating: Investigation Starts
    
    Investigating --> FixIdentified: Root Cause Found
    FixIdentified --> FixDeployed: Deploy Solution
    
    FixDeployed --> HealthCheckPass: Service Responds
    HealthCheckPass --> AutoResolve: Uptime Kuma Detects UP
    AutoResolve --> Monitoring: Incident Closed
    
    note right of IncidentCreated
        Incident: INC-2025-XXX
        Severity: Critical
        Client: client1
        Service: Audit Vault API
    end note
    
    note right of Acknowledged
        ACK Time: 3m 8s
        Engineer: Jyothi Ram
        Channel: Phone
    end note
    
    note right of AutoResolve
        Resolution Time: 14m 40s
        Method: Service Restart
        Verification: Health check passed
    end note
```


## 📞 Escalation Contacts

### **Primary On-Call**
- **Name:** Jyothi Ram
- **Phone:** [Configured in Spike.sh]
- **Email:** jyothi@fixpliance.ai
- **Slack:** @jyothi

### **Secondary Escalation**
- **Name:** Harish Matheshwaran
- **Email:** harish.m@fixpliance.ai
- **Slack:** @harish

### **Spike.sh Support**
- **Founder:** Kaushik Thirthappa
- **Email:** kaushik@spike.sh
- **Documentation:** https://docs.spike.sh
