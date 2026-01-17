# Important Labs for Google Cloud Associate Cloud Engineer

## 🎯 Essential Hands-On Labs

This document contains the most important labs you should complete to prepare for the Google Cloud Associate Cloud Engineer certification exam.

## 🆓 Free Resources for Labs

### Google Cloud Skills Boost (Qwiklabs)
- **URL:** https://www.cloudskillsboost.google/
- **Free Credits:** Sign up for free credits and monthly challenges
- **Recommended Paths:**
  - Google Cloud Essentials
  - Baseline: Infrastructure
  - Cloud Engineering

### Google Cloud Free Tier
- **URL:** https://cloud.google.com/free
- **$300 Free Credit:** Valid for 90 days for new users
- **Always Free:** 20+ products with free usage limits

---

## 📝 Lab Categories and Priority

### ⭐ Priority 1: Must Complete Labs

These labs cover the most critical exam topics and should be completed first.

#### 1. Compute Engine Fundamentals

**Lab: Creating and Managing Virtual Machines**
- Create a VM using Cloud Console
- Create a VM using gcloud command line
- SSH into a VM
- Install and configure a web server
- Create a custom image
- Create an instance template
- Create a managed instance group

**Practice Tasks:**
```bash
# Create a VM with specific configuration
gcloud compute instances create webserver \
  --zone=us-central1-a \
  --machine-type=e2-medium \
  --tags=http-server \
  --metadata=startup-script='#!/bin/bash
    apt-get update
    apt-get install -y apache2
    echo "Hello from GCP" > /var/www/html/index.html'

# Create instance template
gcloud compute instance-templates create web-template \
  --machine-type=e2-small \
  --tags=http-server \
  --image-family=debian-11 \
  --image-project=debian-cloud

# Create managed instance group
gcloud compute instance-groups managed create web-group \
  --template=web-template \
  --size=3 \
  --zone=us-central1-a
```

**Key Learning Points:**
- VM creation and configuration
- Startup scripts
- Instance templates
- Managed instance groups
- Autoscaling configuration

---

#### 2. Cloud Storage Operations

**Lab: Working with Cloud Storage**
- Create buckets with different storage classes
- Upload and download objects
- Set up lifecycle policies
- Configure bucket permissions
- Enable versioning
- Create signed URLs

**Practice Tasks:**
```bash
# Create buckets
gsutil mb -c STANDARD -l us-central1 gs://my-standard-bucket
gsutil mb -c NEARLINE -l us-central1 gs://my-nearline-bucket

# Upload files
gsutil cp local-file.txt gs://my-standard-bucket/

# Set lifecycle policy
cat > lifecycle.json << EOF
{
  "lifecycle": {
    "rule": [
      {
        "action": {"type": "SetStorageClass", "storageClass": "NEARLINE"},
        "condition": {"age": 30}
      },
      {
        "action": {"type": "Delete"},
        "condition": {"age": 365}
      }
    ]
  }
}
EOF
gsutil lifecycle set lifecycle.json gs://my-standard-bucket

# Make object public
gsutil acl ch -u AllUsers:R gs://my-standard-bucket/public-file.txt

# Enable versioning
gsutil versioning set on gs://my-standard-bucket
```

**Key Learning Points:**
- Storage class selection
- Object lifecycle management
- ACL and IAM permissions
- Versioning
- gsutil commands

---

#### 3. VPC Networking Configuration

**Lab: Setting Up VPC Networks**
- Create custom VPC networks
- Create subnets in different regions
- Configure firewall rules
- Set up Cloud NAT
- Configure Private Google Access

**Practice Tasks:**
```bash
# Create custom VPC
gcloud compute networks create custom-vpc --subnet-mode=custom

# Create subnets
gcloud compute networks subnets create subnet-us \
  --network=custom-vpc \
  --range=10.1.0.0/24 \
  --region=us-central1 \
  --enable-private-ip-google-access

gcloud compute networks subnets create subnet-europe \
  --network=custom-vpc \
  --range=10.2.0.0/24 \
  --region=europe-west1

# Create firewall rules
gcloud compute firewall-rules create allow-ssh \
  --network=custom-vpc \
  --allow=tcp:22 \
  --source-ranges=0.0.0.0/0

gcloud compute firewall-rules create allow-http \
  --network=custom-vpc \
  --allow=tcp:80,tcp:443 \
  --target-tags=web-server

# Create Cloud Router for NAT
gcloud compute routers create nat-router \
  --network=custom-vpc \
  --region=us-central1

# Configure Cloud NAT
gcloud compute routers nats create nat-config \
  --router=nat-router \
  --region=us-central1 \
  --auto-allocate-nat-external-ips \
  --nat-all-subnet-ip-ranges
```

**Key Learning Points:**
- VPC network creation
- Subnet management
- Firewall rule configuration
- Cloud NAT setup
- Private Google Access

---

#### 4. IAM and Service Accounts

**Lab: Managing IAM Permissions**
- Assign IAM roles to users
- Create service accounts
- Generate service account keys
- Use service accounts with VMs
- Implement least privilege principle

**Practice Tasks:**
```bash
# Grant IAM role to user
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member=user:user@example.com \
  --role=roles/compute.instanceAdmin.v1

# Create service account
gcloud iam service-accounts create vm-service-account \
  --display-name="VM Service Account"

# Grant role to service account
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member=serviceAccount:vm-service-account@PROJECT_ID.iam.gserviceaccount.com \
  --role=roles/storage.objectViewer

# Create VM with service account
gcloud compute instances create vm-with-sa \
  --service-account=vm-service-account@PROJECT_ID.iam.gserviceaccount.com \
  --scopes=https://www.googleapis.com/auth/cloud-platform

# List IAM policies
gcloud projects get-iam-policy PROJECT_ID
```

**Key Learning Points:**
- IAM role types and selection
- Service account creation and management
- Principle of least privilege
- Service account impersonation
- IAM policy binding

---

#### 5. Google Kubernetes Engine (GKE)

**Lab: Deploying Applications on GKE**
- Create a GKE cluster
- Deploy containerized applications
- Expose services using LoadBalancer
- Scale deployments
- Upgrade cluster

**Practice Tasks:**
```bash
# Create GKE cluster
gcloud container clusters create demo-cluster \
  --zone=us-central1-a \
  --num-nodes=3 \
  --enable-autoscaling \
  --min-nodes=1 \
  --max-nodes=5

# Get credentials
gcloud container clusters get-credentials demo-cluster --zone=us-central1-a

# Deploy application
kubectl create deployment nginx --image=nginx:latest

# Expose service
kubectl expose deployment nginx --type=LoadBalancer --port=80

# Scale deployment
kubectl scale deployment nginx --replicas=5

# Autoscale based on CPU
kubectl autoscale deployment nginx --cpu-percent=50 --min=1 --max=10

# Update image
kubectl set image deployment/nginx nginx=nginx:1.21

# View resources
kubectl get pods
kubectl get services
kubectl get deployments
kubectl describe pod POD_NAME
```

**Key Learning Points:**
- GKE cluster creation
- kubectl commands
- Deployment management
- Service exposure
- Horizontal pod autoscaling
- Rolling updates

---

### ⭐ Priority 2: Important Labs

#### 6. Cloud Load Balancing

**Lab: Setting Up HTTP(S) Load Balancer**
- Create instance groups in multiple regions
- Configure backend service
- Set up URL maps
- Configure health checks
- Enable Cloud CDN

**Practice Tasks:**
```bash
# Create instance template
gcloud compute instance-templates create lb-backend-template \
  --machine-type=e2-small \
  --tags=http-server \
  --metadata=startup-script='#!/bin/bash
    apt-get update
    apt-get install -y apache2
    echo "Hello from $(hostname)" > /var/www/html/index.html'

# Create instance groups in multiple zones
gcloud compute instance-groups managed create lb-backend-group-us \
  --template=lb-backend-template \
  --size=2 \
  --zone=us-central1-a

gcloud compute instance-groups managed set-named-ports lb-backend-group-us \
  --named-ports=http:80 \
  --zone=us-central1-a

# Create health check
gcloud compute health-checks create http http-basic-check \
  --port=80

# Create backend service
gcloud compute backend-services create web-backend-service \
  --protocol=HTTP \
  --health-checks=http-basic-check \
  --global

# Add backend
gcloud compute backend-services add-backend web-backend-service \
  --instance-group=lb-backend-group-us \
  --instance-group-zone=us-central1-a \
  --global

# Create URL map
gcloud compute url-maps create web-map \
  --default-service=web-backend-service

# Create HTTP proxy
gcloud compute target-http-proxies create http-lb-proxy \
  --url-map=web-map

# Create forwarding rule
gcloud compute forwarding-rules create http-content-rule \
  --global \
  --target-http-proxy=http-lb-proxy \
  --ports=80
```

**Key Learning Points:**
- Load balancer types
- Backend services
- Health checks
- URL maps
- Traffic distribution

---

#### 7. Cloud SQL Management

**Lab: Setting Up Cloud SQL**
- Create Cloud SQL instance
- Configure high availability
- Create databases and users
- Set up Cloud SQL Proxy
- Implement backups

**Practice Tasks:**
```bash
# Create Cloud SQL instance
gcloud sql instances create mysql-instance \
  --database-version=MYSQL_8_0 \
  --tier=db-n1-standard-1 \
  --region=us-central1 \
  --backup \
  --backup-start-time=03:00

# Set root password
gcloud sql users set-password root \
  --host=% \
  --instance=mysql-instance \
  --password=YOUR_PASSWORD

# Create database
gcloud sql databases create mydb --instance=mysql-instance

# Create user
gcloud sql users create dbuser \
  --instance=mysql-instance \
  --password=USER_PASSWORD

# Configure high availability
gcloud sql instances patch mysql-instance --availability-type=REGIONAL

# Create read replica
gcloud sql instances create mysql-replica \
  --master-instance-name=mysql-instance \
  --tier=db-n1-standard-1 \
  --region=us-east1
```

**Key Learning Points:**
- Instance creation and configuration
- HA configuration
- Backup and restore
- Read replicas
- Cloud SQL Proxy

---

#### 8. Cloud Functions

**Lab: Deploying Serverless Functions**
- Create HTTP-triggered functions
- Create Cloud Storage-triggered functions
- Create Pub/Sub-triggered functions
- Configure environment variables
- Monitor function execution

**Practice Tasks:**

**HTTP Function (Python):**
```python
# main.py
def hello_http(request):
    name = request.args.get('name', 'World')
    return f'Hello, {name}!'
```

```bash
# Deploy HTTP function
gcloud functions deploy hello_http \
  --runtime=python39 \
  --trigger-http \
  --allow-unauthenticated \
  --entry-point=hello_http

# Call function
curl https://REGION-PROJECT_ID.cloudfunctions.net/hello_http?name=GCP
```

**Storage Trigger (Python):**
```python
# main.py
def process_file(event, context):
    file_name = event['name']
    print(f'Processing file: {file_name}')
```

```bash
# Deploy storage-triggered function
gcloud functions deploy process_file \
  --runtime=python39 \
  --trigger-resource=my-bucket \
  --trigger-event=google.storage.object.finalize
```

**Key Learning Points:**
- Function deployment
- Trigger types
- Runtime selection
- Environment variables
- Function logs

---

#### 9. Cloud Pub/Sub Messaging

**Lab: Implementing Pub/Sub**
- Create topics
- Create subscriptions
- Publish messages
- Pull and acknowledge messages
- Set up push subscriptions

**Practice Tasks:**
```bash
# Create topic
gcloud pubsub topics create orders

# Create pull subscription
gcloud pubsub subscriptions create orders-subscription \
  --topic=orders

# Publish messages
gcloud pubsub topics publish orders --message="Order 1001"
gcloud pubsub topics publish orders --message="Order 1002"

# Pull messages
gcloud pubsub subscriptions pull orders-subscription --auto-ack --limit=5

# Create push subscription
gcloud pubsub subscriptions create orders-push \
  --topic=orders \
  --push-endpoint=https://example.com/push
```

**Key Learning Points:**
- Topic and subscription creation
- Publishing messages
- Pull and push subscriptions
- Message acknowledgment
- Dead letter topics

---

#### 10. Cloud Monitoring and Logging

**Lab: Setting Up Monitoring and Alerts**
- Create custom dashboards
- Set up alerting policies
- Create log-based metrics
- Export logs
- Create uptime checks

**Practice Tasks:**
```bash
# Create uptime check
gcloud monitoring uptime create my-uptime-check \
  --resource-type=uptime-url \
  --host=example.com \
  --path=/

# Read logs
gcloud logging read "resource.type=gce_instance AND severity>=ERROR" \
  --limit=50 \
  --format=json

# Create log sink to BigQuery
gcloud logging sinks create my-bq-sink \
  bigquery.googleapis.com/projects/PROJECT_ID/datasets/my_dataset \
  --log-filter='resource.type="gce_instance"'

# Create log-based metric
gcloud logging metrics create error_count \
  --description="Count of error logs" \
  --log-filter='severity>=ERROR'
```

**Key Learning Points:**
- Dashboard creation
- Alert policies
- Log filtering
- Log sinks
- Uptime monitoring

---

### ⭐ Priority 3: Good to Know Labs

#### 11. Cloud Run Deployment

**Lab: Deploying Containers to Cloud Run**
```bash
# Build container
gcloud builds submit --tag gcr.io/PROJECT_ID/myapp

# Deploy to Cloud Run
gcloud run deploy myapp \
  --image gcr.io/PROJECT_ID/myapp \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated \
  --memory 512Mi \
  --cpu 1 \
  --max-instances 10
```

#### 12. Cloud Deployment Manager

**Lab: Infrastructure as Code with Deployment Manager**
```yaml
# config.yaml
resources:
- name: vm-instance
  type: compute.v1.instance
  properties:
    zone: us-central1-a
    machineType: zones/us-central1-a/machineTypes/e2-medium
    disks:
    - deviceName: boot
      type: PERSISTENT
      boot: true
      autoDelete: true
      initializeParams:
        sourceImage: projects/debian-cloud/global/images/family/debian-11
    networkInterfaces:
    - network: global/networks/default
```

```bash
# Deploy
gcloud deployment-manager deployments create my-deployment --config config.yaml

# Update
gcloud deployment-manager deployments update my-deployment --config config.yaml

# Delete
gcloud deployment-manager deployments delete my-deployment
```

#### 13. Cloud VPN Setup

**Lab: Connecting Networks with Cloud VPN**
```bash
# Create VPN gateway
gcloud compute target-vpn-gateways create vpn-gateway \
  --network=my-network \
  --region=us-central1

# Create forwarding rules
gcloud compute forwarding-rules create vpn-rule-esp \
  --region=us-central1 \
  --ip-protocol=ESP \
  --target-vpn-gateway=vpn-gateway

# Create VPN tunnel
gcloud compute vpn-tunnels create tunnel1 \
  --peer-address=PEER_IP \
  --shared-secret=SECRET \
  --target-vpn-gateway=vpn-gateway \
  --region=us-central1
```

---

## 🎓 Recommended Lab Learning Paths

### Week 1-2: Core Infrastructure
1. Creating VMs (Compute Engine)
2. Cloud Storage operations
3. VPC networking basics
4. IAM and service accounts

### Week 3-4: Advanced Services
5. GKE deployment and management
6. Cloud Load Balancing
7. Cloud SQL setup
8. Cloud Functions

### Week 5-6: Operations and Integration
9. Cloud Pub/Sub
10. Monitoring and Logging
11. Cloud Run
12. Infrastructure as Code

---

## 📚 Additional Practice Resources

### Official Google Courses
1. **Google Cloud Skills Boost - Associate Cloud Engineer Path**
   - Comprehensive labs aligned with exam topics
   - Approximately 40-50 hours of content

2. **Coursera - Architecting with Google Compute Engine Specialization**
   - 6-course series with hands-on labs
   - Covers all exam domains

### Practice Exams
1. Official Google Practice Exam (free)
2. Whizlabs Practice Tests
3. Tutorials Dojo Practice Tests

### Community Resources
1. **GitHub:** Search for "GCP Associate Cloud Engineer labs"
2. **Medium:** Many developers share lab walkthroughs
3. **YouTube:** Official Google Cloud YouTube channel

---

## ✅ Lab Completion Checklist

Track your progress:

**Core Infrastructure Labs:**
- [ ] Create and manage VMs
- [ ] Configure Cloud Storage
- [ ] Set up VPC networking
- [ ] Configure IAM and service accounts
- [ ] Deploy GKE cluster

**Advanced Services Labs:**
- [ ] Configure Load Balancing
- [ ] Set up Cloud SQL
- [ ] Deploy Cloud Functions
- [ ] Implement Pub/Sub messaging
- [ ] Set up monitoring and logging

**Optional Advanced Labs:**
- [ ] Deploy to Cloud Run
- [ ] Use Deployment Manager
- [ ] Configure Cloud VPN
- [ ] Set up Cloud CDN
- [ ] Implement Cloud Armor

---

## 💡 Tips for Labs

1. **Use Cloud Shell:** Most labs can be done entirely in Cloud Shell
2. **Take Notes:** Document commands and configurations that work
3. **Clean Up Resources:** Always delete resources after labs to avoid charges
4. **Make Mistakes:** Breaking things is part of learning - experiment!
5. **Time Yourself:** Practice completing labs within time limits
6. **Understand, Don't Memorize:** Focus on understanding concepts, not just commands
7. **Use Free Tier:** Maximize the $300 free credit and always-free tier

---

## 🧹 Cleanup Commands

Always clean up after labs:
```bash
# Delete VMs
gcloud compute instances delete INSTANCE_NAME --zone=ZONE

# Delete instance groups
gcloud compute instance-groups managed delete GROUP_NAME --zone=ZONE

# Delete buckets
gsutil rm -r gs://BUCKET_NAME

# Delete GKE cluster
gcloud container clusters delete CLUSTER_NAME --zone=ZONE

# Delete Cloud SQL instance
gcloud sql instances delete INSTANCE_NAME

# Delete Cloud Functions
gcloud functions delete FUNCTION_NAME

# List all resources
gcloud compute instances list
gcloud compute disks list
gcloud storage ls
```

Good luck with your hands-on practice! 🚀
