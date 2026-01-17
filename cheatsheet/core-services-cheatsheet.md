# Core GCP Services Cheatsheet

## 🖥️ Compute Services

### Compute Engine
**Purpose:** Infrastructure as a Service (IaaS) - Virtual Machines

**Key Features:**
- Predefined and custom machine types
- Persistent disks (standard, SSD, balanced)
- Local SSD for high-performance temporary storage
- Preemptible VMs (up to 80% cost savings)
- Spot VMs (even more flexible pricing)
- Managed instance groups (MIGs)
- Live migration

**Common gcloud Commands:**
```bash
# Create a VM instance
gcloud compute instances create INSTANCE_NAME --zone=ZONE

# List instances
gcloud compute instances list

# Start/Stop instance
gcloud compute instances start INSTANCE_NAME
gcloud compute instances stop INSTANCE_NAME

# SSH into instance
gcloud compute ssh INSTANCE_NAME --zone=ZONE

# Delete instance
gcloud compute instances delete INSTANCE_NAME
```

**Machine Types:**
- **E2:** Cost-optimized, day-to-day computing
- **N2/N2D:** Balanced price/performance
- **C2/C2D:** Compute-optimized (HPC, gaming)
- **M2/M3:** Memory-optimized (databases, in-memory analytics)
- **A2:** GPU-optimized (ML, HPC)

### Google Kubernetes Engine (GKE)
**Purpose:** Managed Kubernetes service

**Key Features:**
- Fully managed Kubernetes
- Auto-scaling (cluster and pod)
- Auto-upgrade and auto-repair
- Integrated with Cloud Monitoring and Logging
- Workload Identity for secure pod authentication
- Private clusters

**Common kubectl Commands:**
```bash
# Create a cluster
gcloud container clusters create CLUSTER_NAME --zone=ZONE

# Get cluster credentials
gcloud container clusters get-credentials CLUSTER_NAME

# Create deployment
kubectl create deployment NAME --image=IMAGE

# Expose deployment
kubectl expose deployment NAME --type=LoadBalancer --port=80

# Scale deployment
kubectl scale deployment NAME --replicas=3

# Get resources
kubectl get pods
kubectl get services
kubectl get deployments
```

### Cloud Run
**Purpose:** Fully managed serverless platform for containers

**Key Features:**
- Auto-scaling to zero
- Pay per use (CPU, memory, requests)
- HTTPS endpoints automatically
- Supports any language, any library
- Fast deployment

**Common Commands:**
```bash
# Deploy a container
gcloud run deploy SERVICE_NAME --image=IMAGE_URL --platform=managed

# List services
gcloud run services list

# Update service
gcloud run services update SERVICE_NAME --memory=512Mi
```

### Cloud Functions
**Purpose:** Event-driven serverless functions (FaaS)

**Key Features:**
- Supports Node.js, Python, Go, Java, .NET, Ruby, PHP
- Triggered by HTTP, Pub/Sub, Cloud Storage, Firestore
- Automatic scaling
- Pay per invocation

**Trigger Types:**
- HTTP triggers
- Cloud Storage triggers
- Cloud Pub/Sub triggers
- Firebase triggers
- Cloud Firestore triggers

**Common Commands:**
```bash
# Deploy function
gcloud functions deploy FUNCTION_NAME --runtime=python39 --trigger-http

# List functions
gcloud functions list

# Call function
gcloud functions call FUNCTION_NAME --data='{"key":"value"}'
```

### App Engine
**Purpose:** Fully managed PaaS for applications

**Key Features:**
- Standard and Flexible environments
- Automatic scaling
- Traffic splitting
- Built-in services (Task Queue, Memcache)
- Zero server management

**Environments:**
- **Standard:** Language-specific sandboxes, faster scaling
- **Flexible:** Docker containers, more flexibility

---

## 💾 Storage Services

### Cloud Storage
**Purpose:** Object storage service

**Storage Classes:**
1. **Standard:** Frequently accessed data, no minimum storage duration
2. **Nearline:** Infrequently accessed (once per month), 30-day minimum
3. **Coldline:** Rarely accessed (once per 90 days), 90-day minimum
4. **Archive:** Long-term archival (once per year), 365-day minimum

**Key Features:**
- 11 nines durability (99.999999999%)
- Versioning
- Lifecycle management
- Object holds and retention policies
- Signed URLs for temporary access
- Uniform and fine-grained access control

**Common gsutil Commands:**
```bash
# Create bucket
gsutil mb gs://BUCKET_NAME

# Copy file to bucket
gsutil cp FILE gs://BUCKET_NAME/

# List bucket contents
gsutil ls gs://BUCKET_NAME

# Set bucket lifecycle
gsutil lifecycle set LIFECYCLE_CONFIG.json gs://BUCKET_NAME

# Make object public
gsutil acl ch -u AllUsers:R gs://BUCKET_NAME/OBJECT

# Sync directories
gsutil rsync -r LOCAL_DIR gs://BUCKET_NAME
```

**Lifecycle Actions:**
- Delete objects
- SetStorageClass (transition to different class)

### Persistent Disk
**Purpose:** Block storage for VM instances

**Types:**
- **Standard (pd-standard):** HDD, cost-effective
- **Balanced (pd-balanced):** SSD, balance of performance and cost
- **SSD (pd-ssd):** High performance SSD
- **Extreme (pd-extreme):** Highest performance

**Key Features:**
- Independent of VM lifecycle
- Snapshots for backup
- Can be attached to multiple VMs (read-only)
- Regional persistent disks for HA

### Filestore
**Purpose:** Managed NFS file storage

**Use Cases:**
- Enterprise applications
- Media workflows
- Shared file access across VMs

**Tiers:**
- **Basic HDD:** Low cost
- **Basic SSD:** Balanced performance
- **Enterprise:** High performance and availability

---

## 🗄️ Database Services

### Cloud SQL
**Purpose:** Managed relational database service

**Supported Engines:**
- MySQL
- PostgreSQL
- SQL Server

**Key Features:**
- Automatic backups and point-in-time recovery
- High availability configuration
- Read replicas
- Automatic storage increase
- Private IP connectivity

**Common Commands:**
```bash
# Create instance
gcloud sql instances create INSTANCE_NAME --database-version=MYSQL_8_0 --tier=db-n1-standard-1

# Connect to instance
gcloud sql connect INSTANCE_NAME --user=root

# Create database
gcloud sql databases create DATABASE_NAME --instance=INSTANCE_NAME
```

### Cloud Spanner
**Purpose:** Globally distributed relational database

**Key Features:**
- Horizontal scaling
- Strong consistency
- 99.999% availability SLA
- SQL support
- Multi-region and global configurations

**Use Cases:**
- Financial applications
- Global applications requiring consistency
- Applications needing horizontal scale

### Firestore (Cloud Firestore)
**Purpose:** NoSQL document database

**Key Features:**
- Real-time synchronization
- Offline support
- Strong consistency
- ACID transactions
- Automatic scaling

**Modes:**
- **Native mode:** Real-time updates, mobile/web SDKs
- **Datastore mode:** Server-side applications, compatible with Datastore

### Cloud Bigtable
**Purpose:** NoSQL wide-column database for big data

**Key Features:**
- Petabyte-scale
- Low latency (< 10ms)
- High throughput
- HBase API compatible
- Time-series data support

**Use Cases:**
- IoT data
- Time-series data
- Financial data
- AdTech and MarTech

### Memorystore
**Purpose:** Managed Redis and Memcached

**Key Features:**
- Fully managed
- High availability
- Sub-millisecond latency
- Redis or Memcached protocols

---

## 🌐 Networking Services

### Virtual Private Cloud (VPC)
**Purpose:** Private network within GCP

**Key Concepts:**
- **Global resource** spanning all regions
- **Subnets:** Regional resources with IP ranges
- **Routes:** Define paths for traffic
- **Firewall rules:** Allow/deny traffic

**VPC Features:**
- Shared VPC (cross-project networking)
- VPC Peering (connect VPCs)
- Private Google Access (access Google services without external IP)
- VPC Flow Logs

**Firewall Rules:**
```bash
# Create firewall rule
gcloud compute firewall-rules create RULE_NAME \
  --allow=tcp:80,tcp:443 \
  --source-ranges=0.0.0.0/0 \
  --target-tags=web-server

# List firewall rules
gcloud compute firewall-rules list
```

### Cloud Load Balancing
**Purpose:** Distribute traffic across multiple instances

**Types:**
1. **Global HTTP(S) Load Balancer:** Layer 7, global, content-based routing
2. **Global SSL Proxy:** Layer 4, global, SSL offloading
3. **Global TCP Proxy:** Layer 4, global, TCP traffic
4. **Regional Network Load Balancer:** Layer 4, regional, pass-through
5. **Regional Internal Load Balancer:** Layer 4, internal traffic

**Key Features:**
- Auto-scaling
- Health checks
- CDN integration (HTTP(S))
- SSL termination

### Cloud CDN
**Purpose:** Content delivery network

**Key Features:**
- Global edge caching
- Integrated with Cloud Load Balancing
- Cache invalidation
- Signed URLs and cookies

### Cloud DNS
**Purpose:** Managed DNS service

**Features:**
- 100% uptime SLA
- Low latency
- DNSSEC support
- Public and private zones

### Cloud VPN
**Purpose:** Secure connection to GCP

**Types:**
- **Classic VPN:** 99.9% SLA, single tunnel
- **HA VPN:** 99.99% SLA, multiple tunnels

### Cloud Interconnect
**Purpose:** Dedicated connection to GCP

**Types:**
- **Dedicated Interconnect:** Direct physical connection (10 Gbps or 100 Gbps)
- **Partner Interconnect:** Connection through partner

---

## 🔐 Security & Identity

### Identity and Access Management (IAM)
**Purpose:** Access control for GCP resources

**Key Concepts:**
- **Principal:** Who (user, service account, group)
- **Role:** What permissions
- **Resource:** Which resource

**Role Types:**
1. **Primitive Roles:** Owner, Editor, Viewer (not recommended for production)
2. **Predefined Roles:** Service-specific curated roles
3. **Custom Roles:** User-defined roles

**Common Commands:**
```bash
# Grant IAM role
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member=user:EMAIL \
  --role=roles/compute.instanceAdmin

# List IAM policies
gcloud projects get-iam-policy PROJECT_ID

# Create service account
gcloud iam service-accounts create SA_NAME

# Create and download key
gcloud iam service-accounts keys create KEY_FILE.json \
  --iam-account=SA_EMAIL
```

### Service Accounts
**Purpose:** Identity for applications and VMs

**Key Features:**
- Not for human users
- Can be assigned IAM roles
- Can generate keys (JSON)
- Used for VM instances, GKE workloads

**Best Practices:**
- Use Workload Identity for GKE
- Rotate keys regularly
- Use minimal permissions
- Avoid downloading keys when possible

---

## 📊 Operations & Monitoring

### Cloud Monitoring (formerly Stackdriver)
**Purpose:** Monitoring, logging, and diagnostics

**Key Features:**
- Dashboards
- Alerting
- Uptime checks
- Service monitoring
- 150+ integrated services

### Cloud Logging
**Purpose:** Log collection and analysis

**Key Features:**
- Real-time log ingestion
- Log-based metrics
- Log sinks (export to BigQuery, Cloud Storage, Pub/Sub)
- Structured logs

**Common Commands:**
```bash
# Read logs
gcloud logging read "resource.type=gce_instance" --limit=10

# Create sink
gcloud logging sinks create SINK_NAME \
  storage.googleapis.com/BUCKET_NAME \
  --log-filter='resource.type=gce_instance'
```

### Cloud Trace
**Purpose:** Distributed tracing system

### Cloud Profiler
**Purpose:** Continuous profiling of CPU and memory

---

## 🔄 DevOps & Management

### Cloud Deployment Manager
**Purpose:** Infrastructure as Code (IaC) for GCP

**Key Features:**
- YAML/Python templates
- Declarative configuration
- Preview changes before deployment

### Cloud Build
**Purpose:** CI/CD platform

**Key Features:**
- Build, test, and deploy
- Trigger on code changes
- Integration with GitHub, Bitbucket
- Docker support

### Container Registry / Artifact Registry
**Purpose:** Container and artifact storage

- **Container Registry:** Docker container images (legacy)
- **Artifact Registry:** Multi-format repository (recommended)

---

## 💬 Messaging & Integration

### Cloud Pub/Sub
**Purpose:** Asynchronous messaging service

**Key Features:**
- At-least-once delivery
- Global scale
- Push and pull subscriptions
- Message ordering
- Dead letter topics

**Common Commands:**
```bash
# Create topic
gcloud pubsub topics create TOPIC_NAME

# Create subscription
gcloud pubsub subscriptions create SUBSCRIPTION_NAME --topic=TOPIC_NAME

# Publish message
gcloud pubsub topics publish TOPIC_NAME --message="Hello"

# Pull messages
gcloud pubsub subscriptions pull SUBSCRIPTION_NAME --auto-ack
```

---

## 📈 Big Data & Analytics

### BigQuery
**Purpose:** Serverless data warehouse

**Key Features:**
- Petabyte-scale
- SQL interface
- Real-time analytics
- ML integration (BigQuery ML)
- Automatic backups

### Cloud Dataflow
**Purpose:** Stream and batch data processing

### Cloud Dataproc
**Purpose:** Managed Spark and Hadoop

### Cloud Composer
**Purpose:** Managed Apache Airflow for workflow orchestration

---

## 🛠️ Other Important Services

### Cloud Scheduler
**Purpose:** Cron job service

### Cloud Tasks
**Purpose:** Distributed task queue

### Secret Manager
**Purpose:** Store and manage sensitive data

### Cloud KMS
**Purpose:** Cryptographic key management

### Binary Authorization
**Purpose:** Deploy-time security control for containers

---

## 💰 Cost Management

### Pricing Calculator
- Estimate costs for GCP services
- URL: https://cloud.google.com/products/calculator

### Committed Use Discounts (CUDs)
- 1-year or 3-year commitments
- Up to 57% discount for Compute Engine

### Sustained Use Discounts
- Automatic discounts for sustained VM usage

### Preemptible/Spot VMs
- Up to 80% discount
- Can be terminated at any time

### Budgets and Alerts
- Set budgets for projects
- Email alerts at threshold percentages
