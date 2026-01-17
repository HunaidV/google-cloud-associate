# Exam Tips & Best Practices for Google Cloud Associate Cloud Engineer

## 🎯 General Exam Strategy

### Before the Exam

#### 1. Time Management
- **Exam Duration:** 2 hours (120 minutes)
- **Number of Questions:** ~50 questions
- **Average Time per Question:** 2-2.5 minutes
- **Strategy:** Don't spend more than 3 minutes on any single question

#### 2. Exam Environment Setup
- **Test your computer:** Ensure webcam, microphone work
- **Clean workspace:** Remove all materials from desk
- **Stable internet:** Use wired connection if possible
- **Close all applications:** Only browser should be open
- **Valid ID:** Government-issued photo ID required
- **Arrive early:** Check in 15 minutes before exam time

#### 3. Final Week Preparation
- [ ] Complete at least 2 full practice exams
- [ ] Review all weak areas identified in practice
- [ ] Go through this cheatsheet completely
- [ ] Practice key gcloud and gsutil commands
- [ ] Review IAM roles and permissions
- [ ] Sleep well the night before

### During the Exam

#### 1. Question-Answering Strategy
1. **Read carefully:** Read the entire question and all options
2. **Identify key words:** "most cost-effective", "highest availability", "least effort"
3. **Eliminate wrong answers:** Cross out obviously incorrect options
4. **Flag for review:** Mark questions you're unsure about
5. **Don't overthink:** Your first instinct is often correct
6. **Manage time:** Keep track of time, aim to finish with 20-30 minutes to spare

#### 2. Question Types to Expect

**Scenario-Based Questions (Most Common)**
- Presented with a business problem
- Asked to choose the best solution
- Focus on: cost-effectiveness, scalability, security, availability

**Technical Implementation Questions**
- How to configure specific services
- Which commands to use
- Troubleshooting scenarios

**Comparison Questions**
- When to use Service A vs Service B
- Trade-offs between approaches

#### 3. Common Keywords and What They Mean

| Keyword | What It Usually Means |
|---------|----------------------|
| **Most cost-effective** | Choose cheaper option (Preemptible VMs, Standard storage, sustained use discounts) |
| **Highest availability** | Multi-region, HA configurations, replicas |
| **Least effort/operational overhead** | Managed services, serverless options |
| **Lowest latency** | Regional resources, CDN, Memorystore |
| **Scalable** | Auto-scaling, managed services, serverless |
| **Secure** | IAM, service accounts, VPC, encryption |
| **Real-time** | Streaming, Pub/Sub, Dataflow |
| **Legacy system** | Often points to lift-and-shift, Compute Engine |

---

## 📚 Topic-Specific Tips

### Compute Services

#### Compute Engine
✅ **When to Use:**
- Full control over VMs needed
- Specific OS requirements
- Legacy applications (lift-and-shift)
- Workloads requiring specific machine configurations

❌ **When NOT to Use:**
- Don't want to manage infrastructure
- Serverless option available
- Need automatic scaling to zero

**Key Points:**
- Preemptible VMs save up to 80% but can be terminated
- Sustained use discounts are automatic
- Use instance templates for consistent deployments
- Managed instance groups for auto-scaling
- Startup scripts run on VM creation

#### Google Kubernetes Engine (GKE)
✅ **When to Use:**
- Containerized applications
- Microservices architecture
- Need orchestration
- Multi-cloud portability

**Key Points:**
- Regional clusters for HA (nodes across zones)
- Workload Identity for secure pod authentication
- Node pools for different machine types
- Auto-repair and auto-upgrade available
- Use Deployment controller for stateless apps
- Use StatefulSet for stateful apps

#### Cloud Run
✅ **When to Use:**
- Containerized HTTP applications
- Want serverless containers
- Pay per use model
- Scale to zero needed

**Key Points:**
- Fully managed serverless
- Scales automatically (including to zero)
- Each revision is immutable
- Supports any language/binary
- Maximum request timeout: 60 minutes

#### Cloud Functions
✅ **When to Use:**
- Event-driven workloads
- Lightweight processing
- Glue code between services
- Webhooks

**Key Points:**
- Triggered by events (HTTP, Storage, Pub/Sub, etc.)
- 1st gen vs 2nd gen (different features)
- Maximum execution time: 9 minutes (1st gen), 60 minutes (2nd gen)
- Automatic scaling
- Cold start consideration

#### App Engine
✅ **When to Use:**
- Want fully managed PaaS
- Web applications
- Don't want to manage infrastructure

**Key Points:**
- Standard vs Flexible environments
- Standard: Faster scaling, language-specific
- Flexible: Docker containers, more flexibility
- Traffic splitting for A/B testing
- Version management built-in

---

### Storage Services

#### Cloud Storage
**Storage Class Selection:**

| Use Case | Storage Class | Access Frequency |
|----------|--------------|------------------|
| Frequently accessed, hot data | Standard | Multiple times per month |
| Backups, data accessed monthly | Nearline | Once per month |
| Disaster recovery, quarterly access | Coldline | Once per quarter |
| Long-term archival, compliance | Archive | Less than once per year |

**Key Points:**
- Lifecycle policies automate class transitions
- Versioning prevents accidental deletion
- Signed URLs for temporary access
- Uniform bucket-level access vs fine-grained ACLs
- Regional vs multi-region vs dual-region
- Egress charges apply when downloading

**Common Mistakes to Avoid:**
- ❌ Don't use Archive for frequently accessed data
- ❌ Don't forget about early deletion fees (Nearline: 30 days, Coldline: 90 days, Archive: 365 days)
- ❌ Don't use Standard for rarely accessed data

#### Persistent Disks
**Types and When to Use:**
- **Standard (pd-standard):** Cost-sensitive, sequential access
- **Balanced (pd-balanced):** Most workloads, balanced price/performance
- **SSD (pd-ssd):** High IOPS, databases
- **Extreme (pd-extreme):** Highest performance needs

**Key Points:**
- Independent of VM lifecycle
- Can attach to multiple VMs (read-only)
- Regional PDs for HA (2x cost)
- Snapshots for backup (incremental)

---

### Database Services

#### Service Selection Guide

| Use Case | Recommended Service |
|----------|-------------------|
| Relational, vertical scaling | Cloud SQL |
| Relational, horizontal scaling, global | Cloud Spanner |
| NoSQL, document database | Firestore |
| NoSQL, analytical, time-series | Bigtable |
| In-memory cache | Memorystore |
| Data warehouse, analytics | BigQuery |

#### Cloud SQL
**Key Points:**
- Supports MySQL, PostgreSQL, SQL Server
- HA configuration with automatic failover
- Read replicas for scaling reads
- Point-in-time recovery
- Private IP for secure access
- Automatic backups

**High Availability:**
- Primary instance + standby instance in different zones
- Synchronous replication
- Automatic failover

#### Cloud Spanner
**Key Points:**
- Globally distributed
- Horizontal scaling
- Strong consistency
- 99.999% availability SLA (multi-region)
- More expensive than Cloud SQL
- SQL-like interface

---

### Networking

#### VPC Fundamentals
**Key Concepts:**
- VPC is global (not regional)
- Subnets are regional
- Firewall rules are stateful
- Each VPC has implied allow egress and deny ingress rules
- Routes define traffic paths

**Firewall Rules:**
- Default: Implied deny all ingress, allow all egress
- Priority: 0-65535 (lower number = higher priority)
- Direction: Ingress or Egress
- Action: Allow or Deny
- Target: All instances, tags, or service accounts
- Deny rules override allow rules of same priority

#### Load Balancing
**Choosing the Right Load Balancer:**

| Type | Layer | Scope | Use Case |
|------|-------|-------|----------|
| HTTP(S) | 7 | Global | Web applications, content-based routing |
| SSL Proxy | 4 | Global | SSL offloading, non-HTTP(S) |
| TCP Proxy | 4 | Global | TCP traffic without SSL |
| Network | 4 | Regional | High performance, preserve client IP |
| Internal | 4 | Regional | Private load balancing |

**Key Points:**
- Global load balancers route to closest healthy backend
- Health checks determine backend health
- Backend services group backends
- URL maps for content-based routing
- CDN can be enabled for HTTP(S) load balancers

#### Cloud CDN
**When to Use:**
- Static content (images, videos, CSS, JS)
- Reduce latency for global users
- Reduce load on origin servers

**Key Points:**
- Works with HTTP(S) Load Balancer
- Cache content at Google edge locations
- Cache control headers determine caching behavior
- Invalidation available but costs money

#### VPN & Interconnect
**Cloud VPN:**
- Classic VPN: 99.9% SLA, up to 3 Gbps per tunnel
- HA VPN: 99.99% SLA, two interfaces, two external IPs

**Cloud Interconnect:**
- Dedicated: Direct physical connection (10 or 100 Gbps)
- Partner: Through service provider
- Lower latency than VPN
- Not encrypted by default (use application-layer encryption)

---

### IAM & Security

#### IAM Best Practices
1. **Principle of Least Privilege:** Grant minimum permissions needed
2. **Use Groups:** Assign roles to groups, not individuals
3. **Predefined Roles:** Use Google's predefined roles when possible
4. **Service Accounts:** For applications, not humans
5. **Regular Audits:** Review permissions regularly

#### Role Types
1. **Primitive Roles (Basic Roles):** Owner, Editor, Viewer
   - ❌ Too broad for production
   - ✅ OK for dev/test environments

2. **Predefined Roles:** Service-specific curated roles
   - ✅ Recommended for most use cases
   - Examples: `roles/compute.instanceAdmin.v1`, `roles/storage.objectViewer`

3. **Custom Roles:** User-defined roles
   - ✅ Use when predefined roles don't fit
   - Only at project or organization level

#### Service Accounts
**Key Points:**
- Not for human users
- Each service account is also a resource
- Can have up to 10 keys per service account
- Keys should be rotated regularly
- Use Workload Identity for GKE pods
- Default service accounts exist (not recommended for production)

**Common Mistake:**
- ❌ Don't download service account keys if not necessary
- ✅ Use Workload Identity, Application Default Credentials, or instance service accounts

---

### Monitoring & Operations

#### Cloud Monitoring
**Key Metrics to Know:**
- **CPU Utilization:** For auto-scaling decisions
- **Disk I/O:** For storage performance
- **Network Traffic:** For bandwidth monitoring
- **Uptime Checks:** For availability monitoring

**Alerting Policies:**
- Condition + Notification
- Multiple conditions: Any or All
- Alert on: Metric threshold, uptime check, log entries

#### Cloud Logging
**Key Concepts:**
- Structured vs unstructured logs
- Log retention: 30 days (default, can be extended)
- Log sinks export to: Cloud Storage, BigQuery, Pub/Sub
- Log-based metrics for custom monitoring

**Common Use Cases:**
- Audit logs (who did what)
- System logs (health and performance)
- Application logs (custom logging)
- Security logs (firewall rules, IAM changes)

---

## 💡 Common Exam Scenarios

### Scenario 1: Cost Optimization
**Question Type:** "What's the most cost-effective solution?"

**Think:**
- Preemptible/Spot VMs for fault-tolerant workloads
- Right-size machine types
- Committed use discounts for predictable workloads
- Auto-scaling to match demand
- Lower storage classes for infrequent access
- Serverless to pay only for usage

### Scenario 2: High Availability
**Question Type:** "Ensure highest availability"

**Think:**
- Multi-zone or multi-region deployment
- Regional managed instance groups
- Read replicas for databases
- Regional persistent disks
- Global load balancing
- Health checks and auto-healing

### Scenario 3: Security & Compliance
**Question Type:** "Ensure data security" or "Who should have access?"

**Think:**
- IAM roles with least privilege
- Service accounts for applications
- VPC for network isolation
- Private IP for databases
- Encryption at rest and in transit (default in GCP)
- Cloud KMS for key management
- Audit logs for compliance

### Scenario 4: Migration (Lift and Shift)
**Question Type:** "Migrate existing application to GCP"

**Think:**
- Compute Engine for VMs
- Cloud SQL for managed databases
- Cloud Storage for file storage
- VPN for hybrid connectivity
- Cloud Interconnect for large data transfer
- Migrate for Compute Engine for automated migration

### Scenario 5: Serverless Architecture
**Question Type:** "Minimal operational overhead"

**Think:**
- Cloud Functions for event-driven
- Cloud Run for containerized HTTP services
- App Engine for full applications
- Cloud SQL (managed database)
- Cloud Storage (managed object storage)
- Pub/Sub for messaging

---

## 🚨 Common Mistakes to Avoid

### 1. Confusing Similar Services
- **Cloud Storage vs Persistent Disk:** Object storage vs Block storage
- **Cloud SQL vs Cloud Spanner:** Regional vs Global, Vertical vs Horizontal scaling
- **Firestore vs Bigtable:** Document database vs Wide-column database
- **Cloud Functions vs Cloud Run:** Functions (event-driven) vs Containers (HTTP)

### 2. Networking Mistakes
- ❌ Forgetting VPC is global, subnets are regional
- ❌ Not understanding firewall rule priority
- ❌ Assuming default-allow-all (it's deny-all ingress by default)

### 3. IAM Mistakes
- ❌ Using primitive roles (Owner, Editor, Viewer) in production
- ❌ Creating unnecessary service account keys
- ❌ Not understanding resource hierarchy (Org > Folder > Project > Resource)

### 4. Cost Mistakes
- ❌ Forgetting about egress charges
- ❌ Not using auto-scaling (over-provisioning)
- ❌ Using expensive storage class for temporary data
- ❌ Running VMs when serverless would work

### 5. Availability Mistakes
- ❌ Single-zone deployments for critical workloads
- ❌ No health checks configured
- ❌ Not implementing auto-healing

---

## 🎓 Command Line Cheat Sheet (Exam Relevant)

### Most Important gcloud Commands

```bash
# Configuration
gcloud config list
gcloud config set project PROJECT_ID
gcloud config set compute/zone ZONE

# Compute Engine
gcloud compute instances create INSTANCE_NAME
gcloud compute instances list
gcloud compute instances start/stop/delete INSTANCE_NAME
gcloud compute ssh INSTANCE_NAME

# GKE
gcloud container clusters create CLUSTER_NAME
gcloud container clusters get-credentials CLUSTER_NAME
gcloud container clusters delete CLUSTER_NAME

# IAM
gcloud projects add-iam-policy-binding PROJECT_ID --member=user:EMAIL --role=ROLE
gcloud iam service-accounts create SA_NAME

# Cloud SQL
gcloud sql instances create INSTANCE_NAME
gcloud sql databases create DB_NAME --instance=INSTANCE_NAME
```

### gsutil Commands
```bash
gsutil mb gs://BUCKET_NAME              # Create bucket
gsutil cp FILE gs://BUCKET_NAME/        # Copy file
gsutil ls gs://BUCKET_NAME               # List objects
gsutil lifecycle set CONFIG.json gs://BUCKET_NAME  # Set lifecycle
gsutil versioning set on gs://BUCKET_NAME  # Enable versioning
```

---

## ⏰ Last 24 Hours Before Exam

### Review Checklist
- [ ] IAM role types (primitive, predefined, custom)
- [ ] Storage classes and use cases
- [ ] Load balancer types and selection
- [ ] Compute service selection (CE, GKE, Cloud Run, Functions)
- [ ] Database service selection
- [ ] VPC networking basics
- [ ] gcloud command syntax
- [ ] Common port numbers (22-SSH, 80-HTTP, 443-HTTPS, 3389-RDP)

### Don't
- ❌ Cram new information
- ❌ Take a full practice exam
- ❌ Stay up late studying
- ❌ Drink too much coffee before exam

### Do
- ✅ Review your notes and weak areas
- ✅ Get good sleep
- ✅ Eat a healthy meal
- ✅ Test your exam setup early
- ✅ Stay calm and confident

---

## 🎯 Final Tips

1. **Trust Your Preparation:** If you've done the labs and studied, you're ready
2. **Read Questions Carefully:** Many wrong answers are there to catch quick readers
3. **Look for Keywords:** "most cost-effective", "highest availability", "least effort"
4. **Process of Elimination:** Remove obviously wrong answers first
5. **Flag and Move On:** Don't waste time on questions you're unsure about
6. **Review Flagged Questions:** Use remaining time to review flagged questions
7. **Stay Calm:** If you don't know an answer, make your best guess and move on
8. **Manage Time:** Keep an eye on the clock
9. **Don't Change Answers:** Unless you're certain, stick with your first choice
10. **Believe in Yourself:** You've got this! 💪

---

## 📞 On Exam Day

### Technical Issues
- If you have technical issues during online exam, use chat with proctor
- For test center issues, notify proctor immediately
- Don't panic - issues can usually be resolved

### After the Exam
- Results available immediately (pass/fail)
- Official score report within a few days
- If you pass: Certificate and badge within 1-2 weeks
- If you don't pass: Wait 14 days before retaking

---

Good luck on your exam! Remember: Adequate preparation + calm mind = Success 🎓🚀
