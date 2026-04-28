# Hermes-Skill-Game-Developer

> 🎮 **专业游戏开发助手** | Unity/Unreal/Godot 全平台支持 | 从原型到上线的一站式解决方案

---

## 📋 技能概述

Hermes-Skill-Game-Developer 是一个专业的游戏开发 AI 助手，专注于提供跨平台游戏开发的完整解决方案。涵盖 Unity、Unreal Engine、Godot 等主流引擎，支持 2D/3D 游戏、VR/AR 应用、移动端和主机游戏的开发全流程。

### 核心能力

```
┌─────────────────────────────────────────────────────────────┐
│                    Game Development Stack                   │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  🎯 Game Design     │  💻 Programming    │  🎨 Art & Audio │
│  ├── Mechanics      │  ├── C#/C++        │  ├── 2D Assets  │
│  ├── Systems        │  ├── Blueprints    │  ├── 3D Models  │
│  ├── Balance        │  ├── GDScript      │  ├── Animation  │
│  └── Progression    │  └── Shaders       │  └── SFX/Music  │
│                                                             │
│  🔧 Technical       │  📱 Platforms      │  🚀 Production   │
│  ├── Physics        │  ├── Mobile (iOS)  │  ├── Prototyping │
│  ├── Networking     │  ├── Android       │  ├── Iteration  │
│  ├── AI/Behavior    │  ├── PC/Desktop    │  └── Launch     │
│  └── Optimization   │  └── Console       │                 │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚀 快速开始

### 基础使用

```bash
# 游戏类型咨询
/game-dev what-type "casual puzzle game for mobile"

# 技术栈推荐
/game-dev recommend-stack "3D RPG with multiplayer"

# 架构设计
/game-dev architecture "MMORPG server-client"

# 性能优化建议
/game-dev optimize "Unity mobile game 60fps target"
```

### 引擎特定命令

```bash
# Unity 专项
/game-dev unity "implement inventory system with scriptable objects"

# Unreal 专项
/game-dev unreal "create character ability system with gameplay abilities"

# Godot 专项
/game-dev godot "setup 2D platformer controller with state machine"
```

---

## 🎨 游戏设计系统

### 游戏类型模板库

#### 1. Casual Puzzle (休闲益智)

```csharp
// Unity: Match-3 核心逻辑
public class Match3Grid : MonoBehaviour
{
    [SerializeField] private int gridSize = 8;
    [SerializeField] private GemType[] gemTypes;
    private Gem[,] grid;
    
    public async Task<bool> TrySwap(Vector2Int pos1, Vector2Int pos2)
    {
        // 1. 验证交换合法性
        if (!IsAdjacent(pos1, pos2)) return false;
        
        // 2. 执行交换
        SwapGems(pos1, pos2);
        
        // 3. 检测匹配
        var matches = FindMatches();
        if (matches.Count == 0)
        {
            // 无匹配，撤销交换
            SwapGems(pos1, pos2);
            return false;
        }
        
        // 4. 处理消除连锁反应
        await ProcessMatches(matches);
        return true;
    }
}
```

#### 2. Action RPG (动作角色扮演)

```cpp
// Unreal: 角色能力系统
UCLASS()
class UAbilitySystemComponent : public UActorComponent
{
    GENERATED_BODY()
    
public:
    // 能力激活
    UFUNCTION(BlueprintCallable, Category = "Abilities")
    virtual bool TryActivateAbility(FGameplayAbilitySpecHandle Handle);
    
    // 效果应用
    UFUNCTION(BlueprintCallable, Category = "Abilities")
    virtual FActiveGameplayEffectHandle ApplyGameplayEffectToSelf(
        TSubclassOf<UGameplayEffect> GameplayEffectClass,
        float Level,
        FGameplayEffectContextHandle Context
    );
    
    // 属性修改器
    UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Attributes")
    TArray<FGameplayAttributeData> Attributes;
};
```

#### 3. Survival Craft (生存建造)

```gdscript
# Godot: 物品合成系统
extends Node

class_name CraftingSystem

var recipes := {
    "wooden_pickaxe": {
        "ingredients": {"wood": 3, "stone": 2},
        "result": "wooden_pickaxe",
        "craft_time": 2.0
    },
    "torch": {
        "ingredients": {"wood": 1, "coal": 1},
        "result": "torch",
        "craft_time": 0.5
    }
}

func can_craft(recipe_id: String, inventory: Dictionary) -> bool:
    var recipe = recipes.get(recipe_id)
    if not recipe:
        return false
    
    for ingredient, amount in recipe.ingredients.items():
        if inventory.get(ingredient, 0) < amount:
            return false
    return true

func craft(recipe_id: String, inventory: Dictionary) -> Dictionary:
    if not can_craft(recipe_id, inventory):
        return {"success": false, "error": "Insufficient materials"}
    
    var recipe = recipes[recipe_id]
    # 消耗材料
    for ingredient, amount in recipe.ingredients.items():
        inventory[ingredient] -= amount
    
    # 添加产物
    inventory[recipe.result] = inventory.get(recipe.result, 0) + 1
    
    return {
        "success": true,
        "result": recipe.result,
        "craft_time": recipe.craft_time
    }
```

---

## 💻 编程架构模式

### MVC-MVVM 游戏架构

```
┌─────────────────────────────────────────────────────────────┐
│                    Architecture Layers                      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Presentation Layer (表现层)                                 │
│  ├── Views (UI Components)                                  │
│  ├── Animators (Animation Controllers)                      │
│  └── Particle Systems                                       │
│         │                                                   │
│         ▼                                                   │
│  Logic Layer (逻辑层)                                        │
│  ├── Controllers (Input Handling)                           │
│  ├── ViewModels (State Management)                          │
│  └── Services (Business Logic)                              │
│         │                                                   │
│         ▼                                                   │
│  Data Layer (数据层)                                         │
│  ├── Models (Data Structures)                               │
│  ├── Repositories (Data Access)                             │
│  └── Persistence (Save/Load)                                │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### ECS (Entity Component System)

```csharp
// Unity DOTS: 高性能实体组件系统
public struct MovementData : IComponentData
{
    public float3 Velocity;
    public float Speed;
    public float3 Direction;
}

public struct HealthData : IComponentData
{
    public int CurrentHealth;
    public int MaxHealth;
    public bool IsAlive => CurrentHealth > 0;
}

[RequireComponent(typeof(MovementData))]
public partial struct MovementSystem : SystemBase
{
    protected override void OnUpdate()
    {
        float deltaTime = Time.DeltaTime;
        
        Entities
            .ForEach((ref MovementData movement, in Translation position) =>
            {
                // 更新位置
                position.Value += movement.Velocity * movement.Speed * deltaTime;
                
                // 边界检测
                position.Value = math.clamp(position.Value, 
                    new float3(-50, 0, -50), 
                    new float3(50, 10, 50)
                );
            })
            .ScheduleParallel();
    }
}
```

### State Machine (状态机)

```gdscript
# Godot: 角色状态机
class_name CharacterStateMachine extends Node

enum State { IDLE, WALK, RUN, JUMP, ATTACK, HURT, DIE }

var current_state := State.IDLE
var previous_state := State.IDLE
var state_timer := 0.0

func _physics_process(delta):
    match current_state:
        State.IDLE:
            handle_idle(delta)
        State.WALK:
            handle_walk(delta)
        State.RUN:
            handle_run(delta)
        State.JUMP:
            handle_jump(delta)
        State.ATTACK:
            handle_attack(delta)
        State.HURT:
            handle_hurt(delta)
        State.DIE:
            handle_die(delta)

func transition_to(new_state: State):
    exit_state(current_state)
    previous_state = current_state
    current_state = new_state
    state_timer = 0.0
    enter_state(current_state)
```

---

## 🔧 技术实现指南

### 性能优化清单

```
┌─────────────────────────────────────────────────────────────┐
│                  Performance Optimization                   │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  CPU Optimization                                           │
│  ├── ✓ Object Pooling (对象池)                              │
│  ├── ✓ LOD System (细节层次)                                │
│  ├── ✓ Frustum Culling (视锥剔除)                           │
│  ├── ✓ Occlusion Culling (遮挡剔除)                         │
│  └── ✓ Job System/Burst Compiler (并行计算)                 │
│                                                             │
│  GPU Optimization                                           │
│  ├── ✓ Draw Call Batching (绘制调用批处理)                   │
│  ├── ✓ Texture Atlasing (纹理图集)                          │
│  ├── ✓ Shader Optimization (着色器优化)                     │
│  ├── ✓ GPU Instancing (GPU 实例化)                          │
│  └── ✓ Async Loading (异步加载)                             │
│                                                             │
│  Memory Optimization                                         │
│  ├── ✓ Asset Bundles (资源包)                               │
│  ├── ✓ Addressables (可寻址资产)                            │
│  ├── ✓ Texture Compression (纹理压缩)                       │
│  └── ✓ Mesh Compression (网格压缩)                          │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 跨平台适配

```csharp
// 平台特定代码示例
#if UNITY_IOS
    // iOS 特定优化
    Application.targetFrameRate = 60;
    QualitySettings.globalTextureMipmapLimit = 1;
#elif UNITY_ANDROID
    // Android 设备分级
    var deviceTier = GetDevicePerformanceTier();
    switch (deviceTier)
    {
        case DeviceTier.High:
            QualitySettings.SetQualityLevel(QualityLevel.High);
            break;
        case DeviceTier.Medium:
            QualitySettings.SetQualityLevel(QualityLevel.Medium);
            break;
        case DeviceTier.Low:
            QualitySettings.SetQualityLevel(QualityLevel.Low);
            break;
    }
#endif
```

---

## 🌐 多人游戏网络架构

### Client-Server 模型

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Client A   │     │   Server     │     │   Client B   │
│              │     │              │     │              │
│  Input →─────┼────→│  Receive     │←────┼─── Input →    │
│              │     │  Validate    │     │              │
│  State ←────┼────←│  Simulate    │────┼─── State ←    │
│              │     │  Broadcast   │     │              │
│  Render      │     │  Authoritative│    │  Render      │
└──────────────┘     └──────────────┘     └──────────────┘
       ↑                    ↑                    ↑
       │                    │                    │
       └────────────────────┼────────────────────┘
                            │
                     Network Protocol
                     (TCP/UDP/WebSocket)
```

### 同步算法

```csharp
// 客户端预测 + 服务端校正
public class NetworkTransform : MonoBehaviour
{
    private Queue<StatePayload> stateBuffer = new();
    private Vector3 lastServerPosition;
    private Quaternion lastServerRotation;
    
    public void OnClientPrediction(InputPayload input)
    {
        // 1. 客户端立即预测
        var predictedState = PhysicsSimulate(input);
        transform.position = predictedState.position;
        transform.rotation = predictedState.rotation;
        
        // 2. 缓存状态用于回滚
        stateBuffer.Enqueue(predictedState);
    }
    
    public void OnServerReconciliation(StatePayload serverState)
    {
        // 3. 接收服务端权威状态
        lastServerPosition = serverState.position;
        lastServerRotation = serverState.rotation;
        
        // 4. 计算偏差并回滚重模拟
        if (Vector3.Distance(transform.position, serverState.position) > reconciliationThreshold)
        {
            ReconcileAndResimulate(serverState);
        }
    }
}
```

---

## 📱 移动端特殊考虑

### 触控输入系统

```csharp
// Unity: 虚拟摇杆实现
public class VirtualJoystick : MonoBehaviour, IDragHandler, IEndDragHandler
{
    [SerializeField] private RectTransform joystickBackground;
    [SerializeField] private RectTransform joystickHandle;
    [SerializeField] private float handleRange = 50f;
    
    public Vector2 InputDirection { get; private set; }
    
    public void OnDrag(PointerEventData eventData)
    {
        Vector2 position;
        if (RectTransformUtility.ScreenPointToLocalPointInEventSystem(
            eventData.position, 
            eventData.pressEventCamera, 
            out position))
        {
            // 计算方向并限制范围
            var delta = position - joystickBackground.anchoredPosition;
            delta = Vector2.ClampMagnitude(delta, handleRange);
            
            joystickHandle.anchoredPosition = delta;
            InputDirection = delta / handleRange;
        }
    }
    
    public void OnEndDrag(PointerEventData eventData)
    {
        joystickHandle.anchoredPosition = Vector2.zero;
        InputDirection = Vector2.zero;
    }
}
```

### 电量与性能平衡

```csharp
// 自适应质量系统
public class AdaptiveQualityManager : MonoBehaviour
{
    [SerializeField] private int targetFrameRate = 30;
    [SerializeField] private float batteryThreshold = 0.2f;
    
    private void Update()
    {
        var batteryLevel = SystemInfo.batteryLevel;
        var currentFPS = 1f / Time.unscaledDeltaTime;
        
        if (batteryLevel < batteryThreshold || currentFPS < targetFrameRate * 0.8f)
        {
            EnablePowerSavingMode();
        }
        else
        {
            EnableNormalMode();
        }
    }
    
    private void EnablePowerSavingMode()
    {
        QualitySettings.shadowResolution = ShadowResolution.Low;
        QualitySettings.particleCount = ParticleSystemSetting.Low;
        Application.targetFrameRate = targetFrameRate / 2;
    }
}
```

---

## 🎵 音频系统设计

### 3D 音频空间化

```csharp
// Unity: 动态音频混合器
public class DynamicAudioMixer : MonoBehaviour
{
    [SerializeField] private AudioMixer masterMixer;
    [SerializeField] private AudioSource musicSource;
    [SerializeField] private AudioSource sfxSource;
    
    public void SetMusicVolume(float volume)
    {
        // 使用对数刻度（分贝）
        var dbVolume = volume > 0 ? 20f * Mathf.Log10(volume) : -80f;
        masterMixer.SetFloat("MusicVolume", dbVolume);
    }
    
    public void PlaySFX(AudioClip clip, Vector3 worldPosition)
    {
        // 根据 3D 位置播放音效
        AudioSource.PlayClipAtPoint(clip, worldPosition, 1f);
    }
    
    public void ApplyAudioEffects(AudioSource source, AudioEffectType effect)
    {
        switch (effect)
        {
            case AudioEffectType.Underwater:
                source.outputAudioMixerGroup?.audioMixer.SetFloat("LowPass", 2000f);
                break;
            case AudioEffectType.Muffled:
                source.outputAudioMixerGroup?.audioMixer.SetFloat("HighPass", 500f);
                break;
        }
    }
}
```

---

## 🧪 测试策略

### 自动化测试框架

```csharp
// Unity Test Framework 示例
[TestFixture]
public class InventorySystemTests
{
    private InventorySystem inventory;
    
    [SetUp]
    public void SetUp()
    {
        inventory = new InventorySystem(maxSlots: 20);
    }
    
    [Test]
    public void AddItem_WhenInventoryNotFull_ReturnsSuccess()
    {
        var item = CreateTestItem();
        var result = inventory.AddItem(item);
        
        Assert.IsTrue(result.Success);
        Assert.AreEqual(1, inventory.ItemCount);
    }
    
    [Test]
    public void AddItem_WhenInventoryFull_ReturnsFailure()
    {
        // 填满背包
        for (int i = 0; i < 20; i++)
        {
            inventory.AddItem(CreateTestItem());
        }
        
        var result = inventory.AddItem(CreateTestItem());
        
        Assert.IsFalse(result.Success);
        Assert.AreEqual("Inventory is full", result.ErrorMessage);
    }
}
```

---

## 📦 资源管理

### Addressable Asset System

```csharp
// Unity Addressables 异步加载
public class AssetLoader : MonoBehaviour
{
    public async UniTask<GameObject> LoadAssetAsync(string address)
    {
        try
        {
            var handle = Addressables.LoadAssetAsync<GameObject>(address);
            await handle.Task;
            
            if (handle.Status == AsyncOperationStatus.Succeeded)
            {
                return handle.Result;
            }
            else
            {
                Debug.LogError($"Failed to load asset: {address}");
                return null;
            }
        }
        catch (Exception ex)
        {
            Debug.LogError($"Exception loading asset: {ex.Message}");
            return null;
        }
    }
    
    public void ReleaseAsset(object asset)
    {
        Addressables.Release(asset);
    }
}
```

---

## 🎯 最佳实践

### 代码规范

```
✅ DO:
  - 使用 ScriptableObject 配置数据
  - 实现对象池减少 GC
  - 分离 View 和 Logic
  - 使用事件解耦模块
  - 编写单元测试

❌ DON'T:
  - 在 Update 中使用 Find/GetComponent
  - 使用字符串引用组件
  - 忽略移动端性能
  - 硬编码数值
  - 单文件超过 500 行
```

### 项目结构推荐

```
Assets/
├── _Project/
│   ├── Scripts/
│   │   ├── Core/           # 核心系统
│   │   ├── Gameplay/       # 游戏玩法
│   │   ├── UI/             # 用户界面
│   │   ├── Network/        # 网络
│   │   └── Utils/          # 工具类
│   ├── Resources/
│   │   ├── Prefabs/        # 预制体
│   │   ├── Materials/      # 材质
│   │   └── ScriptableObjects/  # 可配置对象
│   └── Settings/           # 项目设置
├── Plugins/                # 第三方插件
└── StreamingAssets/        # 流式资源
```

---

## 🔗 相关技能

- [Hermes-Skill-Performance-Optimizer](../performance/performance-optimizer/SKILL.md) - 性能优化深度指南
- [Hermes-Skill-Mobile-App-Guide](../development/mobile-app-guide/SKILL.md) - 移动端开发最佳实践
- [Hermes-Skill-BI-Dashboard-Builder](../business/bi-dashboard-builder/SKILL.md) - 游戏数据分析仪表盘

---

## 📊 技能统计

| 指标 | 数值 |
|------|------|
| 支持引擎 | Unity, Unreal Engine, Godot, Defold |
| 游戏类型覆盖 | 15+ 类型模板 |
| 代码示例数量 | 50+ |
| 架构模式 | MVC, MVVM, ECS, State Machine |
| 平台支持 | iOS, Android, PC, Console, Web |
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
**适用场景**: 游戏开发全流程 | 跨平台发布 | 团队协作
