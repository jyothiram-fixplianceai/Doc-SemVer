# FixplianceAI Multi-Tenant Incident Response System
## Complete Visual Guide: Uptime Kuma → Spike.sh → Team Escalation


## 📋 Complete Service Inventory

### **Services Monitored Across All Tenants**

```mermaid
graph LR
    subgraph "Frontend Services"
        A1[Web UI<br/>/auditvault]
        A2[Root<br/>/]
    end
    
    subgraph "Backend APIs"
        B1[Audit Vault API<br/>/api/auditvault/v1/health]
        B2[Consumer API<br/>/api/consumer/v1/health]
        B3[Vigilant API<br/>/api/vigilant/v1/health]
        B4[Node Monitor API<br/>/api/node-monitor/v1/health]
        B5[Scheduler API<br/>/api/scheduler/v1/health]
    end
    
    subgraph "DevOps Tools"
        C1[SonarQube<br/>/sonarqube]
        C2[Grafana<br/>/grafana/healthz]
        C3[ArgoCD<br/>/argocd/healthz]
    end
    
    subgraph "Tenant Instances"
        D1[development.hub.fixpliance.ai]
        D2[dev3.hub.fixpliance.ai]
        D3[clientname.hub.fixpliance.ai]
    end
    
    D1 --> A1
    D1 --> A2
    D1 --> B1
    D1 --> B2
    D1 --> B3
    D1 --> B4
    D1 --> B5
    D1 --> C1
    D1 --> C2
    D1 --> C3
    
    D2 --> A1
    D2 --> A2
    D2 --> B1
    D2 --> B2
    D2 --> B3
    D2 --> B4
    D2 --> B5
    
    D3 --> A1
    D3 --> A2
    D3 --> B1
    D3 --> B2
    D3 --> B3
    D3 --> B4
    D3 --> B5
    
    style A1 fill:#95e1d3
    style A2 fill:#95e1d3
    style B1 fill:#6bcf7f
    style B2 fill:#6bcf7f
    style B3 fill:#6bcf7f
    style B4 fill:#6bcf7f
    style B5 fill:#6bcf7f
    style C1 fill:#ffd93d
    style C2 fill:#ffd93d
    style C3 fill:#ffd93d
    style D1 fill:#4ecdc4
    style D2 fill:#4ecdc4
    style D3 fill:#ff6b6b
```

---

## 🎯 Service Details & Health Check Endpoints

| Service | Endpoint Pattern | Check Type | Purpose |
|---------|-----------------|------------|---------|
| **Web UI** | `https://{tenant}.hub.fixpliance.ai/auditvault` | HTTP GET | Main application interface |
| **Root** | `https://{tenant}.hub.fixpliance.ai/` | HTTP GET | Base domain health |
| **Audit Vault API** | `https://{tenant}.hub.fixpliance.ai/api/auditvault/v1/health` | HTTP GET | Core audit management |
| **Consumer API** | `https://{tenant}.hub.fixpliance.ai/api/consumer/v1/health` | HTTP GET | Data consumption service |
| **Vigilant API** | `https://{tenant}.hub.fixpliance.ai/api/vigilant/v1/health` | HTTP GET | Monitoring & analysis |
| **Node Monitor API** | `https://{tenant}.hub.fixpliance.ai/api/node-monitor/v1/health` | HTTP GET | Infrastructure monitoring |
| **Scheduler API** | `https://{tenant}.hub.fixpliance.ai/api/scheduler/v1/health` | HTTP GET | Job scheduling service |
| **SonarQube** | `https://development.hub.fixpliance.ai/sonarqube` | HTTP GET | Code quality analysis |
| **Grafana** | `https://development.hub.fixpliance.ai/grafana/healthz` | HTTP GET | Metrics visualization |
| **ArgoCD** | `https://development.hub.fixpliance.ai/argocd/healthz` | HTTP GET | GitOps deployment |

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

---

## 🔍 Detailed Incident Flow with Real Endpoints

```mermaid
sequenceDiagram
    participant Tenant as client1.hub.fixpliance.ai
    participant UK as Uptime Kuma
    participant Spike as Spike.sh
    participant SMS as SMS
    participant Slack as Slack #dev3-alerts
    participant Email as Email
    participant Phone as Phone Call
    participant Eng as Jyothi Ram
    
    Note over Tenant,UK: Continuous Monitoring (Every 60s)
    
    UK->>Tenant: GET /api/auditvault/v1/health
    Tenant--xUK: ❌ Connection Timeout (48s)
    
    Note over UK: Retry Logic Begins
    loop 3 Retries
        UK->>Tenant: Retry Check (60s wait)
        Tenant--xUK: ❌ Still Down
    end
    
    Note over UK,Spike: All Retries Failed (Total: 3m 48s)
    
    UK->>Spike: POST Webhook
    Note right of UK: JSON Payload:<br/>{<br/>  "monitor_name": "client1-auditvault-api",<br/>  "monitor_url": "https://client1.hub.fixpliance.ai/api/auditvault/v1/health",<br/>  "status_message": "Connection timeout",<br/>  "response_time": "0",<br/>  "tenant": "client1"<br/>}
    
    Spike->>Spike: Create Incident INC-2025-XXX
    Note over Spike: Parse Data & Extract:<br/>- Tenant: client1<br/>- Service: auditvault-api<br/>- Status: DOWN<br/>- Response: 0ms
    
    rect rgb(255, 220, 100)
        Note over SMS,Eng: STEP 1 - SMS (T+0 min)
        Spike->>SMS: Format & Send SMS
        SMS->>Eng: 🚨 client1.hub.fixpliance.ai<br/>Audit Vault API DOWN<br/>Connection timeout
        Note over Eng: Has 1 minute to ACK
    end
    
    alt No Response after 1 min
        rect rgb(150, 230, 180)
            Note over Slack,Eng: STEP 2 - Slack (T+1 min)
            Spike->>Slack: Post to #dev3-alerts
            Slack->>Eng: 🔴 @channel CRITICAL<br/><br/>**Client:** client1<br/>**Service:** Audit Vault API<br/>**URL:** https://client1.hub.fixpliance.ai/api/auditvault/v1/health<br/>**Status:** Connection timeout<br/>**Time:** 10:30:00
            Note over Eng: Has 1 minute to ACK
        end
    end
    
    alt No Response after 2 min
        rect rgb(150, 220, 240)
            Note over Email,Eng: STEP 3 - Email (T+2 min)
            Spike->>Email: Send Detailed Email
            Email->>Eng: Subject: 🚨 CRITICAL: client1 Audit Vault API Down<br/><br/>Full incident details with context
            Note over Eng: Has 1 minute to ACK
        end
    end
    
    alt No Response after 3 min
        rect rgb(255, 150, 150)
            Note over Phone,Eng: STEP 4 - Phone (T+3 min)
            Spike->>Phone: Initiate Call
            Phone->>Eng: 📞 CRITICAL ALERT<br/>client1 Audit Vault API is down<br/>Press 1 to acknowledge
        end
    end
    
    Eng->>Spike: ✅ Acknowledged via Phone
    Note over Eng: Begin Investigation
    
    Eng->>Tenant: Deploy Fix / Restart Service
    Tenant->>UK: ✅ GET /api/auditvault/v1/health → 200 OK
    
    UK->>Spike: POST Webhook (Service Recovered)
    Spike->>Spike: Auto-Resolve Incident
    
    Spike->>SMS: ✅ Resolved: client1 Audit Vault API back online
    Spike->>Slack: ✅ Incident Resolved
    Spike->>Email: ✉️ Resolution Summary
```

## 📊 Service Health Matrix

```mermaid
graph LR
    subgraph "Tenant: client1.hub.fixpliance.ai"
        A1[Web UI ✅ 99.8%]
        A2[Root ✅ 99.9%]
        A3[Audit Vault API ✅ 99.5%]
        A4[Consumer API ✅ 99.7%]
        A5[Vigilant API ✅ 99.6%]
        A6[Node Monitor API ✅ 99.4%]
        A7[Scheduler API ✅ 99.3%]
    end
    
    subgraph "Tenant: development"
        B1[SonarQube ✅ 98.5%]
        B2[Grafana ✅ 99.1%]
        B3[ArgoCD ✅ 98.8%]
    end
    
    subgraph "Overall Health"
        C1[Frontend: 99.85%]
        C2[Backend APIs: 99.5%]
        C3[DevOps: 98.8%]
        C4[Total: 99.4%]
    end
    
    A1 --> C1
    A2 --> C1
    
    A3 --> C2
    A4 --> C2
    A5 --> C2
    A6 --> C2
    A7 --> C2
    
    B1 --> C3
    B2 --> C3
    B3 --> C3
    
    C1 --> C4
    C2 --> C4
    C3 --> C4
    
    style C4 fill:#6bcf7f
    style A1 fill:#6bcf7f
    style A2 fill:#6bcf7f
    style A3 fill:#6bcf7f
```

---

## 🔧 Uptime Kuma Webhook Payload (Updated)

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

### **Example Monitors in Uptime Kuma:**

| Monitor Name | URL | Description | Tags |
|-------------|-----|-------------|------|
| `client1-web-ui` | `https://client1.hub.fixpliance.ai/auditvault` | Web UI | `production, frontend, client1` |
| `client1-auditvault-api` | `https://client1.hub.fixpliance.ai/api/auditvault/v1/health` | Audit Vault API | `production, api, critical, client1` |
| `client1-consumer-api` | `https://client1.hub.fixpliance.ai/api/consumer/v1/health` | Consumer API | `production, api, client1` |
| `client1-vigilant-api` | `https://client1.hub.fixpliance.ai/api/vigilant/v1/health` | Vigilant API | `production, api, client1` |
| `client1-node-monitor-api` | `https://client1.hub.fixpliance.ai/api/node-monitor/v1/health` | Node Monitor API | `production, api, client1` |
| `client1-scheduler-api` | `https://client1.hub.fixpliance.ai/api/scheduler/v1/health` | Scheduler API | `production, api, client1` |
| `client1-root` | `https://client1.hub.fixpliance.ai/` | Root Domain | `production, frontend, client1` |
| `dev-sonarqube` | `https://development.hub.fixpliance.ai/sonarqube` | SonarQube | `development, devops, sonarqube` |
| `dev-grafana` | `https://development.hub.fixpliance.ai/grafana/healthz` | Grafana | `development, devops, monitoring` |
| `dev-argocd` | `https://development.hub.fixpliance.ai/argocd/healthz` | ArgoCD | `development, devops, deployment` |

---

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
