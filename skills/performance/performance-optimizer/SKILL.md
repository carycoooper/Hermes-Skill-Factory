# Hermes-Skill-Performance-Optimizer

> ⚡ **全栈性能优化专家** | Web/Mobile/Backend | 从加载到渲染的端到端优化

---

## 📋 技能概述

Hermes-Skill-Performance-Optimizer 是一个专业的全栈性能优化 AI 助手，专注于提供从前端加载、后端响应到数据库查询的全方位性能优化方案。涵盖 Core Web Vitals、移动端性能、API 响应时间、数据库优化等关键领域。

### 性能优化能力矩阵

```
┌─────────────────────────────────────────────────────────────┐
│                Performance Optimization Stack               │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  🌐 Frontend         │  ⚙️ Backend          │  🗄️ Data      │
│  ├── Core Web Vitals │  ├── API Latency     │  ├── Query    │
│  ├── Bundle Size     │  ├── Caching Strategy│  │   Optimize │
│  ├── Rendering       │  ├── Async Processing│  ├── Indexing │
│  └── Resource Load   │  └── Queue Systems   │  └── Sharding │
│                                                             │
│  📱 Mobile           │  🔍 Monitoring       │  🛠️ Tools     │
│  ├── App Start Time  │  ├── APM             │  ├── Lighthouse│
│  ├── Battery Usage   │  ├── Profiling       │  ├── Chrome   │
│  └── Memory Mgmt     │  └── Alerting        │  │   DevTools │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚀 快速开始

### 性能诊断

```bash
# 网站性能分析
/perf analyze "https://example.com"

# 移动端应用性能检测
/perf mobile-audit "com.example.app"

# API 响应时间分析
/perf api-latency "POST /api/orders"

# 数据库慢查询分析
/perf db-slow-query "production_db"
```

### 优化建议生成

```bash
# LCP 优化方案
/perf optimize-lcp "hero-section-image"

# FID/INP 改进策略
/perf improve-interaction "checkout-form"

# Bundle 体积缩减
/perf reduce-bundle "vendor-chunk"
```

---

## 🌐 Core Web Vitals 优化

### 指标体系详解

```
┌─────────────────────────────────────────────────────────────┐
│                  Core Web Vitals                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  LCP (Largest Contentful Paint)                             │
│  目标: ≤ 2.5 秒                                             │
│  测量: 页面最大内容元素渲染时间                               │
│  影响: 首屏加载感知速度                                      │
│                                                             │
│  INP (Interaction to Next Paint) [取代 FID]                 │
│  目标: ≤ 200 毫秒                                           │
│  测量: 用户交互到下一帧绘制的延迟                             │
│  影响: 页面交互响应性                                        │
│                                                             │
│  CLS (Cumulative Layout Shift)                              │
│  目标: ≤ 0.1                                                │
│  测量: 视觉稳定性（意外布局偏移）                             │
│  影响: 用户体验流畅度                                        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### LCP 优化实战

```javascript
// lcp_optimizer.js - LCP 全面优化方案

// 1. 预加载关键资源
const preloadCriticalResources = () => {
  // 预连接到关键域名
  const preconnectDomains = [
    'https://cdn.example.com',
    'https://api.example.com'
  ];
  
  preconnectDomains.forEach(domain => {
    const link = document.createElement('link');
    link.rel = 'preconnect';
    link.href = domain;
    link.crossOrigin = 'anonymous';
    document.head.appendChild(link);
  });
  
  // 预加载首屏图片
  const heroImage = document.querySelector('[data-hero-image]');
  if (heroImage) {
    const preloadLink = document.createElement('link');
    preloadLink.rel = 'preload';
    preloadLink.as = 'image';
    preloadLink.href = heroImage.dataset.src;
    document.head.appendChild(preloadLink);
  }
};

// 2. 优化图片加载策略
const optimizeImageLoading = () => {
  const images = document.querySelectorAll('img[data-src]');
  
  const imageObserver = new IntersectionObserver((entries, observer) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        const img = entry.target;
        
        // 使用 srcset 实现响应式图片
        if (img.dataset.srcset) {
          img.srcset = img.dataset.srcset;
        }
        
        // 加载图片
        img.src = img.dataset.src;
        img.removeAttribute('data-src');
        
        // 加载完成后移除观察
        img.onload = () => observer.unobserve(img);
      }
    });
  }, {
    rootMargin: '50px',  // 提前 50px 开始加载
    threshold: 0.01
  });
  
  images.forEach(img => imageObserver.observe(img));
};

// 3. 服务端渲染 + 客户端 hydration
const optimizeServerRendering = async () => {
  // 关键 CSS 内联
  const criticalCSS = await fetch('/critical-css.css').then(r => r.text());
  const style = document.createElement('style');
  style.textContent = criticalCSS;
  document.head.insertBefore(style, document.head.firstChild);
  
  // 非关键 JS 异步加载
  const scripts = document.querySelectorAll('script[data-defer]');
  scripts.forEach(script => {
    script.defer = true;
  });
};

// 4. 使用 Performance Observer 监控 LCP
let lcpEntry = null;

const observeLCP = new PerformanceObserver((entryList) => {
  const entries = entryList.getEntries();
  const lastEntry = entries[entries.length - 1];
  
  if (lastEntry) {
    lcpEntry = lastEntry;
    
    // 发送到 analytics
    sendToAnalytics({
      metric: 'LCP',
      value: lastEntry.startTime,
      element: lastTask.element?.tagName || 'unknown'
    });
  }
});

observeLCP.observe({ type: 'largest-contentful-paint', buffered: true });

// 页面隐藏时保存最终 LCP
document.addEventListener('visibilitychange', () => {
  if (lcpEntry && document.visibilityState === 'hidden') {
    sendFinalMetric('LCP', lcpEntry.startTime);
  }
});
```

### INP 优化策略

```typescript
// inp_optimizer.ts - 交互响应优化

type InteractionType = 'click' | 'keydown' | 'pointerdown';

interface InteractionEvent {
  type: InteractionType;
  startTime: number;
  target: EventTarget;
  duration: number;
}

class INPOptimizer {
  private interactions: InteractionEvent[] = [];
  private longTaskThreshold = 50; // ms
  
  constructor() {
    this.init();
  }
  
  private init() {
    // 1. 监控所有用户交互
    ['click', 'keydown', 'pointerdown'].forEach(eventType => {
      document.addEventListener(eventType, this.handleInteraction.bind(this), {
        passive: true,
        capture: true
      });
    });
    
    // 2. 监控长任务
    if ('PerformanceObserver' in window) {
      try {
        const observer = new PerformanceObserver((list) => {
          list.getEntries().forEach(entry => {
            console.warn(`Long Task detected: ${entry.duration}ms`);
            this.reportLongTask(entry.duration);
          });
        });
        observer.observe({ entryTypes: ['longtask'] });
      } catch (e) {
        // Long Task API 不支持
      }
    }
  }
  
  private handleInteraction(event: Event) {
    const startTime = performance.now();
    const target = event.target;
    
    // 使用 requestIdleCallback 或 requestAnimationFrame
    // 来调度非关键工作
    const processAfterPaint = () => {
      const endTime = performance.now();
      const duration = endTime - startTime;
      
      this.interactions.push({
        type: event.type as InteractionType,
        startTime,
        target,
        duration
      });
      
      // 如果交互超过阈值，记录并分析
      if (duration > 200) {  // INP 阈值
        this.analyzeSlowInteraction(event, duration);
      }
    };
    
    // 使用 requestPostCallback (如果支持)
    if ('scheduler' in window && 'postTask' in (window as any).scheduler) {
      (window as any).scheduler.postTask(processAfterPaint, {
        priority: 'user-blocking'
      });
    } else {
      requestAnimationFrame(processAfterPaint);
    }
  }
  
  private analyzeSlowInteraction(event: Event, duration: number) {
    // 分析慢交互原因
    const analysis = {
      eventType: event.type,
      target: (event.target as Element)?.tagName,
      duration: Math.round(duration),
      possibleCauses: [] as string[],
      recommendations: [] as string[]
    };
    
    // 常见原因分析
    if (duration > 500) {
      analysis.possibleCauses.push(
        'Main thread blocked by heavy computation',
        'Synchronous DOM manipulation',
        'Large render tree'
      );
      analysis.recommendations.push(
        'Consider Web Workers for CPU-intensive tasks',
        'Use virtualization for large lists',
        'Debounce/throttle rapid events'
      );
    }
    
    // 发送至监控
    this.sendToMonitoring(analysis);
  }
  
  // 优化技巧：使用 CSS containment 减少重排范围
  public applyContainment() {
    const containers = document.querySelectorAll('[data-contain]');
    containers.forEach(el => {
      el.style.contain = 'content';
    });
  }
  
  // 优化技巧：使用 will-change 提示浏览器优化
  public hintBrowserOptimizations() {
    const animatedElements = document.querySelectorAll('[data-animate]');
    animatedElements.forEach(el => {
      const animationType = el.dataset.animate;
      (el as HTMLElement).style.willChange = animationType;
    });
  }
}
```

### CLS 防护系统

```javascript
// cls_guard.js - 布局偏移防护

class CLSGuard {
  constructor() {
    this.clsScore = 0;
    this.init();
  }
  
  init() {
    // 1. 动态内容尺寸预留
    this.reserveSpaceForDynamicContent();
    
    // 2. 字体加载优化
    this.optimizeFontLoading();
    
    // 3. 图片尺寸稳定化
    this.stabilizeImageDimensions();
    
    // 4. 广告/第三方脚本隔离
    this.isolateThirdPartyScripts();
    
    // 5. 监控 CLS
    this.monitorCLS();
  }
  
  reserveSpaceForDynamicContent() {
    // 为动态内容预留固定空间
    const dynamicContainers = document.querySelectorAll('[data-dynamic-height]');
    
    dynamicContainers.forEach(container => {
      const minHeight = container.dataset.dynamicHeight || '200px';
      container.style.minHeight = minHeight;
      
      // 使用 min-height 而非 height，避免内容溢出问题
      container.style.overflow = 'hidden';
      
      // 内容加载完成后调整
      const observer = new MutationObserver(() => {
        requestAnimationFrame(() => {
          const actualHeight = container.scrollHeight;
          if (actualHeight > parseInt(minHeight)) {
            container.style.minHeight = `${actualHeight}px`;
          }
        });
      });
      
      observer.observe(container, { childList: true, subtree: true });
    });
  }
  
  optimizeFontLoading() {
    // 使用 font-display: swap + 预加载关键字体
    const criticalFonts = [
      '/fonts/inter-var.woff2'
    ];
    
    criticalFonts.forEach(fontUrl => {
      const link = document.createElement('link');
      link.rel = 'preload';
      link.as = 'font';
      link.href = fontUrl;
      link.crossOrigin = 'anonymous';
      document.head.appendChild(link);
    });
    
    // 为 font-display: swap 的字体设置 fallback 尺寸
    document.documentElement.style.setProperty(
      '--font-fallback-metrics',
      ' ascent-override 90%, descent-override 22%, line-gap-override 0%'
    );
  }
  
  stabilizeImageDimensions() {
    const images = document.querySelectorAll('img[src]:not([width]):not([height])');
    
    images.forEach(img => {
      // 如果没有明确尺寸，使用 aspect-ratio 或 padding hack
      if (!img.width && !img.height && img.dataset.aspectRatio) {
        img.style.aspectRatio = img.dataset.aspectRatio;
      }
    });
  }
  
  isolateThirdPartyScripts() {
    // 将第三方脚本放入 iframe 或使用 sandbox
    const thirdPartySelectors = '[data-third-party]';
    
    document.querySelectorAll(thirdPartySelectors).forEach(container => {
      // 创建 iframe 沙箱
      const iframe = document.createElement('iframe');
      iframe.sandbox.add('allow-scripts');
      iframe.style.cssText = `
        width: 100%;
        border: none;
        display: block;
      `;
      
      // 设置固定高度防止 CLS
      iframe.height = container.offsetHeight || 300;
      
      container.parentNode?.replaceChild(iframe, container);
    });
  }
  
  monitorCLS() {
    let clsValue = 0;
    
    const clsObserver = new PerformanceObserver((list) => {
      for (const entry of list.getEntries()) {
        if (!(entry as any).hadRecentInput) {
          clsValue += entry.value;
          
          // 发送实时 CLS 数据
          if (clsValue > 0.1) {  // 超过阈值警告
            console.warn(`CLS Warning: ${clsValue.toFixed(3)} > 0.1`);
            this.sendCLSBreachAlert(clsValue);
          }
        }
      }
    });
    
    clsObserver.observe({ type: 'layout-shift', buffered: true });
  }
}
```

---

## ⚙️ 后端性能优化

### API 响应时间优化

```python
# api_performance.py - FastAPI 性能优化示例

from fastapi import FastAPI, Request, Response
from fastapi.middleware.gzip import GZipMiddleware
from fastapi.responses import JSONResponse
import time
import asyncio
from functools import wraps
from typing import Callable
import aioredis
from prometheus_client import Counter, Histogram, generate_latest

app = FastAPI()

# Metrics
REQUEST_COUNT = Counter(
    'http_requests_total',
    'Total HTTP requests',
    ['method', 'endpoint', 'status']
)

REQUEST_LATENCY = Histogram(
    'http_request_duration_seconds',
    'HTTP request latency in seconds',
    ['method', 'endpoint']
)

# Redis 缓存客户端
redis = None

async def get_redis():
    global redis
    if redis is None:
        redis = await aioredis.from_url(
            "redis://localhost",
            decode_responses=True
        )
    return redis

# 中间件：请求计时与指标收集
@app.middleware("http")
async def add_process_time_header(request: Request, call_next: Callable):
    start_time = time.time()
    
    response = await call_next(request)
    
    process_time = time.time() - start_time
    
    # 记录指标
    REQUEST_COUNT.labels(
        method=request.method,
        endpoint=request.url.path,
        status=response.status_code
    ).inc()
    
    REQUEST_LATENCY.labels(
        method=request.method,
        endpoint=request.url.path
    ).observe(process_time)
    
    # 添加响应头
    response.headers["X-Process-Time"] = str(round(process_time * 1000, 2))
    
    return response

# 启用 Gzip 压缩
app.add_middleware(GZipMiddleware, minimum_size=1000)

def cache_response(ttl: int = 300):
    """
    响应缓存装饰器
    ttl: 缓存时间（秒）
    """
    def decorator(func):
        @wraps(func)
        async def wrapper(*args, **kwargs):
            # 生成缓存键
            cache_key = f"cache:{func.__name__}:{str(args)}:{str(kwargs)}"
            
            redis_client = await get_redis()
            
            # 尝试从缓存获取
            cached = await redis_client.get(cache_key)
            if cached:
                return JSONResponse(
                    content=json.loads(cached),
                    headers={"X-Cache": "HIT"}
                )
            
            # 执行原始函数
            result = await func(*args, **kwargs)
            
            # 存入缓存
            if isinstance(result, dict):
                await redis_client.setex(
                    cache_key,
                    ttl,
                    json.dumps(result)
                )
            
            return JSONResponse(
                content=result,
                headers={"X-Cache": "MISS"
            })
        )
        return wrapper
    return decorator

# 批量查询优化
@cache_response(ttl=60)
async def get_products_batch(product_ids: List[str]):
    """批量获取产品信息"""
    
    redis_client = await get_redis()
    
    # 1. 先从 Redis 批量获取
    cache_keys = [f"product:{pid}" for pid in product_ids]
    cached_products = await redis_client.mget(cache_keys)
    
    results = {}
    missing_ids = []
    
    for pid, cached in zip(product_ids, cached_products):
        if cached:
            results[pid] = json.loads(cached)
        else:
            missing_ids.append(pid)
    
    # 2. 批量查询缺失的数据
    if missing_ids:
        # 使用 WHERE IN 单次查询，而非 N+1 查询
        query = """
        SELECT id, name, price, stock 
        FROM products 
        WHERE id = ANY($1::uuid[])
        """
        
        rows = await database.fetch_all(query, missing_ids)
        
        for row in rows:
            product_data = dict(row)
            results[row['id']] = product_data
            
            # 更新缓存
            await redis_client.setex(
                f"product:{row['id']}",
                300,
                json.dumps(product_data)
            )
    
    return results

# 异步任务队列处理
@app.post("/api/orders")
async def create_order(order_data: dict):
    """异步订单创建"""
    
    # 快速返回订单 ID
    order_id = str(uuid.uuid4())
    
    # 后台处理订单逻辑
    asyncio.create_task(process_order_background(order_id, order_data))
    
    return JSONResponse(
        content={
            "order_id": order_id,
            "status": "processing",
            "message": "Order accepted for processing"
        },
        status_code=202
    )

async def process_order_background(order_id: str, order_data: dict):
    """后台处理订单"""
    try:
        # 库存检查
        await check_inventory(order_data.items)
        
        # 支付处理
        payment_result = await process_payment(order_data.payment)
        
        # 订单确认
        await confirm_order(order_id, payment_result)
        
        # 发送通知
        await send_confirmation_email(order_id, order_data.customer_email)
        
    except Exception as e:
        # 记录错误并标记订单失败
        await mark_order_failed(order_id, str(e))
        raise e
```

---

## 🗄️ 数据库性能优化

### 查询优化指南

```sql
-- 慢查询分析与优化

-- 1. EXPLAIN ANALYZE 分析执行计划
EXPLAIN (ANALYZE, BUFFERS, FORMAT TEXT)
SELECT 
    o.id,
    o.customer_id,
    o.total_amount,
    o.created_at,
    c.name as customer_name,
    c.email
FROM orders o
JOIN customers c ON o.customer_id = c.id
WHERE o.created_at >= '2026-01-01'
  AND o.status = 'completed'
ORDER BY o.created_at DESC
LIMIT 50;

-- 2. 优化后的索引策略
CREATE INDEX CONCURRENTLY idx_orders_customer_date_status 
ON orders (customer_id, created_at DESC, status)
WHERE status IN ('completed', 'processing');

-- 3. 分区表示例（按月分区）
CREATE TABLE orders (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    customer_id UUID NOT NULL REFERENCES customers(id),
    total_amount DECIMAL(10, 2) NOT NULL,
    status VARCHAR(20) NOT NULL DEFAULT 'pending',
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
) PARTITION BY RANGE (created_at);

-- 创建分区
CREATE TABLE orders_2026_01 PARTITION OF orders
    FOR VALUES FROM ('2026-01-01') TO ('2026-02-01');

CREATE TABLE orders_2026_02 PARTITION OF orders
    FOR VALUES FROM ('2026-02-01') TO ('2026-03-01');

-- 4. 物化视图用于聚合查询
CREATE MATERIALIZED VIEW mv_order_summary AS
SELECT 
    DATE_TRUNC('month', created_at) as month,
    status,
    COUNT(*) as order_count,
    SUM(total_amount) as total_revenue,
    AVG(total_amount) as avg_order_value
FROM orders
GROUP BY DATE_TRUNC('month', created_at), status
WITH DATA;

-- 刷新策略（定时或触发器）
CREATE OR REPLACE FUNCTION refresh_order_summary()
RETURNS void AS $$
BEGIN
    REFRESH MATERIALIZED VIEW CONCURRENTLY mv_order_summary;
END;
$$ LANGUAGE plpgsql;

-- 定时刷新（每天凌晨）
SELECT pg_cron.schedule(
    'refresh-order-summary',
    '0 3 * * *',  -- 每天 3:00 AM
    'SELECT refresh_order_summary()'
);
```

### 连接池配置

```python
# database.py - SQLAlchemy 异步连接池优化

from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession
from sqlalchemy.orm import sessionmaker
from contextlib import asynccontextmanager

engine = create_async_engine(
    "postgresql+asyncpg://user:pass@localhost/dbname",
    pool_size=20,              # 连接池大小
    max_overflow=10,           # 允许溢出的最大连接数
    pool_pre_ping=True,        # 连接前先 ping 检查
    pool_recycle=3600,         # 连接回收时间（秒）
    echo=False,                # 生产环境关闭 SQL 日志
    
    # 连接池事件监听
    connect_args={
        "server_settings": {
            "application_name": "hermes_api",
            "statement_timeout": "30000",  # 30 秒超时
        }
    }
)

AsyncSessionLocal = sessionmaker(
    engine,
    class_=AsyncSession,
    expire_on_commit=False
)

@asynccontextmanager
async def get_db_session():
    """优化的会话管理"""
    session = AsyncSessionLocal()
    try:
        yield session
        await session.commit()
    except Exception:
        await session.rollback()
        raise
    finally:
        await session.close()

# 使用示例
async def get_user_with_orders(user_id: str):
    async with get_db_session() as session:
        # 使用 selectinload 避免 N+1 问题
        result = await session.execute(
            select(User)
            .options(selectinload(User.orders).selectinload(Order.items))
            .where(User.id == user_id)
        )
        return result.scalar_one_or_none()
```

---

## 📱 移动端性能优化

### Android 性能最佳实践

```kotlin
// PerformanceOptimizer.kt

object PerformanceOptimizer {
    
    /**
     * 应用启动优化
     */
    fun optimizeAppStartup(application: Application) {
        // 1. 异步初始化非核心模块
        CoroutineScope(Dispatchers.Default).launch {
            initNonCriticalModules()
        }
        
        // 2. 预加载常用数据
        preloadEssentialData(application)
        
        // 3. 延迟加载重型组件
        application.registerActivityLifecycleCallbacks(
            object : ActivityLifecycleCallbacks {
                override fun onActivityCreated(activity: Activity, savedInstanceState: Bundle?) {
                    when (activity) {
                        is MainActivity -> {
                            // 主界面可见后再加载次要功能
                            activity.viewTreeObserver.addOnGlobalLayoutListener(object : 
                                ViewTreeObserver.OnGlobalLayoutListener {
                                override fun onGlobalLayout() {
                                    activity.viewTreeObserver.removeOnGlobalLayoutListener(this)
                                    loadSecondaryFeatures(activity)
                                }
                            })
                        }
                    }
                }
                
                override fun onActivityResumed(activity: Activity) {
                    // 恢复时预加载下一页数据
                    prefetchNextPageData(activity)
                }
                // ... 其他回调方法
            }
        )
    }
    
    /**
     * 内存优化
     */
    fun optimizeMemoryUsage(context: Context) {
        // 1. 使用内存缓存限制
        val memoryCacheSize = (Runtime.getRuntime().maxMemory() / 8).toInt()
        val bitmapCache = LruCache<String, Bitmap>(memoryCacheSize)
        
        // 2. 图片加载优化
        Glide.with(context)
            .applyDefaultRequestOptions(
                RequestOptions()
                    .diskCacheStrategy(DiskCacheStrategy.ALL)
                    .format(DecodeFormat.PREFER_RGB_565)  // 降低内存占用
                    .skipMemoryCache(true)  // 大图不缓存在内存
            )
        
        // 3. 泄漏检测（Debug 模式）
        if (BuildConfig.DEBUG) {
            LeakCanary.install(context.applicationContext as Application)
        }
    }
    
    /**
     * UI 渲染优化
     */
    fun optimizeUIRendering(activity: Activity) {
        // 1. 启用硬件加速
        activity.window.setFlags(
            WindowManager.LayoutParams.FLAG_HARDWARE_ACCELERATED,
            WindowManager.LayoutParams.FLAG_HARDWARE_ACCELERATED
        )
        
        // 2. 过度绘制检测
        if (BuildConfig.DEBUG) {
            activity.window.decorView.post {
                // 开发者选项中开启"显示过度绘制"
                Debug.startMethodTracing()
            }
        }
        
        // 3. RecyclerView 优化
        optimizeRecyclerView(activity)
    }
    
    private fun optimizeRecyclerView(activity: Activity) {
        val recyclerViews = findViewsOfType<RecyclerView>(activity)
        
        recyclerViews.forEach { rv ->
            rv.apply {
                // 固定大小提升性能
                setHasFixedSize(true)
                
                // 复用 ViewPool
                setRecycledViewPool(ViewPool(20))
                
                // ItemAnimator 优化
                itemAnimator = DefaultItemAnimator().apply {
                    changeDuration = 0
                    moveDuration = 150
                    addDuration = 250
                }
                
                // DiffUtil 增量更新
                (adapter as? BaseQuickAdapter<*, *>)?.setDiffCallback(MyDiffCallback())
            }
        }
    }
    
    /**
     * 网络层优化
     */
    fun optimizeNetworkLayer(): OkHttpClient {
        return OkHttpClient.Builder()
            // 连接池
            .connectionPool(ConnectionPool(5, 5, TimeUnit.MINUTES))
            
            // HTTP/2 多路复用
            .protocols(listOf(Protocol.H2_PRIOR_KNOWLEDGE, Protocol.HTTP_1_1))
            
            // 缓存配置
            .addInterceptor(CacheInterceptor(context.cacheDir, 10 * 1024 * 1024))
            
            // 请求压缩
            .addInterceptor(GzipRequestInterceptor())
            
            // 重试机制
            .retryOnConnectionFailure(true)
            
            // 超时配置
            .connectTimeout(10, TimeUnit.SECONDS)
            .readTimeout(15, TimeUnit.SECONDS)
            .writeTimeout(15, TimeUnit.SECONDS)
            
            .build()
    }
}
```

---

## 🔍 性能监控系统

### 自定义 Dashboard 配置

```yaml
# grafana_performance_dashboard.yaml
dashboard:
  title: Application Performance Dashboard
  panels:
    - title: Response Time P50/P95/P99
      type: graph
      targets:
        - expr: histogram_quantile(0.50, rate(http_request_duration_seconds_bucket[5m]))
          legend: P50
        - expr: histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m]))
          legend: P95
        - expr: histogram_quantile(0.99, rate(http_request_duration_seconds_bucket[5m]))
          legend: P99
      
    - title: Error Rate
      type: stat
      targets:
        - expr: |
            sum(rate(http_requests_total{status=~"5.."}[5m])) 
            / sum(rate(http_requests_total[5m])) * 100
            
    - title: Throughput (RPS)
      type: graph
      targets:
        - expr: sum(rate(http_requests_total[5m])) by (method)
          
    - title: Database Query Duration
      type: heatmap
      targets:
        - expr: rate(db_query_duration_seconds_sum[5m]) / rate(db_query_duration_seconds_count[5m])
          
    - title: Memory Usage
      type: graph
      targets:
        - expr: process_resident_memory_bytes / 1024 / 1024
```

---

## 🔗 相关技能

- [Hermes-Skill-Mobile-App-Guide](../development/mobile-app-guide/SKILL.md) - 移动端开发
- [Hermes-Skill-Cloud-Architect](../cloud/cloud-architect/SKILL.md) - 云架构性能设计
- [Hermes-Skill-BI-Dashboard-Builder](../business/bi-dashboard-builder/SKILL.md) - 性能监控仪表盘

---

## 📊 技能统计

| 指标 | 数值 |
|------|------|
| Core Web Vitals | LCP, INP, CLS 全覆盖 |
| 平台支持 | Web, iOS, Android, Server |
| 代码示例 | JavaScript, Python, Kotlin, SQL |
| 优化模式 | 30+ 种最佳实践 |
| 监控集成 | Prometheus, Grafana, APM |
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
**适用场景**: Web 性能 | 移动端优化 | API 加速 | 数据库调优
