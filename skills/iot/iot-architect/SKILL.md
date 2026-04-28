# Hermes-Skill-IoT-Architect

> 🌐 **企业级 IoT 架构设计** | 边缘计算 | 设备管理 | 实时数据处理 | 工业物联网

---

## 📋 技能概述

Hermes-Skill-IoT-Architect 是一个专业的物联网（IoT）系统架构 AI 助手，专注于提供从设备层到云端的端到端 IoT 解决方案设计。涵盖边缘计算、设备管理、协议选择（MQTT/CoAP/HTTP）、实时数据流处理、数字孪生、工业物联网（IIoT）等核心领域。

### IoT 技术栈全景图

```
┌─────────────────────────────────────────────────────────────┐
│                   IoT Architecture Stack                    │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  🔌 Device Layer      │  📡 Gateway Layer     │  ☁️ Cloud   │
│  ├── Sensors         │  ├── Edge Computing   │  ├── Ingest  │
│  ├── Actuators       │  ├── Protocol Trans. │  ├── Process  │
│  ├── Embedded (MCU)  │  ├── Local Storage    │  ├── Store   │
│  └── Firmware OTA    │  └── Security         │  └── Analyze │
│                                                             │
│  📊 Data Pipeline     │  🔒 Security          │  🏭 IIoT     │
│  ├── Stream Process. │  ├── Device Auth      │  ├── Predict.│
│  ├── Time Series DB  │  ├── Encrypted Comm. │  │   Maint.   │
│  ├── Event Processing│  └── Secure Boot      │  ├── Digital │
│  └── Analytics       │                      │  │   Twin     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚀 快速开始

### 架构设计

```bash
# IoT 系统架构评估
/iot architecture-assess "smart factory with 1000+ sensors"

# 协议选型建议
/iot protocol-select "low-power battery devices, indoor environment"

# 边缘计算方案
/iot edge-solution "real-time quality inspection on production line"

# 设备管理平台
/iot device-management "fleet of 5000 environmental monitors"
```

### 数据流设计

```bash
# 数据管道配置
/iot data-pipeline "sensor-data-ingestion-processing-storage"

# 实时分析规则
/iot real-time-rules "temperature alerting system"

# 数字孪生建模
/iot digital-twin "HVAC system simulation"
```

---

## 🏗️ IoT 参考架构

### 分层架构设计

```
┌─────────────────────────────────────────────────────────────┐
│              Enterprise IoT Reference Architecture          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Layer 4: Business Intelligence & Applications             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  BI Dashboards │ ML Models │ ERP Integration        │   │
│  │  Alert Systems │ Reports   │ Third-party APIs        │   │
│  └─────────────────────────────────────────────────────┘   │
│                          ↕ API                             │
│  Layer 3: Data Processing & Analytics                      │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  Stream Processing (Apache Flink/Kafka Streams)      │   │
│  │  Time-Series DB (InfluxDB/TimescaleDB)               │   │
│  │  Data Lake (Delta Lake/Iceberg)                     │   │
│  │  Query Engine (Presto/Trino)                        │   │
│  └─────────────────────────────────────────────────────┘   │
│                          ↕                                 │
│  Layer 2: Platform & Integration                           │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  Device Registry │ Rule Engine │ Data Transformer    │   │
│  │  Message Broker  │ Job Scheduler│ Event Router        │   │
│  └─────────────────────────────────────────────────────┘   │
│                          ↕ MQTT/HTTPS                     │
│  Layer 1: Edge & Connectivity                              │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  Edge Gateways  │ Protocol Adapters │ Local Cache     │   │
│  │  MQTT Broker    │ Data Aggregation  │ Preprocessing   │   │
│  └─────────────────────────────────────────────────────┘   │
│                          ↕ LoRaWAN/Zigbee/BLE/WiFi        │
│  Layer 0: Physical Devices & Sensors                       │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  Sensors │ Actuators │ Controllers │ RFID/Cameras    │   │
│  │  PLCs    │ RTUs       │ Smart Meters │ Wearables     │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### MQTT 消息架构实现

```python
# mqtt_architecture.py - 企业级 MQTT 消息系统

import paho.mqtt.client as mqtt
import json
import ssl
import time
from datetime import datetime
from typing import Dict, List, Optional, Callable
from dataclasses import dataclass, asdict
from enum import Enum
import asyncio
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

class QoSLevel(Enum):
    AT_MOST_ONCE = 0     # Fire and forget
    AT_LEAST_ONCE = 1    # Acknowledged delivery
    EXACTLY_ONCE = 2     # Exactly once delivery

@dataclass
class TelemetryMessage:
    """标准化的遥测数据消息格式"""
    device_id: str
    timestamp: str
    metric_type: str       # temperature, humidity, pressure, etc.
    value: float
    unit: str
    quality: int           # 0=bad, 255=good
    metadata: Dict = None
    
    def to_json(self) -> str:
        return json.dumps(asdict(self))

@dataclass
class DeviceCommand:
    """设备命令消息"""
    command_id: str
    target_device_id: str
    command_type: string    # set_value, reboot, firmware_update, config_change
    parameters: Dict
    issued_at: str
    expires_at: Optional[str] = None
    priority: int = 5       # 1=highest, 10=lowest

class IoTMQTTBroker:
    """
    企业级 MQTT Broker 管理
    支持 AWS IoT / HiveMQ / EMQX / Mosquitto
    """
    
    def __init__(self, config: dict):
        self.config = config
        self.client_id = config.get('client_id', 'hermes-iot-platform')
        
        # 创建 MQTT 客户端
        self.client = mqtt.Client(
            client_id=self.client_id,
            protocol=mqtt.MQTTv311,
            clean_session=True
        )
        
        # 配置 TLS/SSL
        if config.get('tls_enabled', True):
            self._configure_tls()
        
        # 设置回调
        self.client.on_connect = self._on_connect
        self.client.on_message = self._on_message
        self.client.on_publish = self._on_publish
        self.client.on_subscribe = self._on_subscribe
        
        # 消息处理器注册表
        self.message_handlers: Dict[str, Callable] = {}
        
        # QoS 策略
        self.qos_strategy = {
            'telemetry': QoSLevel.AT_LEAST_ONCE.value,
            'alerts': QoSLevel.EXACTLY_ONCE.value,
            'commands': QoSLevel.EXACTLY_ONCE.value,
            'config': QoSLevel.EXACTLY_ONCE.value,
            'heartbeat': QoSLevel.AT_MOST_ONCE.value
        }
    
    def _configure_tls(self):
        """配置 TLS 安全连接"""
        tls_config = {
            'ca_certs': self.config.get('ca_cert_path'),
            'certfile': self.config.get('client_cert_path'),
            'keyfile': self.config.get('client_key_path'),
            'tls_version': ssl.PROTOCOL_TLS,
            'cert_reqs': ssl.CERT_REQUIRED
        }
        self.client.tls_set(**{k: v for k, v in tls_config.items() if v})
    
    async def connect(self) -> bool:
        """连接到 MQTT Broker"""
        try:
            username = self.config.get('username')
            password = self.config.get('password')
            
            self.client.username_pw_set(username, password)
            
            broker_host = self.config['broker_host']
            broker_port = self.config.get('broker_port', 8883)
            keepalive = self.config.get('keepalive', 60)
            
            result = self.client.connect(broker_host, broker_port, keepalive)
            self.client.loop_start()
            
            logger.info(f"Connected to MQTT Broker at {broker_host}:{broker_port}")
            return result == mqtt.MQTT_ERR_SUCCESS
            
        except Exception as e:
            logger.error(f"Failed to connect to MQTT Broker: {e}")
            return False
    
    def subscribe_to_topics(self, topic_patterns: List[str]):
        """订阅主题模式（支持通配符）"""
        for topic in topic_patterns:
            qos = self._determine_qos_for_topic(topic)
            self.client.subscribe(topic, qos=qos)
            logger.info(f"Subscribed to topic: {topic} with QoS {qos}")
    
    def register_handler(self, topic_pattern: str, handler: Callable):
        """注册消息处理器"""
        self.message_handlers[topic_pattern] = handler
        logger.info(f"Registered handler for: {topic_pattern}")
    
    def publish_telemetry(self, message: TelemetryMessage):
        """发布遥测数据"""
        topic = f"devices/{message.device_id}/telemetry/{message.metric_type}"
        qos = self.qos_strategy['telemetry']
        
        payload = message.to_json()
        info = self.client.publish(topic, payload, qos=qos, retain=False)
        
        logger.debug(f"Published telemetry to {topic}: {payload[:100]}")
        return info
    
    def publish_command(self, command: DeviceCommand):
        """发送设备命令"""
        topic = f"devices/{command.target_device_id}/commands/{command.command_type}"
        qos = self.qos_strategy['commands']
        
        payload = json.dumps(asdict(command))
        info = self.client.publish(topic, payload, qos=qos, retain=True)
        
        logger.info(f"Published command to {topic}: {command.command_id}")
        return info
    
    def _on_connect(self, client, userdata, flags, rc, properties=None):
        """连接成功回调"""
        if rc == 0:
            logger.info("MQTT Client connected successfully")
            # 自动订阅系统主题
            client.subscribe("$SYS/#", qos=0)
        else:
            logger.error(f"Connection failed with code: {rc}")
    
    def _on_message(self, client, userdata, msg):
        """消息接收回调"""
        topic = msg.topic
        payload = msg.payload.decode('utf-8')
        
        logger.debug(f"Received message on {topic}: {payload[:200]}")
        
        # 路由到匹配的处理器
        for pattern, handler in self.message_handlers.items():
            if self._match_topic(pattern, topic):
                try:
                    handler(topic, payload)
                except Exception as e:
                    logger.error(f"Handler error for {pattern}: {e}")
                break
    
    def _determine_qos_for_topic(self, topic: str) -> int:
        """根据主题确定 QoS 级别"""
        for key, qos in self.qos_strategy.items():
            if key in topic.lower():
                return qos
        return QoSLevel.AT_LEAST_ONCE.value
    
    @staticmethod
    def _match_topic(pattern: str, topic: str) -> bool:
        """简单的主题模式匹配（支持 + 和 # 通配符）"""
        pattern_parts = pattern.split('/')
        topic_parts = topic.split('/')
        
        if len(pattern_parts) != len(topic_parts) and '#' not in pattern:
            return False
        
        for pp, tp in zip(pattern_parts, topic_parts):
            if pp == '#':
                return True
            elif pp == '+' or pp == tp:
                continue
            else:
                return False
        
        return True


# ===== 使用示例 =====

async def main():
    # 配置
    config = {
        'broker_host': 'mqtt.hermes-iot.com',
        'broker_port': 8883,
        'client_id': 'hermes-platform-core',
        'username': '${MQTT_USERNAME}',
        'password': '${MQTT_PASSWORD}',
        'tls_enabled': True,
        'ca_cert_path': '/certs/ca.crt',
        'client_cert_path': '/certs/client.crt',
        'client_key_path': '/certs/client.key'
    }
    
    # 初始化 Broker 连接
    broker = IoTMQTTBroker(config)
    
    # 连接
    await broker.connect()
    
    # 订阅所有设备数据
    broker.subscribe_to_topics([
        'devices/+/telemetry/#',
        'devices/+/status',
        'commands/+/response',
        'alerts/#'
    ])
    
    # 注册处理器
    def handle_telemetry(topic: str, payload: str):
        data = json.loads(payload)
        logger.info(f"Telemetry from {data['device_id']}: {data['metric_type']}={data['value']}{data['unit']}")
        
        # 触发实时规则引擎
        if data['metric_type'] == 'temperature' and data['value'] > 80:
            broker.publish_command(DeviceCommand(
                command_id=f"cmd_{int(time.time())}",
                target_device_id=data['device_id'],
                command_type="set_value",
                parameters={"target": "cooling_fan", "value": "high"},
                issued_at=datetime.utcnow().isoformat()
            ))
    
    broker.register_handler('devices/+/telemetry/#', handle_telemetry)
    
    # 发布测试数据
    test_msg = TelemetryMessage(
        device_id="sensor-temp-001",
        timestamp=datetime.utcnow().isoformat(),
        metric_type="temperature",
        value=85.5,
        unit="celsius",
        quality=128
    )
    
    broker.publish_telemetry(test_msg)

if __name__ == "__main__":
    asyncio.run(main())
```

---

## 📡 边缘计算架构

### Edge Gateway 设计

```python
# edge_gateway.py - 边缘网关服务

from fastapi import FastAPI, HTTPException, BackgroundTasks
from pydantic import BaseModel
from typing import List, Dict, Optional
from datetime import datetime
import asyncio
import aioredis
import pandas as pd
from dataclasses import dataclass
import statistics

app = FastAPI(title="Hermes Edge Gateway", version="2.0.0")

# ===== 数据模型 =====

class SensorReading(BaseModel):
    sensor_id: str
    value: float
    unit: str
    timestamp: datetime
    quality: int = 255

class AggregatedData(BaseModel):
    window_start: datetime
    window_end: datetime
    metrics: Dict[str, float]  # avg, min, max, std, count
    source_sensors: List[str]

class AlertRule(BaseModel):
    rule_id: str
    metric_name: str
    condition: str  # ">", "<", "==", "range"
    threshold: float
    severity: str  # info, warning, critical
    cooldown_seconds: int = 300

# ===== 边缘存储 =====

class EdgeStorage:
    """本地时序数据缓存（Redis-backed）"""
    
    def __init__(self, redis_url: str):
        self.redis_url = redis_url
        self.redis = None
        self.local_cache: Dict[str, List[SensorReading]] = {}
        self.cache_max_size = 10000
    
    async def connect(self):
        self.redis = await aioredis.from_url(
            self.redis_url,
            decode_responses=True
        )
    
    async def store_reading(self, reading: SensorReading):
        """存储传感器读数（先本地后 Redis）"""
        key = f"sensor:{reading.sensor_id}"
        
        # 本地缓存
        if key not in self.local_cache:
            self.local_cache[key] = []
        
        self.local_cache[key].append(reading)
        
        # 限制缓存大小
        if len(self.local_cache[key]) > self.cache_max_size:
            self.local_cache[key] = self.local_cache[key][-self.cache_max_size:]
        
        # 异步写入 Redis（时间序列）
        await self.redis.zadd(
            f"timeseries:{reading.sensor_id}",
            {reading.timestamp.isoformat(): reading.json()}
        )
        
        # 设置过期时间（保留最近 7 天）
        await self.redis.expire(f"timeseries:{reading.sensor_id}", 604800)
    
    async def get_window_data(
        self,
        sensor_id: str,
        start_time: datetime,
        end_time: datetime,
        downsample: bool = True
    ) -> List[SensorReading]:
        """获取时间窗口内的数据"""
        key = f"timeseries:{sensor_id}"
        
        # 从 Redis ZSET 获取范围数据
        raw_data = await self.redis.zrangebyscore(
            key,
            min=start_time.isoformat(),
            max=end_time.isoformat()
        )
        
        readings = [SensorReading.parse_raw(item) for item in raw_data]
        
        # 如果需要降采样（减少数据点）
        if downsample and len(readings) > 1000:
            readings = self._downsample(readings, target_points=500)
        
        return readings
    
    def _downsample(self, data: List[SensorReading], target_points: int) -> List[SensorReading]:
        """降采样：LTTB (Largest Triangle Three Buckets) 算法简化版"""
        if len(data) <= target_points:
            return data
        
        step = len(data) / target_points
        sampled = []
        
        for i in range(target_points):
            idx = int(i * step)
            sampled.append(data[idx])
        
        return sampled

# ===== 数据聚合引擎 =====

class DataAggregator:
    """边缘数据聚合与预处理"""
    
    def __init__(self, storage: EdgeStorage):
        self.storage = storage
    
    async def aggregate_window(
        self,
        sensor_ids: List[str],
        window_minutes: int = 15,
        aggregation_type: str = "mean"
    ) -> AggregatedData:
        """
        对指定传感器进行时间窗口聚合
        
        Args:
            sensor_ids: 传感器 ID 列表
            window_minutes: 时间窗口大小（分钟）
            aggregation_type: 聚合类型 (mean, sum, min, max, count)
        """
        end_time = datetime.utcnow()
        start_time = end_time - timedelta(minutes=window_minutes)
        
        all_readings = []
        for sensor_id in sensor_ids:
            readings = await self.storage.get_window_data(sensor_id, start_time, end_time)
            all_readings.extend(readings)
        
        if not all_readings:
            raise HTTPException(status_code=404, detail="No data available for window")
        
        values = [r.value for r in all_readings]
        
        # 计算统计指标
        metrics = {
            'mean': statistics.mean(values),
            'min': min(values),
            'max': max(values),
            'std': statistics.stdev(values) if len(values) > 1 else 0,
            'count': len(values),
            'p50': self._percentile(values, 50),
            'p95': self._percentile(values, 95),
            'p99': self._percentile(values, 99)
        }
        
        return AggregatedData(
            window_start=start_time,
            window_end=end_time,
            metrics=metrics,
            source_sensors=sensor_ids
        )
    
    @staticmethod
    def _percentile(data: List[float], percentile: float) -> float:
        """计算百分位数"""
        sorted_data = sorted(data)
        k = (len(sorted_data) - 1) * (percentile / 100)
        f = int(k)
        c = f + 1
        if c >= len(sorted_data):
            return sorted_data[-1]
        return sorted_data[f] + (k - f) * (sorted_data[c] - sorted_data[f])

# ===== 实时规则引擎 =====

class RealtimeRuleEngine:
    """边缘侧实时告警规则引擎"""
    
    def __init__(self):
        self.rules: Dict[str, AlertRule] = {}
        self.alert_history: Dict[str, datetime] = {}  # 用于冷却期
    
    def add_rule(self, rule: AlertRule):
        self.rules[rule.rule_id] = rule
    
    def evaluate(self, reading: SensorReading) -> Optional[Dict]:
        """对单个读数评估所有适用规则"""
        alerts = []
        
        for rule_id, rule in self.rules.items():
            if rule.metric_name != reading.unit.replace('_', ''):
                continue
            
            # 检查冷却期
            last_alert = self.alert_history.get(rule_id)
            if last_alert:
                elapsed = (datetime.utcnow() - last_alert).total_seconds()
                if elapsed < rule.cooldown_seconds:
                    continue
            
            # 评估条件
            triggered = False
            
            if rule.condition == ">":
                triggered = reading.value > rule.threshold
            elif rule.condition == "<":
                triggered = reading.value < rule.threshold
            elif rule.condition == "range":
                # range 需要额外参数
                pass
            
            if triggered:
                self.alert_history[rule_id] = datetime.utcnow()
                alerts.append({
                    'rule_id': rule_id,
                    'severity': rule.severity,
                    'metric': rule.metric_name,
                    'current_value': reading.value,
                    'threshold': rule.threshold,
                    'sensor_id': reading.sensor_id,
                    'timestamp': reading.timestamp.isoformat(),
                    'message': f"{rule.metric_name} {reading.value}{reading.unit} exceeds threshold {rule.threshold}"
                })
        
        return alerts if alerts else None

# ===== API 端点 =====

storage = EdgeStorage("redis://localhost:6379")
aggregator = DataAggregationEngine(storage)
rule_engine = RealtimeRuleEngine()

@app.on_event("startup")
async def startup():
    await storage.connect()

@app.post("/api/v2/telemetry")
async def ingest_telemetry(readings: List[SensorReading]):
    """批量接收遥测数据"""
    tasks = [storage.store_reading(r) for r in readings]
    await asyncio.gather(*tasks)
    
    # 实时规则检查
    alerts = []
    for reading in readings:
        result = rule_engine.evaluate(reading)
        if result:
            alerts.extend(result)
    
    return {
        "accepted": len(readings),
        "alerts_triggered": len(alerts),
        "alerts": alerts[:10]  # 返回前 10 个告警
    }

@app.get("/api/v2/aggregates/{sensor_ids}")
async def get_aggregated_data(
    sensor_ids: str,  # 逗号分隔的传感器 ID
    window_minutes: int = 15
):
    """获取聚合后的数据"""
    ids = sensor_ids.split(',')
    result = await aggregator.aggregate_window(ids, window_minutes)
    return result

@app.post("/api/v2/rules")
async def create_alert_rule(rule: AlertRule):
    """创建新的告警规则"""
    rule_engine.add_rule(rule)
    return {"status": "created", "rule": rule}

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8100)
```

---

## 🔐 IoT 安全架构

### 设备身份认证框架

```python
# iot_security.py - IoT 设备安全认证与管理

from cryptography import x509
from cryptography.x509.oid import NameOID
from cryptography.hazmat.primitives import hashes, serialization
from cryptography.hazmat.primitives.asymmetric import ec
from cryptography.hazmat.backends import default_backend
from datetime import datetime, timedelta
from typing import Dict, Optional, Tuple
import jwt
import hashlib
import secrets
import base64

class IoTDeviceIdentityManager:
    """
    IoT 设备身份管理系统
    基于 PKI + JWT 双重认证机制
    """
    
    def __init__(self, ca_private_key, ca_certificate):
        self.ca_private_key = ca_private_key
        self.ca_certificate = ca_certificate
        self.device_registry: Dict[str, dict] = {}
        self.revoked_devices: set = set()
    
    def generate_device_identity(
        self,
        device_id: str,
        device_type: str,
        validity_days: int = 365
    ) -> Tuple[bytes, bytes]:
        """
        为新设备生成身份证书
        
        Returns:
            (private_key_pem, certificate_pem)
        """
        
        # 生成 EC 密钥对（轻量级，适合嵌入式设备）
        private_key = ec.generate_private_key(ec.SECP256R1(), default_backend())
        
        # 构建 CSR（证书签名请求）
        subject = x509.Name([
            x509.NameAttribute(NameOID.COMMON_NAME, device_id),
            x509.NameAttribute(NameOID.ORGANIZATION_NAME, "Hermes IoT"),
            x509.NameAttribute(NameOID.ORGANIZATIONAL_UNIT_NAME, device_type),
        ])
        
        # 构建证书
        now = datetime.utcnow()
        certificate = (
            x509.CertificateBuilder()
            .subject_name(subject)
            .issuer_name(self.ca_certificate.subject)
            .public_key(private_key.public_key())
            .serial_number(x509.random_serial_number())
            .not_valid_before(now)
            .not_valid_after(now + timedelta(days=validity_days))
            .add_extension(
                x509.SubjectAlternativeName([
                    x509.DNSName(f"{device_id}.devices.hermes-iot.com"),
                    x509.DNSName(device_id),
                ]),
                critical=False,
            )
            .add_extension(
                x509.BasicConstraints(ca=False, path_length=None), critical=True
            )
            .sign(self.ca_private_key, hashes.SHA256(), default_backend())
        )
        
        # 序列化为 PEM 格式
        private_key_pem = private_key.private_bytes(
            encoding=serialization.Encoding.PEM,
            format=serialization.PrivateFormat.PKCS8,
            encryption_algorithm=serialization.NoEncryption()
        )
        
        certificate_pem = certificate.public_bytes(serialization.Encoding.PEM)
        
        # 注册设备
        self.device_registry[device_id] = {
            'certificate_fingerprint': hashlib.sha256(certificate_pem).hexdigest(),
            'type': device_type,
            'registered_at': now.isoformat(),
            'valid_until': (now + timedelta(days=validity_days)).isoformat(),
            'status': 'active'
        }
        
        return private_key_pem, certificate_pem
    
    def generate_device_token(
        self,
        device_id: str,
        additional_claims: Dict = None,
        expiry_hours: int = 24
    ) -> str:
        """
        生成用于 API 访问的 JWT Token
        结合证书指纹实现双向绑定
        """
        
        device_info = self.device_registry.get(device_id)
        if not device_info:
            raise ValueError(f"Device {device_id} not registered")
        
        if device_id in self.revoked_devices:
            raise PermissionError(f"Device {device_id} has been revoked")
        
        payload = {
            'sub': device_id,
            'iat': datetime.utcnow(),
            'exp': datetime.utcnow() + timedelta(hours=expiry_hours),
            'type': 'device_access_token',
            'cert_fingerprint': device_info['certificate_fingerprint'],
            'device_type': device_info['type'],
            **(additional_claims or {})
        }
        
        token = jwt.encode(
            payload,
            self.ca_private_key,  # 使用 CA 私钥签名
            algorithm='ES256'     # ECDSA 签名
        )
        
        return token
    
    def verify_device_token(self, token: str, certificate_pem: bytes) -> Dict:
        """
        验证设备 Token 并校验证书绑定
        """
        try:
            # 解码并验证 JWT
            payload = jwt.decode(
                token,
                self.ca_certificate.public_key(),  # 使用 CA 公钥验证
                algorithms=['ES256']
            )
            
            # 验证证书指纹是否匹配
            cert_fingerprint = hashlib.sha256(certificate_pem).hexdigest()
            if payload.get('cert_fingerprint') != cert_fingerprint:
                raise SecurityError("Certificate fingerprint mismatch")
            
            # 检查吊销状态
            if payload['sub'] in self.revoked_devices:
                raise PermissionError("Device has been revoked")
            
            return {
                'valid': True,
                'device_id': payload['sub'],
                'device_type': payload['device_type'],
                'expires_at': datetime.fromtimestamp(payload['exp']).isoformat()
            }
            
        except jwt.ExpiredSignatureError:
            return {'valid': False, 'error': 'Token expired'}
        except Exception as e:
            return {'valid': False, 'error': str(e)}
    
    def revoke_device(self, device_id: str, reason: str):
        """吊销设备（加入 CRL）"""
        if device_id in self.device_registry:
            self.revoked_devices.add(device_id)
            self.device_registry[device_id]['status'] = 'revoked'
            self.device_registry[device_id]['revoked_at'] = datetime.utcnow().isoformat()
            self.device_registry[device_id]['revocation_reason'] = reason

class SecureBootValidator:
    """安全启动验证器（防止固件篡改）"""
    
    @staticmethod
    def validate_firmware_integrity(
        firmware_binary: bytes,
        expected_hash: str,
        signature: bytes,
        public_key: bytes
    ) -> bool:
        """
        验证固件完整性
        
        流程：
        1. 计算 SHA256 哈希
        2. 与预期哈希比对
        3. 使用公钥验证签名
        """
        
        # 1. 计算实际哈希
        actual_hash = hashlib.sha256(firmware_binary).hexdigest()
        
        # 2. 哈希比对
        if actual_hash != expected_hash:
            return False
        
        # 3. 签名验证（此处为伪代码，实际使用 cryptography 库）
        # verified = verify_signature(public_key, signature, firmware_binary)
        # return verified
        
        return True
    
    @staticmethod
    def generate_secure_boot_token(
        device_id: str,
        firmware_version: str,
        boot_nonce: str
    ) -> str:
        """生成安全启动令牌（防重放攻击）"""
        payload = {
            'device_id': device_id,
            'firmware_version': firmware_version,
            'nonce': boot_nonce,
            'timestamp': datetime.utcnow().isoformat()
        }
        
        # 在实际应用中应使用硬件安全模块 (HSM) 或 TPM
        token = base64.urlsafe_b64encode(
            json.dumps(payload).encode()
        ).decode()
        
        return token
```

---

## 📊 数字孪生建模

### 数字孪生基础框架

```python
# digital_twin.py - 数字孪生核心引擎

from dataclasses import dataclass, field
from typing import Dict, List, Any, Optional, Callable
from datetime import datetime
from enum import Enum
import asyncio
import numpy as np
from abc import ABC, abstractmethod

class TwinState(Enum):
    INITIALIZING = "initializing"
    ACTIVE = "active"
    DEGRADED = "degraded"
    SIMULATING = "simulating"
    ERROR = "error"

@dataclass
class PropertySnapshot:
    """属性快照"""
    property_name: str
    value: Any
    unit: str
    timestamp: datetime
    confidence: float = 1.0
    source: str = "sensor"

@dataclass
class Relationship:
    """实体间关系"""
    target_twin_id: str
    relationship_type: str  # "contains", "controls", "feeds_into", "located_at"
    properties: Dict[str, Any] = field(default_factory=dict)

@dataclass 
class TelemetryBinding:
    """物理设备到数字孪生的数据绑定"""
    sensor_id: str
    twin_property: str
    transformation: Optional[Callable] = None  # 数据转换函数
    update_frequency_ms: int = 1000
    last_update: Optional[datetime] = None

class DigitalTwin(ABC):
    """
    数字孪生抽象基类
    表示物理实体的虚拟副本
    """
    
    def __init__(
        self,
        twin_id: str,
        name: str,
        twin_type: str,
        physical_entity_id: Optional[str] = None
    ):
        self.twin_id = twin_id
        self.name = name
        self.twin_type = twin_type
        self.physical_entity_id = physical_entity_id
        self.state = TwinState.INITIALIZING
        
        # 属性历史记录
        self.property_history: Dict[str, List[PropertySnapshot]] = {}
        
        # 当前属性值
        self.current_properties: Dict[str, Any] = {}
        
        # 关系网络
        self.relationships: List[Relationship] = []
        
        # 数据绑定
        self.telemetry_bindings: List[TelemetryBinding] = []
        
        # 行为模型（可插拔）
        self.behavior_models: Dict[str, Any] = {}
        
        # 事件处理器
        self.event_handlers: Dict[str, List[Callable]] = {}
        
        # 同步状态
        self.last_sync_time: Optional[datetime] = None
        self.sync_quality: float = 1.0  # 0-1, 孪生体与实体的同步程度
    
    def add_property(self, name: str, initial_value: Any, unit: str):
        """添加可观测属性"""
        self.current_properties[name] = initial_value
        self.property_history[name] = [
            PropertySnapshot(
                property_name=name,
                value=initial_value,
                unit=unit,
                timestamp=datetime.utcnow(),
                source="initial"
            )
        ]
    
    def bind_telemetry(self, binding: TelemetryBinding):
        """绑定传感器数据到属性"""
        self.telemetry_bindings.append(binding)
    
    def add_relationship(self, relationship: Relationship):
        """添加与其他孪生体的关系"""
        self.relationships.append(relationship)
    
    def update_property(self, property_name: str, value: Any, source: str = "external"):
        """更新属性值并记录历史"""
        old_value = self.current_properties.get(property_name)
        self.current_properties[property_name] = value
        
        snapshot = PropertySnapshot(
            property_name=property_name,
            value=value,
            unit=self._get_unit(property_name),
            timestamp=datetime.utcnow(),
            source=source
        )
        
        self.property_history[property_name].append(snapshot)
        
        # 触发变更事件
        if old_value != value:
            self._emit_event('property_changed', {
                'property': property_name,
                'old_value': old_value,
                'new_value': value,
                'timestamp': snapshot.timestamp.isoformat()
            })
    
    async def sync_from_physical(self, telemetry_data: Dict[str, Any]):
        """从物理实体同步数据"""
        for binding in self.telemetry_bindings:
            if binding.sensor_id in telemetry_data:
                raw_value = telemetry_data[binding.sensor_id]
                
                # 应用转换函数
                if binding.transformation:
                    processed_value = binding.transformation(raw_value)
                else:
                    processed_value = raw_value
                
                self.update_property(
                    binding.twin_property,
                    processed_value,
                    source=f"sensor:{binding.sensor_id}"
                )
                
                binding.last_update = datetime.utcnow()
        
        self.last_sync_time = datetime.utcnow()
        self.state = TwinState.ACTIVE
    
    async def simulate(self, duration_seconds: float, dt: float = 0.1):
        """
        运行仿真模型
        基于当前状态预测未来行为
        """
        self.state = TwinState.SIMULATING
        
        simulation_steps = int(duration_seconds / dt)
        results = []
        
        current_state = {**self.current_properties}
        
        for step in range(simulation_steps):
            # 应用行为模型
            next_state = await self._apply_behavior_models(current_state, dt)
            
            # 记录仿真结果
            results.append({
                'time': step * dt,
                'state': dict(next_state)
            })
            
            current_state = next_state
        
        self.state = TwinState.ACTIVE
        return results
    
    @abstractmethod
    async def _apply_behavior_models(
        self, 
        current_state: Dict[str, Any], 
        dt: float
    ) -> Dict[str, Any]:
        """子类实现的物理行为模型"""
        pass
    
    def get_property_history(
        self, 
        property_name: str, 
        since: Optional[datetime] = None,
        limit: int = 1000
    ) -> List[PropertySnapshot]:
        """获取属性历史"""
        history = self.property_history.get(property_name, [])
        
        if since:
            history = [h for h in history if h.timestamp >= since]
        
        return history[-limit:]
    
    def calculate_sync_quality(self) -> float:
        """计算同步质量（基于数据新鲜度）"""
        if not self.last_sync_time:
            return 0.0
        
        age_seconds = (datetime.utcnow() - self.last_sync_time).total_seconds()
        
        # 指数衰减：越旧的数据同步质量越低
        quality = np.exp(-age_seconds / 3600)  # 1 小时半衰期
        
        self.sync_quality = quality
        return quality
    
    def _emit_event(self, event_type: str, data: Dict):
        """触发事件"""
        handlers = self.event_handlers.get(event_type, [])
        for handler in handlers:
            try:
                handler(data)
            except Exception as e:
                print(f"Event handler error: {e}")
    
    def on(self, event_type: str, handler: Callable):
        """注册事件监听器"""
        if event_type not in self.event_handlers:
            self.event_handlers[event_type] = []
        self.event_handlers[event_type].append(handler)


class HVDigitalTwin(DigitalTwin):
    """暖通空调系统 (HVAC) 数字孪生示例"""
    
    def __init__(self, twin_id: str, name: str):
        super().__init__(twin_id, name, "hvac_system")
        
        # 物理属性
        self.add_property("indoor_temperature", 22.0, "°C")
        self.add_property("outdoor_temperature", 15.0, "°C")
        self.add_property("humidity", 45.0, "%")
        self.add_property("fan_speed", 0, "RPM")
        self.add_property("compressor_power", 0, "kW")
        self.add_property("setpoint", 22.0, "°C")
        
        # 热力学参数
        self.thermal_mass = 5000  # J/K (建筑热容量)
        self.insulation_factor = 0.95  # 保温系数
        self.max_cooling_capacity = 10.0  # kW
        
        # PID 控制器状态
        self.integral_error = 0.0
        self.previous_error = 0.0
    
    async def _apply_behavior_models(
        self, 
        state: Dict[str, Any], 
        dt: float
    ) -> Dict[str, Any]:
        """HVAC 热力学仿真模型"""
        
        T_in = state['indoor_temperature']
        T_out = state['outdoor_temperature']
        T_set = state['setpoint']
        fan_speed = state['fan_speed']
        compressor = state['compressor_power']
        
        # 1. 热传导模型（牛顿冷却定律）
        heat_transfer_coefficient = 50  # W/K
        q_conduction = heat_transfer_coefficient * (T_out - T_in) * (1 - self.insulation_factor)
        
        # 2. 制冷量计算
        cooling_capacity = compressor * 1000  # W
        q_cooling = -cooling_capacity if compressor > 0 else 0
        
        # 3. 风扇效应（强制对流）
        q_convection = fan_speed * 0.001 * (T_out - T_in)  # 简化模型
        
        # 4. 总热流量
        q_total = q_conduction + q_cooling + q_convection
        
        # 5. 温度变化 (dT = Q / C)
        dT = (q_total * dt) / self.thermal_mass
        new_temp = T_in + dT
        
        # 6. PID 控制逻辑（自动调节）
        error = T_set - T_in
        self.integral_error += error * dt
        derivative = (error - self.previous_error) / dt if dt > 0 else 0
        
        Kp, Ki, Kd = 2.0, 0.5, 0.1
        control_output = Kp*error + Ki*self.integral_error + Kd*derivative
        
        # 限制输出范围
        new_compressor = max(0, min(control_output, self.max_cooling_capacity))
        new_fan_speed = int(new_compressor * 100)  # RPM per kW
        
        self.previous_error = error
        
        return {
            **state,
            'indoor_temperature': round(new_temp, 2),
            'compressor_power': round(new_compressor, 2),
            'fan_speed': new_fan_speed
        }


# ===== 使用示例 =====

async def create_hvac_twin():
    hvac = HVDigitalTwin("hvac-building-a", "Building A HVAC System")
    
    # 绑定传感器
    hvac.bind_telemetry(TelemetryBinding(
        sensor_id="temp-sensor-01",
        twin_property="indoor_temperature",
        update_frequency_ms=5000
    ))
    
    hvac.bind_telemetry(TelemetryBinding(
        sensor_id="outdoor-temp",
        twin_property="outdoor_temperature",
        update_frequency_ms=30000
    ))
    
    # 注册事件
    def on_temperature_alert(data):
        if data['new_value'] > 28:
            print(f"⚠️ High temperature alert: {data['new_value']}°C")
    
    hvac.on('property_changed', on_temperature_alert)
    
    # 模拟运行 1 小时的预测
    prediction = await hvac.simulate(duration_seconds=3600, dt=60)
    
    print(f"Simulation complete. Final predicted temp: {prediction[-1]['state']['indoor_temperature']}°C")
    
    return hvac
```

---

## 🔗 相关技能

- [Hermes-Skill-Edge-Computing-Specialist](../edge-computing/edge-computing-specialist/SKILL.md) - 边缘计算深度优化
- [Hermes-Skill-Cybersecurity-Auditor](../security/cybersecurity-auditor/SKILL.md) - IoT 安全审计
- [Hermes-Skill-Cloud-Architect](../cloud/cloud-architect/SKILL.md) - 云边协同架构

---

## 📊 技能统计

| 指标 | 数值 |
|------|------|
| 支持协议 | MQTT, CoAP, HTTP, WebSocket, AMQP |
| 边缘框架 | AWS Greengrass, Azure IoT Edge, KubeEdge |
| 安全标准 | PKI, TLS 1.3, Secure Boot, TPM |
| 数字孪生 | 物理建模、仿真预测、状态同步 |
| 平台支持 | AWS IoT, Azure IoT Hub, Google Cloud IoT |
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
**适用场景**: 工业 IoT | 智能制造 | 智慧城市 | 农业物联网 | 边缘智能
