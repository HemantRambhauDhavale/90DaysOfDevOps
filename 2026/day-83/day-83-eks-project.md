# Day 83 — EKS Project: Production Deployment of AI-BankApp

## What I learned

Today I completed the final day of my 3-day EKS block by deploying the complete AI-BankApp on Amazon EKS.

This was not just deploying one application. I connected the complete stack:

* Spring Boot BankApp
* MySQL database
* Ollama AI chatbot
* EBS persistent storage
* Gateway API with Envoy
* HPA autoscaling
* Prometheus + Grafana monitoring
* End-to-end validation
* Complete AWS resource cleanup

## What I did

### 1. Deployed the application stack

I deployed the resources in a proper order:

1. Namespace
2. Persistent Volume and Persistent Volume Claim
3. ConfigMap and Secrets
4. MySQL
5. Ollama
6. BankApp
7. HPA

I used `kubectl wait` to make sure MySQL and Ollama were ready before starting the BankApp.

### 2. Connected the application through Gateway API

I used Envoy Gateway to expose the application and create the traffic path from the internet to the BankApp running inside EKS.

The flow was:

**Internet → NLB → Envoy Gateway → BankApp Service → BankApp Pods**

I tested the Spring Boot health endpoint and opened the application in the browser.

### 3. Tested the complete application

I validated:

* User registration
* Login
* Deposit
* Withdraw
* Transfer
* AI chatbot
* Dark/light mode

The important part for me was seeing the complete application working instead of testing individual Kubernetes resources separately.

### 4. Added monitoring

I deployed the Prometheus and Grafana stack using Helm.

Prometheus collects application and Kubernetes metrics, while Grafana gives a dashboard to understand what is happening inside the cluster.

I also configured a `ServiceMonitor` for the BankApp's `/actuator/prometheus` endpoint.

I checked metrics such as:

* JVM memory usage
* HTTP request rate
* HTTP request latency
* Pod resources
* Node health

### 5. Performed end-to-end validation

I checked all major layers:

**Application**

* Pods running
* Health endpoint working
* HPA active
* Prometheus endpoint working

**Data**

* MySQL healthy
* PVCs bound
* EBS-backed storage available
* Ollama model loaded

**Infrastructure**

* EKS nodes healthy
* Gateway serving traffic
* Monitoring pods running

**Security**

* BankApp running as non-root
* Kubernetes Secret used for sensitive configuration

### 6. Cleaned up AWS resources

After testing, I removed the workloads, monitoring stack, Gateway resources and supporting components.

Finally, I used:

```bash
terraform destroy
```

This was important because leaving EKS, EC2, NAT Gateway, load balancers or EBS resources running can create unnecessary AWS costs.

## What I understood from Day 81–83

| Day    | Main learning                                                                   |
| ------ | ------------------------------------------------------------------------------- |
| Day 81 | Created EKS infrastructure using Terraform and connected kubectl                |
| Day 82 | Worked with Gateway API, Envoy, TLS and EBS storage                             |
| Day 83 | Combined everything into a complete application with monitoring and autoscaling |

## Architecture I understood

```text
                    Internet
                       |
                      NLB
                       |
                Envoy Gateway
                       |
                Gateway API
                       |
                BankApp Service
                       |
              +--------+--------+
              |        |        |
           BankApp   BankApp   BankApp
             Pod      Pod       Pod
               |
        +------+------+
        |             |
      MySQL         Ollama
        |             |
      EBS PVC       EBS PVC
```

Monitoring:

```text
BankApp /actuator/prometheus
            |
       ServiceMonitor
            |
        Prometheus
            |
         Grafana
```

## Key takeaways

* EKS is more than just creating a Kubernetes cluster.
* Persistent applications need proper storage planning.
* Gateway API can provide a clean way to expose applications.
* HPA helps applications respond to changing load.
* Prometheus and Grafana make the cluster observable.
* `kubectl wait` is useful for reliable deployment scripts.
* Cleanup is part of cloud engineering, not an optional step.
* A production deployment needs networking, storage, security, scaling and monitoring together.

## What I would add for a real production environment

* Route 53 + ExternalDNS
* Network Policies
* Pod Disruption Budgets
* External Secrets with AWS Secrets Manager
* Automated database backups
* Centralized logging
* Separate dev and production environments

## Final result

By the end of Day 83, I had a complete AI-powered banking application running on Amazon EKS with:

**Terraform + EKS + Kubernetes + Gateway API + EBS + HPA + Prometheus + Grafana + Spring Boot + MySQL + Ollama**

The biggest learning for me was not one individual command. It was understanding how all these DevOps pieces connect to run a real application.

#90DaysOfDevOps #DevOpsKaJosh
