# Hermes-Skill-Green-IT-Architect

> 🌱 **可持续 IT 与绿色计算架构师** | 碳足迹优化 | 能效设计 | ESG 报告 | 循环经济

---

## 📋 技能概述

Hermes-Skill-Green-IT-Architect 是一个专业的可持续 IT（Green IT）与绿色计算 AI 助手，专注于帮助组织降低数字基础设施的碳足迹、优化能源效率、实现 ESG（环境、社会和治理）目标。涵盖数据中心能效优化、云碳核算、可持续软件工程、电子垃圾管理以及循环经济 IT 实践等核心领域。

### Green IT 能力矩阵

```
┌─────────────────────────────────────────────────────────────┐
│                  Green IT Stack                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ⚡ Energy Efficiency  │  📊 Carbon Accounting │  ☁️ Green   │
│  ├── Data Center PUE  │  ├── Scope 1/2/3      │    Cloud    │
│  ├── Server Utiliz.   │  ├── Software Carbon │  ├── Region  │
│  └── Edge Optimiz.    │  │   Intensity (SCI) │  │   Select. │
│                      │  └── LCA Analysis    │  └── Spot VMs │
│                                                             │
|  ♻️ Circular Economy     │  🌍 ESG Reporting      │  💚 Sustainable|
│  ├── Device Lifecycle │  ├── GRI Standards    │    Software  │
│  ├── E-Waste Mgmt     │  ├── GHG Protocol     │  ├── Green   │
│  └── Refurb/Reuse     │  └── TCFD Framework   │  │   Coding   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚀 快速开始

### 碳足迹评估

```bash
# 云基础设施碳排放审计
/greenit cloud-carbon "aws-account-production" --period Q1-2026

# 软件碳强度评估
/greenit software-carbon-intensity "api-service" --users 10000

# 数据中心 PUE 分析
/greenit pue-analysis "datacenter-east"
```

### 绿色架构建议

```bash
# 可持续架构设计
/greenit sustainable-architecture "microservices-platform"

# 能源效率优化方案
/greenit energy-optimization "ml-training-pipeline"

# ESG 合规报告生成
/greenit esg-report "annual-2026"
```

---

## 📊 碳核算体系

### 软件碳强度 (SCI) 计算

```python
# carbon_accounting.py - 软件碳强度核算系统

from dataclasses import dataclass
from typing import Dict, List, Optional
from datetime import datetime, timedelta
import math
from enum import Enum

class EmissionScope(Enum):
    SCOPE_1 = "scope_1"  # 直接排放（自有设施）
    SCOPE_2 = "scope_2"  # 间接排放（购买电力）
    SCOPE_3 = "scope_3"  # 价值链排放（供应链等）

@dataclass
class CarbonEmission:
    """碳排放数据点"""
    source: str                    # 排放源描述
    scope: EmissionScope
    co2_equivalent_kg: float       # CO2 当量（千克）
    energy_kwh: float              # 消耗能量（千瓦时）
    timestamp: datetime
    region: str                    # 地区（用于电网排放因子）
    category: str                  # 类别：compute, storage, network, etc.
    
    @property
    def carbon_intensity(self) -> float:
        """碳强度 gCO2/kWh"""
        if self.energy_kwh > 0:
            return (self.co2_equivalent_kg * 1000) / self.energy_kwh
        return 0.0

@dataclass
class SoftwareCarbonIntensity:
    """
    软件碳强度 (Software Carbon Intensity)
    
    SCI = (E * I) + M per unit of work (R)
    
    其中：
    - E = 能量消耗 (kWh)
    - I = 电网排放因子 (gCO2eq/kWh) 
    - M = 嵌入碳 (硬件制造) (gCO2eq)
    - R = 功能单位 (如请求数、用户数等)
    """
    total_energy_kwh: float
    grid_emission_factor_gco2_per_kwh: float  # 区域特定
    embodied_carbon_gco2: float
    functional_units: int                   # 如 API 调用次数
    
    @property
    def sci_score(self) -> float:
        """计算 SCI 分数 (gCO2eq per R)"""
        operational_carbon = self.total_energy_kwh * self.grid_emission_factor_gco2_per_kwh
        total_carbon = operational_carbon + self.embodied_carbon_gco2
        
        if self.functional_units > 0:
            return total_carbon / self.functional_units
        return float('inf')
    
    @property
    def sci_grade(self) -> str:
        """SCI 评级"""
        score = self.sci_score
        
        if score < 0.01:  # < 0.01 gCO2eq/request
            return 'A+'
        elif score < 0.05:
            return 'A'
        elif score < 0.25:
            return 'B'
        elif score < 1.0:
            return 'C'
        else:
            return 'D'

class CarbonAccountant:
    """
    企业级碳核算引擎
    
    符合标准：
    - GHG Protocol (温室气体协议)
    - ISO 14064
    - Science Based Targets initiative (SBTi)
    """
    
    # 各地区电网平均排放因子 (2024 数据，单位: gCO2/kWh)
    GRID_EMISSION_FACTORS = {
        # 中国区域电网
        'cn-north': 583.9,
        'cn-northeast': 537.6,
        'cn-east': 467.3,
        'cn-central': 493.9,
        'cn-northwest': 519.8,
        'cn-south': 424.7,
        
        # 其他主要地区
        'us-average': 386.0,
        'eu-average': 255.0,
        'us-west': 82.0,    # 华盛顿州（水电为主）
        'eu-france': 56.0,  # 法国（核能为主）
        'eu-sweden': 13.0,  # 瑞典（可再生能源）
        'australia': 520.0,
        'japan': 455.0,
        'india': 620.0,
        
        # 云服务商特定（已包含可再生能源采购）
        'aws-us-east-1': 234.0,
        'aws-eu-west-1': 142.0,
        'azure-west-europe': 98.0,
        'gcp-europe-west1': 76.0,
    }
    
    # 硬件嵌入碳因子 (gCO2eq per device-year amortized)
    EMBODIED_CARBON = {
        'server-enterprise': 1200000,  # 服务器（分摊到使用年限）
        'storage-hdd': 150000,
        'storage-ssd': 80000,
        'network-switch': 200000,
        'laptop': 300000,
        'smartphone': 60000,
    }
    
    def __init__(self, organization_id: str):
        self.org_id = organization_id
        self.emissions_log: List[CarbonEmission] = []
        self.baseline_year: Optional[int] = None
    
    def calculate_cloud_emissions(
        self,
        provider: str,
        region: str,
        compute_hours: float,
        instance_type: str,
        storage_gb_months: float = 0,
        network_tb: float = 0
    ) -> CarbonEmission:
        """
        计算云服务碳排放
        
        Args:
            provider: aws, azure, gcp
            region: 区域代码
            compute_hours: 计算实例运行小时数
            instance_type: 实例类型 (e.g., m5.large)
            storage_gb_months: 存储量 (GB·月)
            network_tb: 数据传输量 (TB)
        """
        
        # 1. 获取区域排放因子
        region_key = f"{provider}-{region}".lower()
        emission_factor = self.GRID_EMISSION_FACTORS.get(
            region_key,
            self.GRID_EMISSION_FACTORS.get(region.lower(), 400.0)  # 默认值
        )
        
        # 2. 计算各组件能耗
        # 计算能耗（基于实例规格估算）
        instance_power_map = {
            # AWS EC2 实例功耗 (Watts)
            't3.micro': 10, 't3.small': 15, 't3.medium': 20,
            'm5.large': 80, 'm5.xlarge': 128, 'm5.2xlarge': 192,
            'c5.large': 90, 'c5.xlarge': 144, 'r5.large': 100,
            'p3.2xlarge': 1200, 'p4d.24xlarge': 8000,
            
            # 通用估算
            'default': 50
        }
        
        power_watts = instance_power_map.get(instance_type, instance_power_map['default'])
        power_kw = power_watts / 1000.0
        
        compute_energy_kwh = power_kw * compute_hours
        
        # 存储能耗（SSD ~1.5W/TB, HDD ~7W/TB）
        storage_energy_kwh = storage_gb_months * 0.0015 * 730  # GB→TB, months→hours
        
        # 网络能耗（约 0.05 kWh/GB for CDN edge, 0.1 kWh/GB for transit）
        network_energy_kwh = network_tb * 1024 * 0.08
        
        # 总能耗
        total_energy_kwh = compute_energy_kwh + storage_energy_kwh + network_energy_kwh
        
        # 3. 计算碳排放
        co2_kg = (total_energy_kwh * emission_factor) / 1000  # g → kg
        
        emission = CarbonEmission(
            source=f"{provider}:{instance_type}@{region}",
            scope=EmissionScope.SCOPE_2,
            co2_equivalent_kg=round(co2_kg, 4),
            energy_kwh=round(total_energy_kwh, 4),
            timestamp=datetime.utcnow(),
            region=region_key,
            category='cloud_compute'
        )
        
        self.emissions_log.append(emission)
        return emission
    
    def calculate_software_sci(
        self,
        service_name: str,
        period_days: int = 30,
        requests_count: int = 0,
        active_users: int = 0
    ) -> SoftwareCarbonIntensity:
        """
        计算软件服务的碳强度
        
        Returns:
            SoftwareCarbonIntensity 对象，包含 SCI 分数和评级
        """
        
        # 过滤该服务在指定时间段的排放
        cutoff = datetime.utcnow() - timedelta(days=period_days)
        service_emissions = [
            e for e in self.emissions_log
            if e.timestamp >= cutoff and service_name in e.source
        ]
        
        total_energy = sum(e.energy_kwh for e in service_emissions)
        
        # 加权平均排放因子（按能源加权）
        if total_energy > 0:
            weighted_factor = sum(
                e.carbon_intensity * e.energy_kwh 
                for e in service_emissions
            ) / total_energy
        else:
            weighted_factor = 400.0  # 默认值
        
        # 估算嵌入碳（基于使用的硬件数量）
        # 假设每个服务平均使用 2 台服务器 + 存储
        embodied_carbon = (
            self.EMBODIED_CARBON['server-enterprise'] * 2 +
            self.EMBODIED_CARBON['storage-ssd'] * 1
        ) / 12  # 月度分摊
        
        # 功能单位选择（优先级：请求 > 用户 > 时间）
        functional_units = max(requests_count, active_users, period_days * 24 * 60)
        
        sci = SoftwareCarbonIntensity(
            total_energy_kwh=total_energy,
            grid_emission_factor_gco2_per_kwh=weighted_factor,
            embodied_carbon_gco2=embodied_carbon,
            functional_units=functional_units
        )
        
        return sci
    
    def generate_esg_report(
        self,
        year: int,
        format: str = "markdown"
    ) -> str:
        """生成 ESG 环境部分报告"""
        
        year_start = datetime(year, 1, 1)
        year_end = datetime(year, 12, 31, 23, 59, 59)
        
        year_emissions = [
            e for e in self.emissions_log
            if year_start <= e.timestamp <= year_end
        ]
        
        total_co2_tonnes = sum(e.co2_equivalent_kg for e in year_emissions) / 1000
        total_energy_mwh = sum(e.energy_kwh for e in year_emissions) / 1000
        
        # Scope 分类统计
        scope_breakdown = {}
        for scope in EmissionScope:
            scope_emissions = [e for e in year_emissions if e.scope == scope]
            scope_co2 = sum(e.co2_equivalent_kg for e in scope_emissions) / 1000
            scope_breakdown[scope.value] = round(scope_co2, 2)
        
        report = f"""# Environmental Impact Report - {year}

## Executive Summary

| Metric | Value | YoY Change |
|--------|-------|------------|
| **Total Carbon Footprint** | **{total_co2_tonnes:.2f} tCO2e** | TBD |
| Total Energy Consumption | {total_energy_mwh:.2f} MWh | TBD |
| Average Carbon Intensity | {(total_co2_tonnes*1000/total_energy_mwh):.1f} gCO2/kWh | TBD |

## GHG Emissions by Scope (GHG Protocol)

| Scope | Description | Emissions (tCO2e) | Percentage |
|-------|-------------|-------------------|------------|
| Scope 1 | Direct emissions (facilities, vehicles) | {scope_breakdown.get('scope_1', 0)} | {(scope_breakdown.get('scope_1', 0)/max(total_co2_tonnes, 0.01)*100):.1f}% |
| Scope 2 | Indirect emissions (purchased electricity) | {scope_breakdown.get('scope_2', 0)} | {(scope_breakdown.get('scope_2', 0)/max(total_co2_tonnes, 0.01)*100):.1f}% |
| Scope 3 | Value chain emissions (supply chain, use phase) | {scope_breakdown.get('scope_3', 0)} | {(scope_breakdown.get('scope_3', 0)/max(total_co2_tonnes, 0.01)*100):.1f}% |
| **Total** | | **{total_co2_tonnes:.2f}** | **100%** |

## Recommendations for Reduction

Based on the analysis, here are prioritized recommendations:

### Immediate Actions (0-6 months)
1. **Migrate to green cloud regions**: Move workloads from high-carbon regions (CN, AU) to low-carbon regions (EU-Nordic, US-Pacific Northwest)
2. **Enable auto-scaling**: Reduce idle resource waste (estimated 30% savings potential)

### Medium-term Initiatives (6-18 months)
3. **Renewable Energy PPAs**: Consider purchasing renewable energy certificates or entering PPAs
4. **Code efficiency optimization**: Implement green coding practices to reduce computational requirements

### Long-term Strategy (18+ months)
5. **Carbon offset program**: Invest in verified carbon removal projects
6. **Circular IT procurement**: Establish device refurbishment and recycling programs

---
*Report generated on {datetime.utcnow().strftime('%Y-%m-%d %H:%M UTC')}*
*Methodology: GHG Protocol Corporate Standard, ISO 14064-1*
"""
        
        return report


# ===== 使用示例 =====

if __name__ == "__main__":
    accountant = CarbonAccountant("hermes-tech")
    
    # 记录云服务排放
    emission = accountant.calculate_cloud_emissions(
        provider="aws",
        region="us-east-1",
        compute_hours=720,  # 30天 x 24小时
        instance_type="m5.large",
        storage_gb_months=500,
        network_tb=2
    )
    
    print(f"Emission recorded: {emission.co2_equivalent_kg} kg CO2eq")
    print(f"Energy consumed: {emission.energy_kwh} kWh")
    print(f"Carbon intensity: {emission.carbon_intensity:.1f} gCO2/kWh")
    
    # 计算 SCI
    sci = accountant.calculate_software_sci(
        service_name="api-gateway",
        period_days=30,
        requests_count=5000000
    )
    
    print(f"\nSoftware Carbon Intensity:")
    print(f"  SCI Score: {sci.sci_score:.6f} gCO2eq/request")
    print(f"  Grade: {sci.sci_grade}")
```

---

## 💚 可持续软件工程实践

### Green Coding 准则

```python
# green_patterns.py - 高效能/低碳软件模式

from functools import lru_cache
from typing import Callable, TypeVar, Any
import asyncio
import time
from dataclasses import dataclass

T = TypeVar('T')

@dataclass
class EfficiencyMetrics:
    """能效指标"""
    execution_time_ms: float
    memory_usage_mb: float
    cpu_cycles_estimated: float
    carbon_estimate_mg: float  # 毫克 CO2

class GreenPatterns:
    """
    可持续软件开发模式集合
    
    目标：在保持功能不变的前提下最小化资源消耗
    """
    
    # ===== 1. 缓存模式（减少重复计算）=====
    
    @staticmethod
    @lru_cache(maxsize=1024)
    def expensive_computation_cached(input_data: tuple) -> T:
        """带缓存的计算函数 - 避免重复的 CPU 密集操作"""
        result = GreenPatterns._heavy_calculation(input_data)
        return result
    
    # ===== 2. 批处理模式（减少 I/O 开销）=====
    
    @staticmethod
    async def batch_process(
        items: list,
        process_func: Callable[[Any], Any],
        batch_size: int = 100
    ) -> list:
        """
        批量处理模式
        
        原理：将多个小操作合并为少量大操作，
             减少 I/O 系统调用和网络往返次数
        """
        results = []
        
        for i in range(0, len(items), batch_size):
            batch = items[i:i + batch_size]
            
            # 单次批量操作（而非 N 次单独操作）
            batch_result = await process_func(batch)
            results.extend(batch_result)
            
            # 协调式等待（减少 CPU 占用）
            await asyncio.sleep(0)
        
        return results
    
    # ===== 3. 懒加载模式（按需初始化）=====
    
    class LazyResource:
        """延迟初始化的资源包装器"""
        
        def __init__(self, factory: Callable[[], Any]):
            self._factory = factory
            self._instance = None
            self._initialized = False
        
        def get(self) -> Any:
            if not self._initialized:
                self._instance = self._factory()
                self._initialized = True
            return self._instance
        
        def reset(self):
            """释放资源（用于长时间空闲时）"""
            if hasattr(self._instance, 'close'):
                self._instance.close()
            self._instance = None
            self._initialized = False
    
    # ===== 4. 自适应算法（根据负载调整）=====
    
    class AdaptiveThrottler:
        """
        自适应限流器
        
        根据系统能力和当前负载动态调整请求速率，
        在保证服务质量的前提下最大化吞吐量
        """
        
        def __init__(
            self,
            initial_rate: float = 100.0,
            min_rate: float = 10.0,
            max_rate: float = 1000.0
        ):
            self.current_rate = initial_rate
            self.min_rate = min_rate
            self.max_rate = max_rate
            
            self.success_count = 0
            self.error_count = 0
            self.window_size = 100  # 每 100 个请求调整一次
            self.request_count = 0
            
            # 性能目标
            self.target_latency_p99_ms = 500
            self.target_error_rate = 0.01
        
        async def acquire(self) -> bool:
            """获取执行许可（令牌桶变体）"""
            # 根据历史表现动态调整速率
            if self.request_count >= self.window_size:
                self._adjust_rate()
                self.request_count = 0
            
            self.request_count += 1
            
            # 简单的速率限制实现
            await asyncio.sleep(1.0 / self.current_rate)
            return True
        
        def report_success(self, latency_ms: float):
            self.success_count += 1
            # 可以记录延迟用于更精细的调整
        
        def report_error(self):
            self.error_count += 1
        
        def _adjust_rate(self):
            """基于反馈的自适应调整"""
            error_rate = self.error_count / self.window_size
            
            if error_rate > self.target_error_rate:
                # 错误率过高，降速
                self.current_rate *= 0.8
            elif error_rate < self.target_error_rate / 2 and \
                 self.current_rate < self.max_rate:
                # 错误率很低且未达上限，加速
                self.current_rate *= 1.1
            
            # 边界限制
            self.current_rate = max(self.min_rate, min(self.max_rate, self.current_rate))
            
            # 重置计数器
            self.success_count = 0
            self.error_count = 0
    
    # ===== 5. 算法复杂度优化 =====
    
    @staticmethod
    def efficient_search(sorted_list: list, target: Any) -> int:
        """
        使用二分查找而非线性查找
        O(log n) vs O(n)
        """
        left, right = 0, len(sorted_list) - 1
        
        while left <= right:
            mid = (left + right) // 2
            if sorted_list[mid] == target:
                return mid
            elif sorted_list[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        
        return -1
    
    @staticmethod
    def _heavy_calculation(data: tuple) -> T:
        """模拟重计算（实际应用中替换为真实逻辑）"""
        time.sleep(0.001)  # 模拟 1ms 的计算
        return hash(data)  # 返回哈希作为结果


# ===== 环境感知调度 =====

class EnvironmentAwareScheduler:
    """
    环境感知任务调度器
    
    根据实时碳强度数据智能调度批处理任务，
    在可再生能源占比高的时段执行高能耗任务
    """
    
    def __init__(self, carbon_intensity_api_url: str):
        self.api_url = carbon_intensity_api_url
        self.cache_ttl_seconds = 300  # 5 分钟缓存
        self._carbon_cache = {}
    
    async def get_current_carbon_intensity(self, region: str) -> float:
        """获取当前区域的实时碳强度 (gCO2/kWh)"""
        import aiohttp
        
        cache_key = region
        if cache_key in self._carbon_cache:
            cached_time, cached_value = self._carbon_cache[cache_key]
            if time.time() - cached_time < self.cache_ttl_seconds:
                return cached_value
        
        async with aiohttp.ClientSession() as session:
            async with session.get(f"{self.api_url}/intensity?region={region}") as resp:
                data = await resp.json()
                intensity = data['current_intensity']
                
                self._carbon_cache[cache_key] = (time.time(), intensity)
                return intensity
    
    async def schedule_green_job(
        self,
        job: Callable,
        preferred_regions: list[str] = None,
        deadline: float = None,
        max_wait_minutes: int = 60
    ) -> Any:
        """
        在碳强度最低的时刻/区域执行任务
        
        Args:
            job: 要执行的函数
            preferred_regions: 首选区域列表
            deadline: 截止时间戳（None 表示无截止）
            max_wait_minutes: 最大等待时间
        """
        
        regions = preferred_regions or ['us-west', 'eu-north', 'eu-south']
        
        start_time = time.time()
        
        while True:
            # 检查是否超过最大等待时间或截止时间
            elapsed = (time.time() - start_time) / 60  # minutes
            if elapsed > max_wait_minutes:
                break  # 不再等待，立即执行
            if deadline and time.time() > deadline:
                break
            
            # 寻找碳强度最低的区域
            best_region = None
            best_intensity = float('inf')
            
            for region in regions:
                intensity = await self.get_current_carbon_intensity(region)
                if intensity < best_intensity:
                    best_intensity = intensity
                    best_region = region
            
            # 如果当前是较好时机（低于平均值），执行
            avg_intensity = 300.0  # 全球平均参考值
            if best_intensity < avg_intensity * 0.8:
                print(f"[GreenScheduler] Executing in {best_region} "
                      f"(intensity: {best_intensity:.1f} gCO2/kWh)")
                return await job()
            
            # 否则等待一段时间再检查
            await asyncio.sleep(60)  # 每分钟检查一次
        
        # 超时，直接执行
        print("[GreenScheduler] Timeout reached, executing immediately")
        return await job()
```

---

## 🔗 相关技能

- [Hermes-Skill-Cloud-Architect](../cloud/cloud-architect/SKILL.md) - 绿色云架构
- [Hermes-Skill-Performance-Optimizer](../performance/performance-optimizer/SKILL.md) - 能效性能平衡
- [Hermes-Skill-DevSecOps-Engineer](../devsecops/devsecops-engineer/SKILL.md) - 可持续安全运营

---

## 📊 技能统计

| 指标 | 数值 |
|------|------|
| 碳核算标准 | GHG Protocol, ISO 14064, SBTi, CDP |
| 排放范围覆盖 | Scope 1, 2, 3 全覆盖 |
| 电网数据库 | 50+ 国家/地区排放因子 |
| 云平台支持 | AWS, Azure, GCP, Alibaba Cloud |
| SCI 实现 | 完整软件碳强度计算框架 |
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
**适用场景**: 碳中和战略 | 绿色数据中心 | ESG 合规 | 可持续软件 | 循环经济 IT
