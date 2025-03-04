# High Availability Solution
## Architecture

Please take a look on this file:
[High Availability Architecture](./DevOps%20Challenge.drawio)

## Explanation
### AWS
#### EKS
For easily scaling and managing applications seamlessly without affecting running traffics, can apply different deployment strategy (blue-green, canary,...)

Possible alternatives: ECS Fargate

#### RDS Multi-region
Ensure workloads while in rush hours or processes that requires quick read and write operations.

Possible alternatives: Aurora RDS

#### ElastiCache
For Caching effectively

Possible alternatives: custom-built redis to run inside EKS

#### ECR
For pushing container images for EKS/ECS to pull and run.

Possible alternatives: Self-deploy registry (not recommended as more work to do but don't have much value unless running on-prem without external network connection)

#### SQS
For event-driven architecture with pub-sub mechanism using pull-based

Possible alternatives: custom-built kafka or rabbitmq to run inside EKS

#### Load Balancer (Application and Network)

Application load balancer is for application running only.
Network load balancer is for TCP connection which is required by Kubernetes api-server to successfully connect and register nodes.

Possible alternatives: None

#### Network Interface
Important component to connect services in AWS.

#### Global Accelerator
Increase the high availability by distributing traffics between load balancers and regions.

#### Route53
For DNS Registration

Possible alternatives: GoDaddy, Cloudflare,...

#### ACM (AWS Certificate Manager)
For registering CA certificate for domain name.

Possible alternatives: self-signed certificate or Let's Encrypt (Not recommended)

#### KMS
For managing keys to encrypt between AWS services

### Kubernetes

#### CI/CD
1. Gitlab CI as an agent to run on EKS cluster, this can alternatively replace ArgoCD if in small scale
2. ArgoCD for seamless deployment as it can healthcheck, deploy with specific deployment strategy, tracking components of an application.

#### Monitoring
1. metric-server will be deployed to get the number of system resources.
2. Grafana for monitoring dashboard and can configure alerts,.. and send notifications to different external communication providers like SMS, Teams, Slack,...
3. Prometheus to get the metric from `metric-server` and visualize it
4. Loki for logging and tracing data within the system

## Plan for scaling

As this plan is properly designed right before implementation, however it could cause extra efforts to scale up the systems if the requirements changed (Change cloud provider, move to on-premises,...). It could varies, these are steps that I will approach if plans go differently:

1. Analize existing system and evaluate impacts
2. Identify the requirements
    - System requirements (CPU, RAM,...)
    - Budget
    - Workload (concurrent users, req/s)
    - Timeline (whether scaling fast or having time to investigate)
3. Finding solution (maybe with colleagues)
    - Improve existing system
    - Propose solution and get more feedbacks
    - Do PoC if required
    - Mesure and report of feasibility, learning curve, difficulty,...
4. Apply solution
    - Unify workflow
    - Inform possible impacts to parties
    - Inform mantenance windows
    - Apply and fixing bugs (if any)
    - Continue to actively monitoring