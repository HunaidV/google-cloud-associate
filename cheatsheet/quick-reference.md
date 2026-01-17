# Quick Reference Guide - Google Cloud Associate Cloud Engineer

## 🚀 Essential gcloud Commands

### Configuration & Setup
```bash
# View current configuration
gcloud config list

# Set default project
gcloud config set project PROJECT_ID

# Set default region/zone
gcloud config set compute/region us-central1
gcloud config set compute/zone us-central1-a

# List configurations
gcloud config configurations list

# Create new configuration
gcloud config configurations create CONFIG_NAME

# Activate configuration
gcloud config configurations activate CONFIG_NAME

# Initialize gcloud
gcloud init

# Authenticate
gcloud auth login
gcloud auth application-default login

# List accounts
gcloud auth list

# Set active account
gcloud config set account ACCOUNT_EMAIL
```

### Compute Engine Quick Commands
```bash
# Create VM (minimal)
gcloud compute instances create VM_NAME --zone=ZONE

# Create VM (with options)
gcloud compute instances create VM_NAME \
  --zone=us-central1-a \
  --machine-type=e2-medium \
  --image-family=debian-11 \
  --image-project=debian-cloud \
  --boot-disk-size=10GB \
  --tags=web-server \
  --metadata=startup-script='#!/bin/bash
    apt-get update'

# List VMs
gcloud compute instances list
gcloud compute instances list --filter="zone:us-central1-a"

# Start/Stop/Restart VM
gcloud compute instances start VM_NAME --zone=ZONE
gcloud compute instances stop VM_NAME --zone=ZONE
gcloud compute instances reset VM_NAME --zone=ZONE

# Delete VM
gcloud compute instances delete VM_NAME --zone=ZONE

# SSH into VM
gcloud compute ssh VM_NAME --zone=ZONE

# Describe instance
gcloud compute instances describe VM_NAME --zone=ZONE

# Update instance
gcloud compute instances add-tags VM_NAME --tags=http-server --zone=ZONE
gcloud compute instances remove-tags VM_NAME --tags=http-server --zone=ZONE

# Attach disk
gcloud compute instances attach-disk VM_NAME --disk=DISK_NAME --zone=ZONE

# Create snapshot
gcloud compute disks snapshot DISK_NAME --snapshot-names=SNAPSHOT_NAME --zone=ZONE

# Create image from disk
gcloud compute images create IMAGE_NAME --source-disk=DISK_NAME --source-disk-zone=ZONE

# Create instance template
gcloud compute instance-templates create TEMPLATE_NAME \
  --machine-type=e2-medium \
  --image-family=debian-11 \
  --image-project=debian-cloud

# Create instance group
gcloud compute instance-groups managed create GROUP_NAME \
  --template=TEMPLATE_NAME \
  --size=3 \
  --zone=ZONE

# Autoscale instance group
gcloud compute instance-groups managed set-autoscaling GROUP_NAME \
  --max-num-replicas=10 \
  --min-num-replicas=1 \
  --target-cpu-utilization=0.6 \
  --zone=ZONE
```

### GKE (Kubernetes) Quick Commands
```bash
# Create cluster
gcloud container clusters create CLUSTER_NAME \
  --zone=us-central1-a \
  --num-nodes=3

# Get credentials
gcloud container clusters get-credentials CLUSTER_NAME --zone=ZONE

# List clusters
gcloud container clusters list

# Delete cluster
gcloud container clusters delete CLUSTER_NAME --zone=ZONE

# Resize cluster
gcloud container clusters resize CLUSTER_NAME --num-nodes=5 --zone=ZONE

# Upgrade cluster
gcloud container clusters upgrade CLUSTER_NAME --zone=ZONE
```

### Cloud Storage (gsutil) Quick Commands
```bash
# Create bucket
gsutil mb gs://BUCKET_NAME
gsutil mb -c NEARLINE -l us-central1 gs://BUCKET_NAME

# List buckets
gsutil ls
gsutil ls gs://BUCKET_NAME

# Copy files
gsutil cp FILE.txt gs://BUCKET_NAME/
gsutil cp gs://BUCKET_NAME/FILE.txt .
gsutil cp -r DIRECTORY gs://BUCKET_NAME/

# Move files
gsutil mv gs://BUCKET_NAME/OLD.txt gs://BUCKET_NAME/NEW.txt

# Delete files
gsutil rm gs://BUCKET_NAME/FILE.txt
gsutil rm -r gs://BUCKET_NAME/FOLDER

# Delete bucket
gsutil rb gs://BUCKET_NAME

# Sync directories
gsutil rsync -r LOCAL_DIR gs://BUCKET_NAME/DIR

# Make object public
gsutil acl ch -u AllUsers:R gs://BUCKET_NAME/FILE.txt

# Set lifecycle policy
gsutil lifecycle set lifecycle.json gs://BUCKET_NAME

# Enable versioning
gsutil versioning set on gs://BUCKET_NAME

# View object metadata
gsutil stat gs://BUCKET_NAME/FILE.txt

# Set storage class
gsutil rewrite -s NEARLINE gs://BUCKET_NAME/FILE.txt

# Create signed URL (1 hour)
gsutil signurl -d 1h KEY_FILE.json gs://BUCKET_NAME/FILE.txt
```

### IAM Quick Commands
```bash
# List IAM policy
gcloud projects get-iam-policy PROJECT_ID

# Add IAM policy binding
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member=user:EMAIL \
  --role=roles/compute.instanceAdmin.v1

# Remove IAM policy binding
gcloud projects remove-iam-policy-binding PROJECT_ID \
  --member=user:EMAIL \
  --role=roles/compute.instanceAdmin.v1

# Create service account
gcloud iam service-accounts create SA_NAME \
  --display-name="Display Name"

# List service accounts
gcloud iam service-accounts list

# Create service account key
gcloud iam service-accounts keys create KEY_FILE.json \
  --iam-account=SA_EMAIL

# List service account keys
gcloud iam service-accounts keys list --iam-account=SA_EMAIL

# Delete service account key
gcloud iam service-accounts keys delete KEY_ID --iam-account=SA_EMAIL

# Grant role to service account
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member=serviceAccount:SA_EMAIL \
  --role=roles/storage.objectViewer

# Impersonate service account
gcloud compute instances list --impersonate-service-account=SA_EMAIL
```

### Networking Quick Commands
```bash
# Create VPC network
gcloud compute networks create VPC_NAME --subnet-mode=custom

# Create subnet
gcloud compute networks subnets create SUBNET_NAME \
  --network=VPC_NAME \
  --range=10.0.0.0/24 \
  --region=us-central1

# List networks
gcloud compute networks list
gcloud compute networks subnets list

# Create firewall rule
gcloud compute firewall-rules create RULE_NAME \
  --network=VPC_NAME \
  --allow=tcp:80,tcp:443 \
  --source-ranges=0.0.0.0/0

# List firewall rules
gcloud compute firewall-rules list
gcloud compute firewall-rules describe RULE_NAME

# Delete firewall rule
gcloud compute firewall-rules delete RULE_NAME

# Reserve static IP
gcloud compute addresses create ADDRESS_NAME --region=REGION

# List IP addresses
gcloud compute addresses list
```

### Cloud SQL Quick Commands
```bash
# Create instance
gcloud sql instances create INSTANCE_NAME \
  --database-version=MYSQL_8_0 \
  --tier=db-n1-standard-1 \
  --region=us-central1

# List instances
gcloud sql instances list

# Describe instance
gcloud sql instances describe INSTANCE_NAME

# Connect to instance
gcloud sql connect INSTANCE_NAME --user=root

# Create database
gcloud sql databases create DB_NAME --instance=INSTANCE_NAME

# Create user
gcloud sql users create USER_NAME \
  --instance=INSTANCE_NAME \
  --password=PASSWORD

# Backup instance
gcloud sql backups create --instance=INSTANCE_NAME

# Restore from backup
gcloud sql backups restore BACKUP_ID --instance=INSTANCE_NAME

# Delete instance
gcloud sql instances delete INSTANCE_NAME
```

### Cloud Functions Quick Commands
```bash
# Deploy function (HTTP trigger)
gcloud functions deploy FUNCTION_NAME \
  --runtime=python39 \
  --trigger-http \
  --allow-unauthenticated

# Deploy function (Storage trigger)
gcloud functions deploy FUNCTION_NAME \
  --runtime=python39 \
  --trigger-resource=BUCKET_NAME \
  --trigger-event=google.storage.object.finalize

# List functions
gcloud functions list

# Describe function
gcloud functions describe FUNCTION_NAME

# Call function
gcloud functions call FUNCTION_NAME --data='{"key":"value"}'

# View logs
gcloud functions logs read FUNCTION_NAME

# Delete function
gcloud functions delete FUNCTION_NAME
```

### Cloud Run Quick Commands
```bash
# Deploy service
gcloud run deploy SERVICE_NAME \
  --image=gcr.io/PROJECT_ID/IMAGE \
  --platform=managed \
  --region=us-central1 \
  --allow-unauthenticated

# List services
gcloud run services list

# Describe service
gcloud run services describe SERVICE_NAME

# Update service
gcloud run services update SERVICE_NAME \
  --memory=512Mi \
  --cpu=2 \
  --max-instances=10

# Delete service
gcloud run services delete SERVICE_NAME
```

### Pub/Sub Quick Commands
```bash
# Create topic
gcloud pubsub topics create TOPIC_NAME

# List topics
gcloud pubsub topics list

# Publish message
gcloud pubsub topics publish TOPIC_NAME --message="Hello World"

# Create subscription
gcloud pubsub subscriptions create SUBSCRIPTION_NAME --topic=TOPIC_NAME

# Pull messages
gcloud pubsub subscriptions pull SUBSCRIPTION_NAME --auto-ack --limit=5

# Delete topic
gcloud pubsub topics delete TOPIC_NAME

# Delete subscription
gcloud pubsub subscriptions delete SUBSCRIPTION_NAME
```

### Cloud Build Quick Commands
```bash
# Submit build
gcloud builds submit --tag=gcr.io/PROJECT_ID/IMAGE_NAME

# List builds
gcloud builds list

# Describe build
gcloud builds describe BUILD_ID

# View logs
gcloud builds log BUILD_ID
```

### Logging and Monitoring Quick Commands
```bash
# Read logs
gcloud logging read "resource.type=gce_instance" --limit=50

# Read logs with filter
gcloud logging read "severity>=ERROR" --limit=20

# Create log sink
gcloud logging sinks create SINK_NAME \
  storage.googleapis.com/BUCKET_NAME \
  --log-filter='resource.type=gce_instance'

# List sinks
gcloud logging sinks list

# Create log metric
gcloud logging metrics create METRIC_NAME \
  --description="Description" \
  --log-filter='severity>=ERROR'
```

---

## 📊 Service Selection Matrix

### Compute Service Selection
| Requirement | Service |
|-------------|---------|
| Full VM control | Compute Engine |
| Containers + Orchestration | GKE |
| Serverless containers | Cloud Run |
| Event-driven functions | Cloud Functions |
| Managed web apps | App Engine |
| Batch processing | Batch |
| High performance computing | Compute Engine (C2/C2D) |

### Storage Service Selection
| Data Type | Service |
|-----------|---------|
| Objects/files | Cloud Storage |
| Block storage | Persistent Disk |
| File storage (NFS) | Filestore |
| Archival | Cloud Storage (Archive) |
| In-memory cache | Memorystore |

### Database Service Selection
| Use Case | Service |
|----------|---------|
| Relational, < 10 TB | Cloud SQL |
| Relational, > 10 TB, global | Cloud Spanner |
| NoSQL, documents | Firestore |
| NoSQL, wide-column, big data | Bigtable |
| Data warehouse | BigQuery |
| In-memory cache | Memorystore (Redis/Memcached) |

### Load Balancer Selection
| Use Case | Type |
|----------|------|
| HTTP/HTTPS, global | HTTP(S) Load Balancer |
| SSL, non-HTTP, global | SSL Proxy Load Balancer |
| TCP, global | TCP Proxy Load Balancer |
| TCP/UDP, regional, high performance | Network Load Balancer |
| Internal services | Internal Load Balancer |

---

## 🔢 Important Port Numbers

| Service | Port | Protocol |
|---------|------|----------|
| SSH | 22 | TCP |
| HTTP | 80 | TCP |
| HTTPS | 443 | TCP |
| RDP | 3389 | TCP |
| MySQL | 3306 | TCP |
| PostgreSQL | 5432 | TCP |
| SQL Server | 1433 | TCP |
| Redis | 6379 | TCP |
| MongoDB | 27017 | TCP |
| SMTP | 25, 587 | TCP |
| DNS | 53 | UDP/TCP |
| ICMP (ping) | N/A | ICMP |

---

## 📐 Common IP Ranges

### Private IP Ranges (RFC 1918)
- 10.0.0.0/8 (10.0.0.0 - 10.255.255.255)
- 172.16.0.0/12 (172.16.0.0 - 172.31.255.255)
- 192.168.0.0/16 (192.168.0.0 - 192.168.255.255)

### Google Private Access
- 199.36.153.8/30 (for Private Google Access)

### Common CIDR Notation
- /32 = 1 IP address
- /24 = 256 IP addresses (254 usable)
- /20 = 4,096 IP addresses
- /16 = 65,536 IP addresses
- /8 = 16,777,216 IP addresses

---

## 🎯 IAM Role Quick Reference

### Primitive Roles (Avoid in Production)
- **roles/viewer**: Read-only access
- **roles/editor**: Viewer + edit resources (no IAM changes)
- **roles/owner**: Editor + manage IAM + billing

### Common Predefined Roles

**Compute Engine:**
- `roles/compute.viewer`: View resources
- `roles/compute.instanceAdmin.v1`: Manage instances
- `roles/compute.networkAdmin`: Manage networks
- `roles/compute.storageAdmin`: Manage disks and images

**Storage:**
- `roles/storage.objectViewer`: Read objects
- `roles/storage.objectCreator`: Create objects
- `roles/storage.objectAdmin`: Full control over objects
- `roles/storage.admin`: Full control over buckets and objects

**GKE:**
- `roles/container.viewer`: View clusters
- `roles/container.developer`: Deploy to clusters
- `roles/container.admin`: Full control

**Cloud SQL:**
- `roles/cloudsql.viewer`: View instances
- `roles/cloudsql.client`: Connect to instances
- `roles/cloudsql.admin`: Full control

**IAM:**
- `roles/iam.serviceAccountUser`: Use service accounts
- `roles/iam.serviceAccountAdmin`: Manage service accounts
- `roles/iam.securityReviewer`: View access policies

---

## 💰 Cost Optimization Quick Tips

1. **Right-size VMs**: Use custom machine types, don't over-provision
2. **Use Preemptible VMs**: For fault-tolerant workloads (80% savings)
3. **Committed Use Discounts**: 1 or 3-year commitments (up to 57% off)
4. **Storage Classes**: Use appropriate class (Nearline, Coldline, Archive)
5. **Auto-scaling**: Scale down when not needed
6. **Stop Unused VMs**: Stop development VMs when not in use
7. **Delete Unattached Disks**: Clean up orphaned resources
8. **Use Sustained Use Discounts**: Automatic discounts for running VMs
9. **Regional vs Multi-regional**: Use regional when multi-regional not needed
10. **Egress Optimization**: Minimize data transfer out of GCP

---

## 🔐 Security Best Practices

1. **IAM**: Use least privilege principle
2. **Service Accounts**: Use for applications, not humans
3. **VPC**: Use custom VPC with specific firewall rules
4. **Private IPs**: Use for internal communication
5. **Encryption**: At rest (automatic), in transit (HTTPS/TLS)
6. **Secrets**: Use Secret Manager, not environment variables
7. **Audit Logs**: Enable and monitor Cloud Audit Logs
8. **Updates**: Keep systems and applications updated
9. **MFA**: Enable for all user accounts
10. **Key Rotation**: Rotate service account keys regularly

---

## 📱 Console Quick Access URLs

- **Console Home**: https://console.cloud.google.com
- **Compute Engine**: https://console.cloud.google.com/compute
- **Cloud Storage**: https://console.cloud.google.com/storage
- **GKE**: https://console.cloud.google.com/kubernetes
- **Cloud SQL**: https://console.cloud.google.com/sql
- **IAM**: https://console.cloud.google.com/iam-admin
- **Networking**: https://console.cloud.google.com/networking
- **Monitoring**: https://console.cloud.google.com/monitoring
- **Billing**: https://console.cloud.google.com/billing
- **Cloud Shell**: Click icon in top-right of console

---

## ⌨️ Cloud Shell Quick Tips

```bash
# Cloud Shell is pre-configured with gcloud, kubectl, gsutil, and more

# Boost Cloud Shell (more CPU/RAM)
# Click on "More" menu in Cloud Shell

# Transfer files to/from Cloud Shell
# Use "Upload File" or "Download File" from menu

# Start code editor
cloudshell edit FILENAME

# Open web preview (for testing web apps)
# Click "Web Preview" button

# Persistent storage
# $HOME is persistent (5GB)
# /tmp is ephemeral
```

---

## 🔄 Common Workflow Patterns

### Deploy a Web Application
1. Create VM or GKE cluster
2. Configure VPC and firewall rules (allow HTTP/HTTPS)
3. Deploy application code
4. Set up Cloud Load Balancer
5. Configure auto-scaling
6. Set up Cloud Monitoring alerts
7. Configure Cloud CDN (optional)

### Secure a Database
1. Create Cloud SQL with private IP
2. Configure VPC and Private Service Access
3. Create database and users
4. Grant IAM roles (cloudsql.client)
5. Use Cloud SQL Proxy for connections
6. Enable automated backups
7. Set up replication (if needed)

### Set Up CI/CD
1. Connect source repository
2. Create Cloud Build trigger
3. Define build steps in cloudbuild.yaml
4. Build container image
5. Push to Artifact Registry
6. Deploy to Cloud Run/GKE
7. Set up monitoring

---

## 🆘 Troubleshooting Quick Checks

### VM Won't Start
- [ ] Check quotas
- [ ] Check IAM permissions
- [ ] Check zone availability
- [ ] Review error message in logs

### Can't SSH into VM
- [ ] Check firewall rules (allow tcp:22)
- [ ] Check VM has external IP or Cloud NAT
- [ ] Verify SSH keys are configured
- [ ] Check if VM is running

### Application Not Accessible
- [ ] Check firewall rules
- [ ] Check load balancer configuration
- [ ] Verify health checks passing
- [ ] Check backend service configuration
- [ ] Verify application is listening on correct port

### Permission Denied Errors
- [ ] Check IAM roles
- [ ] Verify service account permissions
- [ ] Check organization policies
- [ ] Review Cloud Audit Logs

### High Costs
- [ ] Check Billing reports
- [ ] Review running VMs
- [ ] Check storage usage
- [ ] Look for orphaned disks
- [ ] Review BigQuery usage
- [ ] Check egress charges

---

## 📖 Quick Documentation Links

- **gcloud reference**: https://cloud.google.com/sdk/gcloud/reference
- **gsutil reference**: https://cloud.google.com/storage/docs/gsutil
- **kubectl cheatsheet**: https://kubernetes.io/docs/reference/kubectl/cheatsheet/
- **GCP pricing calculator**: https://cloud.google.com/products/calculator
- **GCP free tier**: https://cloud.google.com/free
- **Status dashboard**: https://status.cloud.google.com

---

Keep this guide handy for quick reference during your exam preparation and certification journey! 🚀
