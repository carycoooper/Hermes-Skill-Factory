# Hermes-Skill-Cloud-Architect

> ☁️ **企业级云架构设计专家** | AWS/Azure/GCP 多云策略 | 成本优化与安全合规

---

## 📋 技能概述

Hermes-Skill-Cloud-Architect 是一个专业的云架构 AI 助手，专注于提供企业级云基础设施的设计、迁移、优化和运维解决方案。支持 AWS、Azure、GCP 等主流云平台，涵盖微服务架构、Serverless、容器化、DevOps 等现代云原生技术栈。

### 核心能力矩阵

```
┌─────────────────────────────────────────────────────────────┐
│                   Cloud Architecture Stack                  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ☁️ Cloud Platforms  │  🏗️ Architecture    │  🔒 Security   │
│  ├── AWS            │  ├── Microservices   │  ├── IAM       │
│  ├── Azure          │  ├── Serverless      │  ├── KMS       │
│  ├── GCP            │  ├── Event-Driven    │  ├── Secrets   │
│  └── Multi-Cloud    │  └── Space-Based     │  └── Compliance│
│                                                             │
│  📦 Containers      │  🔄 DevOps           │  💰 Cost Mgmt  │
│  ├── Docker         │  ├── CI/CD           │  ├── Rightsizing│
│  ├── Kubernetes     │  ├── IaC             │  ├── Reserved  │
│  ├── ECS/EKS        │  ├── GitOps          │  └── Spot/Preemptible│
│  └── Service Mesh   │  └── Observability   │                 │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚀 快速开始

### 架构评估

```bash
# 云平台选型咨询
/cloud-arch recommend "e-commerce platform with 100K daily users"

# 架构审查
/cloud-arch review "current architecture diagram"

# 成本估算
/cloud-arch estimate "3-tier web app with RDS, Redis, S3"

# 迁移规划
/cloud-arch migrate "on-premise data center to AWS"
```

### IaC (Infrastructure as Code) 生成

```bash
# Terraform 模块生成
/cloud-arch terraform "VPC with public/private subnets"

# CloudFormation 模板
/cloud-arch cloudformation "serverless API with DynamoDB"

# ARM 模板 (Azure)
/cloud-arch arm-template "AKS cluster with ACR integration"
```

---

## 🏗️ 架构模式库

### 1. 微服务架构

```
┌─────────────────────────────────────────────────────────────┐
│              Microservices Architecture                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Client Layer                                               │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐                    │
│  │ Web App │  │ Mobile  │  │  Third  │                    │
│  │ (React) │  │ (iOS)   │  │ Party   │                    │
│  └────┬────┘  └────┬────┘  └────┬────┘                    │
│       │            │            │                          │
│  ┌────▼────────────▼────────────▼────┐                    │
│  │         API Gateway (ALB/Nginx)    │                    │
│  └────────────────┬───────────────────┘                    │
│                   │                                        │
│  ┌────────────────▼────────────────────────────────────┐  │
│  │              Service Mesh (Istio/Linkerd)            │  │
│  │  ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐      │  │
│  │  │ User   │ │ Order  │ │Payment │ │Product │      │  │
│  │  │Service │ │Service │ │Service │ │Service │      │  │
│  │  └───┬────┘ └───┬────┘ └───┬────┘ └───┬────┘      │  │
│  └──────┼──────────┼──────────┼──────────┼───────────┘  │
│         │          │          │          │               │
│  ┌──────▼──────────▼──────────▼──────────▼───────────┐  │
│  │              Data Layer                           │  │
│  │  PostgreSQL │ MongoDB │ Redis │ Elasticsearch     │  │
│  └───────────────────────────────────────────────────┘  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### Terraform 实现

```hcl
# main.tf - 微服务基础设施

provider "aws" {
  region = var.aws_region
}

module "vpc" {
  source = "./modules/vpc"
  
  cidr_block           = "10.0.0.0/16"
  availability_zones   = ["us-east-1a", "us-east-1b"]
  environment          = var.environment
  enable_nat_gateway   = true
  single_nat_gateway   = var.environment == "dev"
}

module "eks" {
  source = "./modules/eks"
  
  cluster_name    = "${var.project_name}-${var.environment}"
  vpc_id          = module.vpc.vpc_id
  subnet_ids      = module.vpc.private_subnet_ids
  
  node_groups = {
    general = {
      instance_types = ["m5.large", "m5.xlarge"]
      desired_size   = 3
      min_size       = 2
      max_size       = 10
      
      labels = {
        workload = "general"
      }
    }
    
    memory_optimized = {
      instance_types = ["r5.xlarge", "r5.2xlarge"]
      desired_size   = 2
      min_size       = 1
      max_size       = 5
      
      labels = {
        workload = "memory-intensive"
      }
    }
  }
}

module "rds" {
  source = "./modules/rds"
  
  identifier     = "${var.project_name}-postgres"
  engine         = "postgres"
  engine_version = "15.4"
  instance_class = "db.r6g.xlarge"
  
  allocated_storage     = 100
  max_allocated_storage = 500
  storage_encrypted     = true
  
  multi_az               = var.environment == "prod"
  db_subnet_group_name   = module.vpc.database_subnet_group_name
  vpc_security_group_ids = [module.security.sg_rds_id]
}
```

### 2. Serverless 架构

```yaml
# serverless.yml - Serverless Framework
service: hermes-api

frameworkVersion: '3'

provider:
  name: aws
  runtime: nodejs18.x
  region: us-east-1
  stage: ${opt:stage, 'dev'}
  environment:
    TABLE_NAME: ${self:service}-${opt:stage, 'dev'}
    SECRET_ARN: !Ref ApiSecretArn
  iam:
    role:
      statements:
        - Effect: Allow
          Action:
            - dynamodb:Query
            - dynamodb:Scan
            - dynamodb:GetItem
            - dynamodb:PutItem
            - dynamodb:UpdateItem
            - dynamodb:DeleteItem
          Resource: "arn:aws:dynamodb:${self:provider.region}:*:table/${self:service}-${opt:stage, 'dev'}"

functions:
  api:
    handler: src/handlers/api.handler
    events:
      - httpApi:
          path: /api/{proxy+}
          method: ANY
  
  webhookProcessor:
    handler: src/handlers/webhook.handler
    events:
      - schedule: rate(5 minutes)
    
resources:
  Resources:
    ApiTable:
      Type: AWS::DynamoDB::Table
      Properties:
        TableName: ${self:service}-${opt:stage, 'dev'}
        BillingMode: PAY_PER_REQUEST
        AttributeDefinitions:
          - AttributeName: PK
            AttributeType: S
          - AttributeName: SK
            AttributeType: S
        KeySchema:
          - AttributeName: PK
            KeyType: HASH
          - AttributeName: SK
            KeyType: RANGE
        GlobalSecondaryIndexes:
          - IndexName: GSI1
            KeySchema:
              - AttributeName: SK
                KeyType: HASH
              - AttributeName: PK
                KeyType: RANGE
            Projection:
              ProjectionType: ALL
```

### 3. Event-Driven 架构

```python
# event_bridge_architecture.py - AWS EventBridge 设计
import boto3
from aws_cdk import (
    core,
    aws_events as events,
    aws_events_targets as targets,
    aws_lambda as lambda_,
    aws_sqs as sqs,
    aws_stepfunctions as sfn
)

class EventDrivenArchitecture(core.Stack):
    def __init__(self, scope: core.Construct, id: str, **kwargs):
        super().__init__(scope, id, **kwargs)
        
        # Event Bus
        bus = events.EventBus(
            self,
            "HermesEventBus",
            event_bus_name="hermes-bus"
        )
        
        # Rules
        order_created_rule = events.Rule(
            self,
            "OrderCreatedRule",
            event_bus=bus,
            event_pattern=events.EventPattern(
                source=["order.service"],
                detail_type=["OrderCreated"],
                detail={
                    "amount": [{"numeric": [">", 0]}]
                }
            )
        )
        
        # Targets
        inventory_lambda = lambda_.Function(
            self,
            "InventoryHandler",
            runtime=lambda_.Runtime.PYTHON_3_11,
            handler="inventory.handler",
            code=lambda_.Code.from_asset("lambda/inventory")
        )
        
        dlq = sqs.Queue(self, "DeadLetterQueue")
        
        # Connect Rule → Lambda with DLQ
        order_created_rule.add_target(
            targets.LambdaFunction(
                inventory_lambda,
                dead_letter_queue=dlq,
                max_event_age=core.Duration.hours(1),
                retry_attempts=3
            )
        )
```

---

## 🔒 安全与合规

### IAM 最佳实践

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::hermes-app-${aws:PrincipalTag/Environment}/*",
        "arn:aws:s3:::hermes-app-${aws:PrincipalTag/Environment}"
      ],
      "Condition": {
        "StringEquals": {
          "aws:PrincipalTag/Department": "engineering",
          "s3:x-amz-server-side-encryption": "aws:kms"
        },
        "IpAddress": {
          "aws:SourceIp": [
            "192.168.1.0/24",
            "10.0.0.0/8"
          ]
        }
      }
    }
  ]
}
```

### 加密策略

```python
# encryption_config.py - 全面的加密配置
import boto3

kms_client = boto3.client('kms')

def create_customer_managed_key():
    """创建客户管理的 KMS 密钥"""
    response = kms_client.create_key(
        Description='Hermes Application Encryption Key',
        KeyUsage='ENCRYPT_DECRYPT',
        Origin='AWS_KMS',
        MultiRegion=True,
        
        Policy={
            'Version': '2012-10-17',
            'Statement': [
                {
                    'Sid': 'EnableAdminAccess',
                    'Effect': 'Allow',
                    'Principal': {'AWS': f'arn:aws:iam::{account_id}:root'},
                    'Action': 'kms:*',
                    'Resource': '*'
                },
                {
                    'Sid': 'AllowApplicationUse',
                    'Effect': 'Allow',
                    'Principal': {
                        'AWS': f'arn:aws:iam::{account_id}:role/hermes-app-role'
                    },
                    'Action': [
                        'kms:Encrypt',
                        'kms:Decrypt',
                        'kms:ReEncrypt*',
                        'kms:GenerateDataKey*',
                        'kms:DescribeKey'
                    ],
                    'Resource': '*',
                    'Condition': {
                        'StringEquals': {
                            'kms:CallerAccount': account_id,
                            'kms:ViaService': 'dynamodb.us-east-1.amazonaws.com'
                        }
                    }
                }
            ]
        }
    )
    
    # 启用自动密钥轮换
    kms_client.enable_key_rotation(KeyId=response['KeyMetadata']['KeyId'])
    
    return response['KeyMetadata']['KeyId']
```

---

## 💰 成本优化策略

### Right-Sizing 建议

```bash
# EC2 实例推荐工具输出示例
Instance Recommendation Report
==============================

Current Instance: m5.xlarge (4 vCPU, 16 GiB RAM)
Monthly Cost: $153.00

Recommendations:
┌─────────────────┬──────────┬──────────┬─────────────┬────────────┐
│ Instance Type   │ vCPU     │ RAM      │ Monthly Cost│ Savings %  │
├─────────────────┼──────────┼──────────┼─────────────┼────────────┤
│ m5.large        │ 2        │ 8 GiB    │ $76.80      │ 49.8% ↓    │
│ m6i.large       │ 2        │ 8 GiB    │ $72.60      │ 52.6% ↓    │
│ t3.large        │ 2        │ 8 GiB    │ $58.40      │ 61.8% ↓    │
│ (Spot) m5.large │ 2        │ 8 GiB    │ $23.04      │ 84.9% ↓    │
└─────────────────┴──────────┴──────────┴─────────────┴────────────┘

CloudWatch Metrics Analysis:
- CPU Utilization Avg: 12%
- Memory Utilization Avg: 35%
- Network In/Out: Low

⚠️ Recommendation: Downsize to t3.large or use Graviton2 (m6i)
```

### Reserved Instances & Savings Plans

```python
# cost_optimizer.py - 成本优化自动化
import boto3

class CloudCostOptimizer:
    def __init__(self):
        self.ce_client = boto3.client('ce')
        self.ec2_client = boto3.client('ec2')
    
    def analyze_reserved_instance_opportunities(self):
        """分析 RI 购买机会"""
        
        # 获取过去 90 天的使用数据
        end_date = datetime.now()
        start_date = end_date - timedelta(days=90)
        
        response = self.ce.get_cost_and_usage(
            TimePeriod={
                'Start': start_date.strftime('%Y-%m-%d'),
                'End': end_date.strftime('%Y-%m-%d')
            },
            Granularity='DAILY',
            Metrics=['UnblendedCost', 'UsageQuantity'],
            GroupBy=[
                {'Type': 'DIMENSION', 'Key': 'INSTANCE_TYPE'},
                {'Type': 'DIMENSION', 'Key': 'PLATFORM'}
            ]
        )
        
        ri_opportunities = []
        
        for result in response['ResultsByTime']:
            for group in result['Groups']:
                instance_type = group['Keys'][0]
                on_demand_cost = float(group['Metrics']['UnblendedCost']['Amount'])
                
                if on_demand_cost > 100:  # 只分析成本 > $100 的实例
                    
                    # 计算 RI 节省（假设 1 年期标准 RI）
                    ri_cost = on_demand_cost * 0.4  # 约 40% 折扣
                    savings = on_demand_cost - ri_cost
                    
                    if savings > 50:  # 节省 > $50 才推荐
                        ri_opportunities.append({
                            'instance_type': instance_type,
                            'on_demand_monthly': on_demand_cost,
                            'ri_monthly': ri_cost,
                            'potential_savings': savings,
                            'savings_percentage': (savings / on_demand_cost) * 100
                        })
        
        return sorted(ri_opportunities, 
                     key=lambda x: x['potential_savings'], 
                     reverse=True)
```

---

## 📊 监控与可观测性

### CloudWatch Dashboard 配置

```json
{
  "widgets": [
    {
      "type": "metric",
      "width": 12,
      "properties": {
        "metrics": [
          ["AWS/Lambda", "Duration", "FunctionName", "api-handler", {"stat": "Average"}],
          [".", "Errors", ".", "."],
          [".", "Throttles", "."],
          [".", "Invocations", ".", {"stat": "Sum"}]
        ],
        "period": 300,
        "stat": "Average",
        "region": "us-east-1",
        "title": "API Handler Performance",
        "view": "timeSeries"
      }
    },
    {
      "type": "alarm",
      "width": 6,
      "properties": {
        "alarms": [
          "HighErrorRate-Alarm",
          "HighLatency-Alarm",
          "LowAvailability-Alarm"
        ]
      }
    }
  ]
}
```

### 分布式追踪

```python
# xray_tracing.py - AWS X-Ray 集成
from aws_xray_sdk.core import xray_recorder
from aws_xray_sdk.ext.flask_middleware import XRayMiddleware

app = Flask(__name__)
XRayMiddleware(app, xray_recorder)

@app.route('/api/orders')
@xray_recorder.capture('orders_handler')
def get_orders():
    subsegment = xray_recorder.begin_subsegment('database_query')
    
    try:
        orders = db.query("SELECT * FROM orders")
        subsegment.put_metadata('query_result', {
            'count': len(orders),
            'execution_time_ms': subsegment.end_time - subsegment.start_time
        })
        return jsonify(orders)
    except Exception as ex:
        subsegment.add_error(ex)
        raise
    finally:
        xray_recorder.end_subsegment()
```

---

## 🔄 多云与混合云策略

### Kubernetes 跨云部署

```yaml
# kubernetes-multi-cloud.yaml
apiVersion: cluster.x-k8s.io/v1beta1
kind: Cluster
metadata:
  name: hermes-multi-cloud
  namespace: default
spec:
  clusterNetwork:
    pods:
      cidrBlocks:
        - 192.168.0.0/16
  controlPlaneRef:
    kind: AWSClusterControlPlane
    name: aws-control-plane
  infrastructureRef:
    kind: AWSCluster
    name: aws-infrastructure
---
apiVersion: cluster.x-k8s.io/v1beta1
kind: MachineDeployment
metadata:
  name: aws-worker-nodes
spec:
  clusterName: hermes-multi-cloud
  replicas: 3
  template:
    spec:
      version: "v1.28.0"
      bootstrap:
        configRef:
          kind: KubeadmConfigTemplate
          name: worker-config
      infrastructureRef:
        kind: AWSMachineTemplate
        name: worker-machine-template
```

---

## 🚨 灾难恢复

### RPO/RTO 目标实现

```
┌─────────────────────────────────────────────────────────────┐
│              Disaster Recovery Strategy                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Tier 1: Critical Systems (RPO < 1hr, RTO < 4hr)           │
│  ├── Multi-AZ Deployment                                   │
│  ├── Cross-Region Replication (Async)                      │
│  ├── Automated Failover                                    │
│  └── Pilot Light DR Environment                            │
│                                                             │
│  Tier 2: Business Essential (RPO < 4hr, RTO < 12hr)        │
│  ├── Backup to S3/GCS (Daily + Incremental)                │
│  ├── Infrastructure as Code (Terraform State Backup)       │
│  └── Warm Standby (Pilot Light + Some Pre-provisioned)     │
│                                                             │
│  Tier 3: Non-Critical (RPO < 24hr, RTO < 48hr)            │
│  ├── Backups Only                                          │
│  ├── IaC for Quick Rebuild                                 │
│  └── Documented Runbook                                     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 📈 容量规划

### 自动扩缩容配置

```yaml
# autoscaling_policy.yaml
Type: AWS::AutoScaling::ScalingPolicy
Properties:
  AutoScalingGroupName: !Ref WebServerGroup
  PolicyType: TargetTrackingScaling
  TargetTrackingConfiguration:
    PredefinedMetricSpecification:
      PredefinedMetricType: ASGAverageCPUUtilization
    TargetValue: 65.0
---
Type: AWS::ApplicationAutoScaling::ScalingPolicy
Properties:
  ServiceNamespace: dynamodb
  ResourceId: table/OrdersTable
  ScalableDimension: dynamodb:table:ReadCapacityUnits
  PolicyType: TargetTrackingScaling
  TargetTrackingConfiguration:
    TargetValue: 70.0
    PredefinedMetricSpecification:
      PredefinedMetricType: DynamoDBReadCapacityUtilization
    ScaleInCooldown: 60
    ScaleOutCooldown: 60
```

---

## 🔗 相关技能

- [Hermes-Skill-CI-CD-Pipeline-Manager](../devops/cicd-pipeline-manager/SKILL.md) - CI/CD 流水线管理
- [Hermes-Skill-Cybersecurity-Auditor](../security/cybersecurity-auditor/SKILL.md) - 云安全审计
- [Hermes-Skill-Knowledge-Base-Manager](../knowledge/knowledge-base-manager/SKILL.md) - 架构文档管理

---

## 📊 技能统计

| 指标 | 数值 |
|------|------|
| 支持云平台 | AWS, Azure, GCP, Alibaba Cloud |
| 架构模式 | 15+ 种经典模式 |
| IaC 示例 | Terraform, CloudFormation, CDK, Pulumi |
| 安全最佳实践 | IAM, KMS, Security Hub, GuardDuty |
| 成本优化工具 | Cost Explorer, Compute Optimizer, Trusted Advisor |
| 文档完整度 | ★★★★★ |

---

## ⭐ 支持本项目

如果这个技能对你有帮助，请考虑支持我们的持续开发：

💰 **微信支付**：查看 `images/wechat-pay.png`  
💰 **支付宝**：查看 `images/alipay.png`  
⭐ **GitHub Star**：给个 Star 让更多人发现这个项目！

---

**技能版本**: v1.0.0  
**最后更新**: 2026-04-28  
**适用场景**: 企业云架构 | 迁移规划 | 成本优化 | 安全合规
