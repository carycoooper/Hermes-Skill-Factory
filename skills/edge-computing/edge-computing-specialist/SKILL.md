# Hermes-Skill-Edge-Computing-Specialist

> 🌐 **边缘计算专家** | Kubernetes Edge | IoT Edge | CDN/边缘AI | 5G MEC

---

## 📋 技能概述

Hermes-Skill-Edge-Computing-Specialist 是一个专业的边缘计算架构与实施 AI 助手，专注于提供从边缘节点部署、边缘 AI 推理、内容分发网络 (CDN) 优化到 5G 多接入边缘计算 (MEC) 的全方位边缘计算解决方案。涵盖 K3s/KubeEdge、AWS Greengrass、Azure IoT Edge、Cloudflare Workers 等主流边缘平台。

### 边缘计算技术栈

```
┌─────────────────────────────────────────────────────────────┐
│               Edge Computing Stack                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  🖥️ Edge Runtime      │  ⚡ Edge AI           │  📡 5G MEC   │
│  ├── K3s/KubeEdge    │  ├── ONNX/TensorRT   │  ├── UPF      │
│  ├── AWS Greengrass  │  ├── TFLite/EdgeTPU  │  ├── MEC Apps │
│  ├── Azure IoT Edge  │  ├── NVIDIA Jetson   │  └── Slicing  │
│  └── Cloudflare Wkr  │  └── OpenVINO        │                 │
│                                                             │
│  🔀 Content Delivery   │  🛡️ Security          │  📊 Observ.  │
│  ├── CDN Caching     │  ├── Zero Trust       │  ├── Metrics  │
│  ├── Edge Functions  │  ├── mTLS             │  ├── Logging  │
│  └── Image Optimize  │  └── Hardware TPM     │  └── Tracing  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚀 快速开始

### 边缘集群搭建

```bash
# 使用 K3s 搭建轻量级边缘集群
/edge deploy-k3s "edge-node-01,edge-node-02,edge-node-03"

# 配置边缘 AI 推理服务
/edge setup-edge-inference "yolov8-object-detection" --accelerator tpu

# 5G MEC 应用部署
/edge deploy-mec "low-latency-ar-application" --slice-urlllc
```

### 性能优化

```bash
# 边缘缓存策略优化
/edge optimize-cache "api-responses" --ttl 300 --stale-while-revalidate 86400

# 冷启动优化
/edge reduce-cold-start "cloudflare-workers" --warming enabled
```

---

## 🏗️ 边缘架构模式

### 云边协同架构 (Cloud-Edge Collaboration)

```
┌─────────────────────────────────────────────────────────────┐
│              Cloud-Edge Architecture                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ☁️ Cloud Region (Central)                                  │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ Control Plane │ ML Training │ Global DB │ Analytics │   │
│  └─────────────────────┬───────────────────────────────┘   │
│                        │ Secure Tunnel (mTLS)               │
│                        ▼                                   │
│  ┌─────────────────────────────────────────────────────┐   │
│  │            Edge Aggregation Layer                     │   │
│  │  (Regional PoP / MEC Host)                           │   │
│  │  ┌─────────┐ ┌─────────┐ ┌─────────┐                │   │
│  │  │ API GW  │ │ Cache   │ │ Stream  │                │   │
│  │  │ Rate Lim│ │ Cluster │ │ Process │                │   │
│  │  └────┬────┘ └────┬────┘ └────┬────┘                │   │
│  └───────┼──────────┼──────────┼────────────────────────┘   │
│          │          │          │                            │
│  ┌───────▼──────────▼──────────▼────────────────────────┐   │
│  │              Edge Nodes (Distributed)                  │   │
│  │  ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐        │   │
│  │  │Factory │ │Retail  │ │Vehicle │ │Smart  │        │   │
│  │  │Floor   │ │Store   │ │(Car)   │ │Camera │        │   │
│  │  └────────┘ └────────┘ └────────┘ └────────┘        │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### K3s 边缘集群配置

```yaml
# k3s-edge-cluster.yaml - 轻量级边缘 K8s 集群配置

apiVersion: k3s.cattle.io/v1
kind: ClusterConfig
metadata:
  name: hermes-edge-cluster
spec:
  # 集群范围配置
  clusterCidr: "10.42.0.0/16"
  serviceCidr: "10.43.0.0/16"
  
  # 边缘优化选项
  options:
    # 禁用不需要的组件以节省资源
    disable:
      - traefik          # 使用自定义 Ingress Controller
      - metrics-server   # 替代为 edge-metrics-agent
      - servicelb        # 边缘场景通常使用 NodePort 或 HostNetwork
    
    # 启用边缘特性
    enable:
      - rootless          # 无根运行（安全）
      - embedded-registry # 内置镜像仓库（离线部署）
    
    # 网络配置（适合不稳定网络）
    flannel-backend: "wireguard"  # 加密 + NAT 穿透
    
    # 数据存储（使用 SQLite 替代 etcd，减少资源占用）
    datastore: "sqlite"
    
    # 资源限制（适配边缘硬件）
    kubelet-arg:
      - "max-pods=50"         # 限制 Pod 数量
      - "eviction-hard=memory.available<200Mi,nodefs.available<10%"
    
    # 保护性驱逐策略
    protect-kernel-defaults: true

---
# 边缘工作负载示例：实时视频分析
apiVersion: apps/v1
kind: Deployment
metadata:
  name: edge-video-analyzer
  namespace: edge-workloads
spec:
  replicas: 1
  selector:
    matchLabels:
      app: video-analyzer
  template:
    metadata:
      labels:
        app: video-analyzer
    spec:
      # 使用宿主网络（低延迟要求）
      hostNetwork: true
      
      # 使用 GPU/NPU（如果可用）
      nodeSelector:
        accelerator: nvidia-t4  # or intel-movidius, google-coral
      
      containers:
      - name: analyzer
        image: hermes-registry/edge-yolov8:v2.1
        resources:
          limits:
            nvidia.com/gpu: 1
            memory: "512Mi"
            cpu: "500m"
          requests:
            memory: "256Mi"
            cpu: "250m"
        
        env:
        - name: MODEL_PATH
          value: "/models/yolov8n-edge.onnx"
        - name: INFERENCE_ENGINE
          value: "tensorrt"  # or onnxruntime, openvino
        
        volumeMounts:
        - name: model-cache
          mountPath: /models
        - name: video-input
          mountPath: /dev/video0
          
        securityContext:
          readOnlyRootFilesystem: true
          runAsNonRoot: true
          capabilities:
            drop: ["ALL"]
            
        livenessProbe:
          httpGet:
            path: /healthz
            port: 8080
          initialDelaySeconds: 5
          periodSeconds: 10
          
      volumes:
      - name: model-cache
        hostPath:
          path: /var/lib/edge-models
          type: DirectoryOrCreate
      - name: video-input
        hostPath:
          path: /dev/video0
          type: CharDevice
          
      # 边缘特定容忍度
      tolerations:
      - key: "edge.hermes.io/network-unstable"
        operator: "Exists"
        effect: "NoSchedule"
      - key: "edge.hermes.io/resource-constrained"
        operator: "Exists"
        effect: "NoExecute"
```

---

## ⚡ 边缘 AI 推理优化

### ONNX Runtime 边缘部署

```python
# edge_inference.py - 高性能边缘 AI 推理引擎

import onnxruntime as ort
import numpy as np
from typing import Dict, List, Tuple, Optional
from dataclasses import dataclass
import time
import cv2

@dataclass
class InferenceResult:
    """推理结果封装"""
    predictions: np.ndarray
    confidence_scores: np.ndarray
    inference_time_ms: float
    preprocess_time_ms: float
    postprocess_time_ms: float
    model_name: str
    input_shape: Tuple[int, ...]

class EdgeInferenceEngine:
    """
    边缘推理引擎
    
    特性：
    - 支持 ONNX/TensorRT/OpenVINO 后端
    - 自动设备检测和优化
    - 动态批处理
    - 模型热加载
    """
    
    def __init__(
        self,
        model_path: str,
        execution_provider: str = 'auto',
        optimization_level: ort.GraphOptimizationLevel = ort.GraphOptimizationLevel.ORT_ENABLE_ALL
    ):
        self.model_path = model_path
        self.execution_provider = self._detect_best_provider(execution_provider)
        
        # Session 选项（边缘优化）
        sess_options = ort.SessionOptions()
        sess_options.graph_optimization_level = optimization_level
        sess_options.intra_op_num_threads = self._get_optimal_threads()
        sess_options.inter_op_num_threads = 2
        sess_options.execution_mode = ort.ExecutionMode.SEQUENTIAL  # 低内存
        sess_options.enable_mem_pattern = True
        sess_options.enable_mem_reuse = True
        
        # 创建会话
        self.session = ort.InferenceSession(
            model_path,
            sess_options,
            providers=[self.execution_provider]
        )
        
        # 元数据
        self.input_meta = self.session.get_inputs()[0]
        self.output_meta = self.session.get_outputs()[0]
        self.model_name = model_path.split('/')[-1]
        
        print(f"[EdgeInference] Model loaded: {self.model_name}")
        print(f"[EdgeInference] Provider: {self.execution_provider}")
        print(f"[EdgeInference] Input shape: {self.input_meta.shape}")
    
    def _detect_best_provider(self, requested: str) -> str:
        """自动检测最佳执行后端"""
        if requested != 'auto':
            return requested
        
        available = ort.get_available_providers()
        
        priority_order = [
            'TensorrtExecutionProvider',   # NVIDIA GPU (最快)
            'CUDAExecutionProvider',        # NVIDIA GPU
            'OpenVINOExecutionProvider',    # Intel CPU/GPU/NPU
            'CoreMLExecutionProvider',      # Apple Silicon
            'XNNPACKExecutionProvider',     # ARM/移动端 CPU
            'CPUExecutionProvider'          # 通用 CPU
        ]
        
        for provider in priority_order:
            if provider in available:
                return provider
        
        return 'CPUExecutionProvider'
    
    def _get_optimal_threads(self) -> int:
        """根据硬件确定最优线程数"""
        import os
        try:
            cpu_count = os.cpu_count() or 4
            return min(cpu_count, 4)  # 边缘设备通常不超过 4 核
        except:
            return 2
    
    def preprocess(self, image: np.ndarray, target_size: Tuple[int, int] = (640, 640)) -> np.ndarray:
        """
        图像预处理流水线（针对 YOLOv8 优化）
        """
        start_time = time.time()
        
        # 1. 缩放（保持宽高比，填充至目标尺寸）
        h, w = image.shape[:2]
        scale = min(target_size[0] / h, target_size[1] / w)
        new_h, new_w = int(h * scale), int(w * scale)
        
        resized = cv2.resize(image, (new_w, new_h), interpolation=cv2.INTER_LINEAR)
        
        # 填充（letterbox）
        padded = np.full((target_size[0], target_size[1], 3), 114, dtype=np.uint8)
        padded[(target_size[0] - new_h)//2:(target_size[0] - new_h)//2 + new_h,
               (target_size[1] - new_w)//2:(target_size[1] - new_w)//2 + new_w] = resized
        
        # 2. 格式转换 HWC → CHW
        transposed = padded.transpose(2, 0, 1)
        
        # 3. 归一化 [0,255] → [0,1]
        normalized = transposed.astype(np.float32) / 255.0
        
        # 4. 添加 batch 维度
        batched = np.expand_dims(normalized, axis=0)
        
        preprocess_time = (time.time() - start_time) * 1000
        return batched, preprocess_time
    
    def infer(self, input_data: np.ndarray) -> InferenceResult:
        """
        执行推理
        """
        pre_start = time.time()
        processed_input, preprocess_time = self.preprocess(input_data)
        
        # 推理
        infer_start = time.time()
        outputs = self.session.run(
            None,
            {self.input_meta.name: processed_input}
        )
        inference_time = (time.time() - infer_start) * 1000
        
        post_start = time.time()
        raw_output = outputs[0]
        
        # 后处理（YOLOv8 NMS）
        predictions, confidences = self._postprocess_yolo(raw_output)
        postprocess_time = (time.time() - post_start) * 1000
        
        return InferenceResult(
            predictions=predictions,
            confidence_scores=confidences,
            inference_time_ms=inference_time,
            preprocess_time_ms=preprocess_time,
            postprocess_time_ms=postprocess_time,
            model_name=self.model_name,
            input_shape=processed_input.shape
        )
    
    def _postprocess_yolo(self, output: np.ndarray, 
                            conf_threshold: float = 0.25,
                            iou_threshold: float = 0.45) -> Tuple[np.ndarray, np.ndarray]:
        """YOLOv8 后处理：转置 + NMS"""
        
        # 转置输出 (1,84,8400) → (8400, 84)
        output = output.transpose(0, 2, 1)[0]
        
        # 提取类别分数
        class_scores = output[:, 4:]  # (8400, num_classes)
        max_scores = np.max(class_scores, axis=1)
        
        # 置信度过滤
        mask = max_scores > conf_threshold
        detections = output[mask]
        scores = max_scores[mask]
        
        if len(detections) == 0:
            return np.array([]), np.array([])
        
        # NMS (非极大值抑制)
        boxes = detections[:, :4]
        class_ids = np.argmax(detections[:, 4:], axis=1)
        
        indices = cv2.dnn.NMSBoxes(
            bboxes=boxes.tolist(),
            scores=scores.tolist(),
            score_threshold=conf_threshold,
            nms_threshold=iou_threshold
        )
        
        final_boxes = boxes[indices]
        final_scores = scores[indices]
        final_classes = class_ids[indices]
        
        return final_boxes, final_scores
    
    def benchmark(self, iterations: int = 100, warmup: int = 10) -> Dict:
        """性能基准测试"""
        import statistics
        
        dummy_input = np.random.randint(0, 255, (640, 640, 3), dtype=np.uint8)
        
        # Warmup
        for _ in range(warmup):
            self.infer(dummy_input)
        
        # Benchmark
        times = []
        for _ in range(iterations):
            result = self.infer(dummy_input)
            times.append(result.inference_time_ms)
        
        return {
            'model': self.model_name,
            'provider': self.execution_provider,
            'iterations': iterations,
            'mean_latency_ms': round(statistics.mean(times), 2),
            'p50_ms': round(np.percentile(times, 50), 2),
            'p95_ms': round(np.percentile(times, 95), 2),
            'p99_ms': round(np.percentile(times, 99), 2),
            'throughput_fps': round(1000 / statistics.mean(times), 1),
            'input_shape': list(result.input_shape)
        }


# ===== 使用示例 =====

if __name__ == "__main__":
    engine = EdgeInferenceEngine(
        model_path="models/yolov8n-edge.onnx",
        execution_provider="auto"  # 自动选择最优后端
    )
    
    # 单次推理测试
    test_image = np.zeros((640, 640, 3), dtype=np.uint8)
    result = engine.infer(test_image)
    
    print(f"\nInference Result:")
    print(f"  Predictions: {len(result.predictions)} objects")
    print(f"  Latency: {result.inference_time_ms:.2f}ms")
    print(f"  Total Pipeline: {result.preprocess_time_ms + result.inference_time_ms + result.postprocess_time_ms:.2f}ms")
    
    # 性能基准
    bench = engine.benchmark(iterations=200)
    print(f"\nBenchmark Results:")
    for key, value in bench.items():
        print(f"  {key}: {value}")
```

---

## 📡 5G MEC 应用开发

### 低延迟应用架构

```python
# mec_application.py - 5G MEC 应用框架

from fastapi import FastAPI, Request, Response
from fastapi.middleware.gzip import GZipMiddleware
from typing import Dict, List
from datetime import datetime
import asyncio
import time
import json

app = FastAPI(title="Hermes MEC Application", version="1.0.0")

# Gzip 压缩（5G 带宽优化）
app.add_middleware(GZipMiddleware, minimum_size=100)

# ===== 5G 网络信息接口 =====

class NetworkContext5G:
    """
    5G 网络上下文感知
    
    通过 NEF (Network Exposure Function) 获取：
    - 用户位置
    - 网络切片信息
    - QoS 保证
    - UE 移动速度
    """
    
    def __init__(self, nef_endpoint: str):
        self.nef_endpoint = nef_endpoint
        self.cache_ttl = 1  # 秒（高频更新）
        self._context_cache = {}
    
    async def get_ue_context(self, ue_ip: str) -> Dict:
        """获取用户设备上下文"""
        cache_key = f"ue:{ue_ip}"
        
        if cache_key in self._context_cache:
            cached = self._context_cache[cache_key]
            if time.time() - cached['timestamp'] < self.cache_ttl:
                return cached['data']
        
        # 从 NEF 获取最新上下文
        context = await self._fetch_from_nef(f"/ue-context/{ue_ip}")
        
        self._context_cache[cache_key] = {
            'data': context,
            'timestamp': time.time()
        }
        
        return context
    
    async def get_network_slice_info(self, slice_id: str) -> Dict:
        """获取网络切片信息"""
        return await self._fetch_from_nef(f"/slices/{slice_id}")
    
    async def _fetch_from_nef(self, path: str) -> Dict:
        """实际调用 NEF API（此处模拟）"""
        await asyncio.sleep(0.001)  # 模拟网络延迟 (< 1ms for MEC local)
        
        return {
            'ue_location': {'lat': 39.9042, 'lon': 116.4074},
            'network_slice': {
                'id': 'slice-urllc-001',
                'type': 'URLLC',
                'guaranteed_bw_ul': 10000000,  # 10 Mbps uplink
                'guaranteed_bw_dl': 50000000,  # 50 Mbps downlink
                'max_latency_ms': 5,
                'reliability': '99.999%'
            },
            'ue_velocity_kmh': 30,
            'signal_quality_dBm': -75
        }

# ===== MEC API 端点 =====

@app.get("/api/v1/mec/status")
async def mec_status():
    """MEC 应用状态端点"""
    return {
        "status": "active",
        "deployment": "edge-node-beijing-05",
        "latency_to_upf_ms": 0.8,
        "cpu_usage_percent": 35,
        "memory_usage_mb": 280,
        "gpu_utilization_percent": 12,
        "active_sessions": 42
    }

@app.post("/api/v1/mec/process")
async def process_with_low_latency(request: Request):
    """
    低延迟处理端点
    
    针对 URLLC 场景优化：
    - 目标延迟: < 10ms (端到端)
    - 可靠性: 99.999%
    """
    start_time = time.perf_counter()
    
    data = await request.json()
    
    # 1. 获取网络上下文（本地获取，< 1ms）
    network_ctx = await get_network_context(request.client.host)
    
    # 2. 执行业务逻辑（边缘计算）
    result = await execute_edge_processing(data, network_ctx)
    
    # 3. 记录延迟指标
    processing_time = (time.perf_counter() - start_time) * 1000
    
    response_headers = {
        "X-MEC-Latency-ms": f"{processing_time:.2f}",
        "X-Network-Slice": network_ctx.get('slice_id', 'default'),
        "X-QoS-Class": "URLLC"
    }
    
    return JSONResponse(
        content=result,
        headers=response_headers
    )

@app.websocket("/ws/mec/stream")
async def websocket_stream(websocket: WebSocket):
    """
    实时数据流 WebSocket 端点
    
    用于 AR/VR、远程控制等超低延迟场景
    """
    await websocket.accept()
    
    try:
        while True:
            # 接收数据帧
            data = await websocket.receive_bytes()
            
            # 边缘处理（目标 < 5ms）
            processed = await process_frame(data)
            
            # 发送结果
            await websocket.send_bytes(processed)
            
    except WebSocketDisconnect:
        pass


# ===== 边缘函数示例 =====

@app.edge_function(path="/api/image-process")
async def edge_image_processor(event):
    """
    Cloudflare Workers / Vercel Edge 函数风格
    
    在全球边缘节点执行图像处理
    """
    image_data = event.request.body
    
    # 边缘图像优化
    optimized = await optimize_image_for_edge(image_data, format='webp', quality=80)
    
    return Response(
        content=optimized,
        media_type="image/webp",
        headers={
            "Cache-Control": "public, max-age=86400",
            "CDN-Cache-Control": "public, s-maxage=604800",
            "Edge-Cache-TTL": "3600"
        }
    )
```

---

## 🔗 相关技能

- [Hermes-Skill-IoT-Architect](../iot/iot-architect/SKILL.md) - IoT 与边缘计算结合
- [Hermes-Skill-Cloud-Architect](../cloud/cloud-architect/SKILL.md) - 云边协同架构
- [Hermes-Skill-Performance-Optimizer](../performance/performance-optimizer/SKILL.md) - 边缘性能调优

---

## 📊 技能统计

| 指标 | 数值 |
|------|------|
| 边缘平台 | K3s, KubeEdge, AWS Greengrass, Azure IoT Edge, Cloudflare |
| AI 推理 | ONNX Runtime, TensorRT, OpenVINO, TFLite, CoreML |
| 5G 集成 | MEC, NEF, Network Slicing, URLLC |
| 代码示例 | Python, Go, YAML, JavaScript |
| 延迟目标 | < 10ms (边缘推理), < 5ms (MEC 处理) |
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
**适用场景**: 边缘 AI | IoT 网关 | CDN 优化 | 5G MEC | 实时数据处理
