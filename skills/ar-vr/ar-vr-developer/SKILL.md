# Hermes-Skill-AR-VR-Developer

> 🥽 **AR/VR/MR 全栈开发专家** | Unity/Unreal/WebXR | 空间计算 | 元宇宙应用

---

## 📋 技能概述

Hermes-Skill-AR-VR-Developer 是一个专业的扩展现实（XR）开发 AI 助手，涵盖 AR（增强现实）、VR（虚拟现实）、MR（混合现实）应用的全栈开发。支持 Unity、Unreal Engine、WebXR、Native SDK（ARKit/ARCore）等主流平台，以及空间交互设计、3D 资产优化、性能调优等核心领域。

### XR 开发能力矩阵

```
┌─────────────────────────────────────────────────────────────┐
│                XR Development Stack                         │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  🎮 Platforms        │  🛠️ Engines          │  📱 SDKs     │
│  ├── VR (Oculus/PSVR)│  ├── Unity (URP/HDRP)│  ├── ARKit   │
│  ├── AR (iOS/Android)│  ├── Unreal Engine  │  ├── ARCore  │
│  ├── MR (HoloLens)  │  ├── WebXR          │  ├── Vuforia │
│  └── WebXR Browsers │  └── Native (Swift) │  └── OpenXR  │
│                                                             │
│  🎨 3D Assets        │  ✋ Interaction       │  ⚡ Performance│
│  ├── Models/GLTF    │  ├── Hand Tracking   │  ├── 90fps+  │
│  ├── Textures/PBR   │  ├── Gaze/Raycast    │  ├── LOD      │
│  ├── Animations     │  ├── Voice Commands  │  └── Occlusion│
│  └── Shaders        │  └── Haptics         │                 │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚀 快速开始

### 项目初始化

```bash
# 创建新 XR 项目
/xr create-project "ar-product-configurator" --platform ios,android --engine unity

# 性能目标设置
/xr set-target "quest3" --fps 90 --resolution-per-eye 2064x2208

# 交互模式选择
/xr interaction-mode "hand-tracking + voice"
```

### 核心功能实现

```bash
# 空间锚点系统
/xr implement "spatial-anchors" --persistence cloud

# 对象抓取与放置
/xr implement "grab-and-place" --physics enabled

# 多人协作空间
/xr implement "multiplayer-sync" --network photon
```

---

## 🎯 AR 应用架构

### AR 基础框架 (Unity + AR Foundation)

```csharp
// ARSessionManager.cs - 核心 AR 会话管理

using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;
using System;
using System.Collections.Generic;

public class ARSessionManager : MonoBehaviour
{
    [Header("AR Components")]
    [SerializeField] private ARSession arSession;
    [SerializeField] private ARSessionOrigin arOrigin;
    [SerializeField] private AROcclusionManager occlusionManager;
    [SerializeField] private ARPlaneManager planeManager;
    [SerializeField] private ARRaycastManager raycastManager;
    [SerializeField] private ARAnchorManager anchorManager;
    
    [Header("Configuration")]
    [SerializeField] private ARRenderMode renderMode = ARRenderMode.Material;
    [SerializeField] private bool enableOcclusion = true;
    [SerializeField] private bool enablePlaneDetection = true;
    [SerializeField] private float maxRayDistance = 10f;
    
    // Events
    public event Action<ARPlanesChangedEventArgs> OnPlanesUpdated;
    public event Action<ARTapEventArgs> OnTappedInWorld;
    public event Action<ARRaycastHit> OnObjectPlaced;
    
    // State
    public List<ARPlane> DetectedPlanes => new List<ARPlane>(planeManager.trackables);
    public bool IsTracking => arSession.subsystem?.running ?? false;
    
    private Camera mainCamera;
    private List<ARRaycastHit> rayHits = new List<ARRaycastHit>();
    
    private void Awake()
    {
        mainCamera = arOrigin.camera;
        
        // Configure session
        ConfigureARSession();
    }
    
    private void Start()
    {
        // Enable required features
        if (enableOcclusion)
            EnableFeature<OcclusionManager>();
            
        if (enablePlaneDetection)
            EnableFeature<PlaneDetection>();
        
        // Subscribe to events
        planeManager.planesChanged += HandlePlanesChanged;
    }
    
    private void Update()
    {
        if (Input.touchCount > 0 && Input.GetTouch(0).phase == TouchPhase.Began)
        {
            PerformRaycast(Input.GetTouch(0).position);
        }
#if UNITY_EDITOR
        else if (Input.GetMouseButtonDown(0))
        {
            PerformRaycast(Input.mousePosition);
        }
#endif
    }
    
    private void ConfigureARSession()
    {
        var config = new ARSessionConfig
        {
            PlaneDetection = enablePlaneDetection ? 
                PlaneDetectionFlags.Horizontal | PlaneDetectionFlags.Vertical : 
                PlaneDetectionFlags.None,
            LightEstimation = LightEstimation.EnvironmentalHDR,
            UpdatesPerSecond = 60
        };
        
        arSession.GetComponent<ARSession>().enabled = true;
    }
    
    private void PerformRaycast(Vector2 screenPosition)
    {
        rayHits.Clear();
        
        if (raycastManager.Raycast(screenPosition, rayHits, TrackableType.PlaneWithinPolygon))
        {
            var hit = rayHits[0];
            
            OnTappedInWorld?.Invoke(new ARTapEventArgs
            {
                screenPosition = screenPosition,
                worldPosition = hit.pose.position,
                worldRotation = hit.pose.rotation,
                hitPlane = hit.trackable as ARPlane
            });
        }
    }
    
    public ARAnchor CreateAnchor(string anchorName, Pose pose, ARPlane plane = null)
    {
        var anchor = anchorManager.TryAddAnchor(pose);
        if (anchor != null)
        {
            anchor.gameObject.name = anchorName;
            
            if (plane != null)
            {
                // Parent to plane for automatic tracking
                anchor.transform.parent = plane.transform;
            }
            
            Debug.Log($"Created anchor: {anchorName} at {pose.position}");
        }
        
        return anchor;
    }
    
    public void PlaceObject(GameObject prefab, Vector3 position, Quaternion rotation)
    {
        var obj = Instantiate(prefab, position, rotation);
        OnObjectPlaced?.Invoke(new ARRaycastHit
        {
            pose = new Pose(position, rotation)
        });
    }
    
    public T EnableFeature<T>() where T : Component
    {
        var feature = GetComponent<T>() ?? gameObject.AddComponent<T>();
        feature.enabled = true;
        return feature;
    }
    
    public void SaveAnchorsToCloud()
    {
        // 实现云锚点保存逻辑（ARCore Cloud Anchors / ARKit World Map）
        Debug.Log("Saving anchors to cloud...");
    }
}

public class ARTapEventArgs : EventArgs
{
    public Vector2 screenPosition;
    public Vector3 worldPosition;
    public Quaternion worldRotation;
    public ARPlane hitPlane;
}
```

### 手部追踪与手势识别

```csharp
// HandInteractionSystem.cs - 手部追踪交互系统

using UnityEngine;
using UnityEngine.XR;
using UnityEngine.XR.Hands;
using System.Collections.Generic;

public class HandInteractionSystem : MonoBehaviour
{
    [Header("Hand Tracking")]
    [SerializeField] private XRHandTrackingEvents leftHandEvents;
    [SerializeField] private XRHandTrackingEvents rightHandEvents;
    
    [Header("Interaction Settings")]
    [SerializeField] private float pinchThreshold = 0.7f;
    [SerializeField] private float grabThreshold = 0.5f;
    [SerializeField] private LayerMask interactableLayer;
    [SerializeField] private float maxInteractionDistance = 0.5f;
    
    [Header("Visual Feedback")]
    [SerializeField] private Material hoverMaterial;
    [SerializeField] private LineRenderer rayLinePrefab;
    
    // State tracking
    private Dictionary<Handedness, HandState> handStates = new();
    private GameObject currentHoveredObject;
    private GameObject grabbedObject;
    
    private struct HandState
    {
        public bool isTracked;
        public bool isPinching;
        public bool isGrabbing;
        public Vector3 wristPosition;
        public Vector3 indexTipPosition;
        public Quaternion indexRotation;
        public float pinchAmount;
        public float grabAmount;
    }
    
    private void OnEnable()
    {
        SubscribeToHandEvents(leftHandEvents, Handedness.Left);
        SubscribeToHandEvents(rightHandEvents, Handedness.Right);
    }
    
    private void SubscribeToHandEvents(XRHandTrackingEvents events, Handedness handedness)
    {
        events.jointsUpdated.AddListener((data) => UpdateHandState(handedness, data));
        events.trackingLost.AddListener(() => LostTracking(handedness));
    }
    
    private void UpdateHandState(Handedness handedness, XRHandJointUpdateEventArgs args)
    {
        var state = new HandState
        {
            isTracked = true,
            wristPosition = args.handData[XRHandJointID.Wrist].Position,
            indexTipPosition = args.handData[XRHandJointID.IndexTip].Position,
            indexRotation = args.handData[XRHandJointID.IndexTip].Rotation,
            pinchAmount = CalculatePinchAmount(args.handData),
            grabAmount = CalculateGrabAmount(args.handData)
        };
        
        state.isPinching = state.pinchAmount > pinchThreshold;
        state.isGrabbing = state.grabAmount > grabThreshold;
        
        handStates[handedness] = state;
        
        ProcessInteractions(handedness, state);
    }
    
    private float CalculatePinchAmount(XRHandJointData jointData)
    {
        var thumbTip = jointData[XRHandJointID.ThumbTip].Position;
        var indexTip = jointData[XRHandJointID.IndexTip].Position;
        
        var distance = Vector3.Distance(thumbTip, indexTip);
        return Mathf.Clamp01(1f - (distance / 0.05f)); // Normalize based on typical pinch distance
    }
    
    private float CalculateGrabAmount(XRHandJointData jointData)
    {
        var middleTip = jointData[XRHandJointID.MiddleTip].Position;
        var palmCenter = jointData[XRHandJointID.MiddleMetacarpal].Position;
        
        var distance = Vector3.Distance(middleTip, palmCenter);
        return Mathf.Clamp01(1f - (distance / 0.1f));
    }
    
    private void ProcessInteractions(Handedness handedness, HandState state)
    {
        Ray ray = new Ray(state.wristPosition, 
                          (state.indexTipPosition - state.wristPosition).normalized);
        
        RaycastHit hit;
        if (Physics.Raycast(ray, out hit, maxInteractionDistance, interactableLayer))
        {
            // Hover effect
            if (currentHoveredObject != hit.collider.gameObject)
            {
                ClearHoverEffect();
                currentHoveredObject = hit.collider.gameObject;
                ApplyHoverEffect(currentHoveredObject);
            }
            
            // Pinch interaction
            if (state.isPinching && grabbedObject == null)
            {
                StartGrab(hit.collider.gameObject, handedness, state.indexTipPosition);
            }
            else if (!state.isPinching && grabbedObject != null)
            {
                EndGrab();
            }
            
            // Grabbing - move object with hand
            if (grabbedObject != null && state.isGrabbing)
            {
                MoveGrabbedObject(state.indexTipPosition, state.indexRotation);
            }
        }
        else
        {
            ClearHoverEffect();
            
            if (!state.isPinching && grabbedObject != null)
            {
                EndGrab();
            }
        }
    }
    
    private void StartGrab(GameObject obj, Handedness hand, Vector3 grabPoint)
    {
        grabbedObject = obj;
        
        var interactable = obj.GetComponent<I XRGrabbable>();
        interactable?.OnGrabStart(hand, grabPoint);
        
        Debug.Log($"Started grabbing: {obj.name} with {hand}");
    }
    
    private void MoveGrabbedObject(Vector3 position, Quaternion rotation)
    {
        if (grabbedObject == null) return;
        
        // Smooth follow
        grabbedObject.transform.position = Vector3.Lerp(
            grabbedObject.transform.position,
            position,
            Time.deltaTime * 15f
        );
        
        grabbedObject.transform.rotation = Quaternion.Slerp(
            grabbedObject.transform.rotation,
            rotation,
            Time.deltaTime * 15f
        );
    }
    
    private void EndGrab()
    {
        if (grabbedObject != null)
        {
            var interactable = grabbedObject.GetComponent<IXRGrabbable>();
            interactable?.OnGrabEnd();
            
            Debug.Log($"Released: {grabbedObject.name}");
            grabbedObject = null;
        }
    }
    
    private void ApplyHoverEffect(GameObject obj)
    {
        var renderer = obj.GetComponent<Renderer>();
        if (renderer && hoverMaterial)
        {
            // Store original material for restoration
            renderer.material = hoverMaterial;
        }
    }
    
    private void ClearHoverEffect()
    {
        // Restore original material logic here
        currentHoveredObject = null;
    }
    
    private void LostTracking(Handedness handedness)
    {
        if (handStates.ContainsKey(handedness))
        {
            handStates[handedness].isTracked = false;
        }
        
        if (grabbedObject != null)
        {
            EndGrab();
        }
    }
}
```

---

## 🌐 WebXR 开发

### 基于 Three.js 的 WebXR 场景

```javascript
// webxr_scene.js - WebXR VR/AR 场景框架

import * as THREE from 'three';
import { VRButton } from 'three/examples/jsm/webxr/VRButton.js';
import { ARButton } from 'three/examples/jsm/webxr/ARButton.js';
import { XRControllerModelFactory } from 'three/examples/jsm/webxr/XRControllerModelFactory.js';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';

class HermesWebXRScene {
    constructor(container, options = {}) {
        this.container = container;
        this.options = {
            mode: options.mode || 'vr', // 'vr' or 'ar'
            antialias: options.antialias !== false,
            ...options
        };
        
        this.scene = null;
        this.camera = null;
        this.renderer = null;
        this.controllers = [];
        this.interactiveObjects = [];
        this.raycaster = new THREE.Raycaster();
        this.tempMatrix = new THREE.Matrix4();
        
        this.init();
    }
    
    init() {
        // Scene setup
        this.scene = new THREE.Scene();
        this.scene.background = new THREE.Color(0x808080);
        
        // Camera
        this.camera = new THREE.PerspectiveCamera(
            70,
            window.innerWidth / window.innerHeight,
            0.01,
            20
        );
        
        // Renderer with XR support
        this.renderer = new THREE.WebGLRenderer({
            antialias: this.options.antialias,
            alpha: this.options.mode === 'ar'
        });
        this.renderer.setPixelRatio(window.devicePixelRatio);
        this.renderer.setSize(window.innerWidth, window.innerHeight);
        this.renderer.outputColorSpace = THREE.SRGBColorSpace;
        this.renderer.xr.enabled = true;
        this.renderer.shadowMap.enabled = true;
        this.renderer.shadowMap.type = THREE.PCFSoftShadowMap;
        
        this.container.appendChild(this.renderer.domElement);
        
        // Lighting
        this.setupLighting();
        
        // Environment
        this.setupEnvironment();
        
        // Controllers
        this.setupControllers();
        
        // XR Session
        this.setupXRSession();
        
        // Resize handler
        window.addEventListener('resize', () => this.onResize());
        
        // Render loop
        this.renderer.setAnimationLoop((time) => this.render(time));
    }
    
    setupLighting() {
        // Ambient light
        const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
        this.scene.add(ambientLight);
        
        // Directional light (sun)
        const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
        directionalLight.position.set(5, 10, 7);
        directionalLight.castShadow = true;
        directionalLight.shadow.mapSize.width = 2048;
        directionalLight.shadow.mapSize.height = 2048;
        directionalLight.shadow.camera.near = 0.5;
        directionalLight.shadow.camera.far = 50;
        this.scene.add(directionalLight);
        
        // Hemisphere light for natural feel
        const hemisphereLight = new THREE.HemisphereLight(0xffffff, 0x444444, 0.6);
        this.scene.add(hemisphereLight);
    }
    
    setupEnvironment() {
        // Ground plane (for shadow receiving)
        const groundGeometry = new THREE.PlaneGeometry(50, 50);
        const groundMaterial = new THREE.ShadowMaterial({ opacity: 0.3 });
        const ground = new THREE.Mesh(groundGeometry, groundMaterial);
        ground.rotation.x = -Math.PI / 2;
        ground.receiveShadow = true;
        ground.name = 'ground';
        this.scene.add(ground);
        
        // Grid helper for spatial reference
        const gridHelper = new THREE.GridHelper(20, 20, 0x444444, 0x888888);
        gridHelper.position.y = 0.001; // Slightly above ground to prevent z-fighting
        this.scene.add(gridHelper);
    }
    
    setupControllers() {
        // Controller model factory
        const controllerModelFactory = new XRControllerModelFactory();
        
        // Setup both controllers
        for (let i = 0; i < 2; i++) {
            const controller = this.renderer.xr.getController(i);
            controller.addEventListener('selectstart', (event) => this.onSelectStart(event));
            controller.addEventListener('selectend', (event) => this.onSelectEnd(event));
            this.scene.add(controller);
            
            // Grip (controller body visualization)
            const grip = this.renderer.xr.getControllerGrip(i);
            grip.add(controllerModelFactory.createControllerModel(grip));
            this.scene.add(grip);
            
            // Ray (for pointing/interaction)
            const rayGeometry = new THREE.BufferGeometry().setFromPoints([
                new THREE.Vector3(0, 0, 0),
                new THREE.Vector3(0, 0, -1)
            ]);
            const rayMaterial = new THREE.LineBasicMaterial({ color: 0xff0000 });
            const rayLine = new THREE.Line(rayGeometry, rayMaterial);
            rayLine.scale.z = 5; // Ray length
            controller.add(rayLine);
            
            // Cursor (at end of ray)
            const cursorGeometry = new THREE.RingGeometry(0.02, 0.04, 32);
            const cursorMaterial = new THREE.MeshBasicMaterial({
                color: 0xff0000,
                transparent: true,
                opacity: 0.8
            });
            const cursor = new THREE.Mesh(cursorGeometry, cursorMaterial);
            cursor.position.z = -5;
            cursor.userData.isCursor = true;
            controller.add(cursor);
            
            this.controllers.push({
                controller,
                grip,
                ray: rayLine,
                cursor
            });
        }
    }
    
    setupXRSession() {
        if (this.options.mode === 'vr') {
            const vrButton = VRButton.createButton(this.renderer);
            vrButton.style.position = 'bottom-right';
            this.container.appendChild(vrButton);
        } else if (this.options.mode === 'ar') {
            const arButton = ARButton.createButton(this.renderer, {
                requiredFeatures: ['hit-test'],
                optionalFeatures: ['dom-overlay'],
                domOverlay: { root: document.body }
            });
            arButton.style.position = 'bottom-right';
            this.container.appendChild(arButton);
        }
    }
    
    onSelectStart(event) {
        const controller = event.target;
        const tempMatrix = new THREE.Matrix4();
        tempMatrix.identity().extractRotation(controller.matrixWorld);
        
        this.raycaster.ray.origin.setFromMatrixPosition(controller.matrixWorld);
        this.raycaster.ray.direction.set(0, 0, -1).applyMatrix4(tempMatrix);
        
        const intersects = this.raycaster.intersectObjects(this.interactiveObjects, true);
        
        if (intersects.length > 0) {
            const object = intersects[0].object;
            
            // Attach object to controller
            object.attach(controller);
            controller.userData.selected = object;
            
            // Visual feedback
            object.material.emissive?.setHex(0x555555);
        }
    }
    
    onSelectEnd(event) {
        const controller = event.target;
        
        if (controller.userData.selected !== undefined) {
            const object = controller.userData.selected;
            object.attach(this.scene); // Detach from controller
            
            // Remove emissive highlight
            object.material.emissive?.setHex(0x000000);
            
            controller.userData.selected = undefined;
        }
    }
    
    addInteractiveObject(object) {
        this.interactiveObjects.push(object);
        this.scene.add(object);
        return object;
    }
    
    loadModel(url, position = new THREE.Vector3(0, 1, 0), scale = 1) {
        const loader = new GLTFLoader();
        
        return new Promise((resolve, reject) => {
            loader.load(url, (gltf) => {
                const model = gltf.scene;
                
                model.position.copy(position);
                model.scale.setScalar(scale);
                model.traverse((child) => {
                    if (child.isMesh) {
                        child.castShadow = true;
                        child.receiveShadow = true;
                    }
                });
                
                this.addInteractiveObject(model);
                resolve(model);
            }, undefined, reject);
        });
    }
    
    render(time) {
        // Update controller rays and cursors
        this.controllers.forEach(({ controller, cursor }) => {
            if (cursor) {
                // Make cursor face camera
                cursor.lookAt(this.camera.position);
            }
        });
        
        this.renderer.render(this.scene, this.camera);
    }
    
    onResize() {
        this.camera.aspect = window.innerWidth / window.innerHeight;
        this.camera.updateProjectionMatrix();
        this.renderer.setSize(window.innerWidth, window.innerHeight);
    }
}

// Export for use
export default HermesWebXRScene;
```

---

## ⚡ XR 性能优化策略

### 关键优化清单

```
┌─────────────────────────────────────────────────────────────┐
│              XR Performance Optimization                   │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Rendering Optimization                                     │
│  ├── Single Pass Instanced Rendering                        │
│  ├── Foveated Rendering (Fixed/ Eye-Tracked)               │
│  ├── Asynchronous TimeWarp                                  │
│  ├── Dynamic Resolution (Adaptive)                         │
│  └── Occlusion Culling                                     │
│                                                             │
│  Asset Optimization                                         │
│  ├── Mesh Compression (Draco)                              │
│  ├── Texture Compression (ASTC/ETC2/BC7)                  │
│  ├── LOD System (Level of Detail)                          │
│  ├── Object Pooling                                        │
│  └── GPU Instancing                                        │
│                                                             │
│  CPU Optimization                                           │
│  ├── Job System / Burst Compiler                           │
│  ├── Physics Optimization (Layered)                       │
│  ├── Spatial Partitioning (Octree/BVH)                    │
│  └── Multithreading                                       │
│                                                             │
│  Memory Management                                          │
│  ├── Texture Streaming                                     │
│  ├── Asset Bundle Loading                                  │
│  ├── Garbage Collection Minimization                       │
│  └── Texture Atlasing                                      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🔗 相关技能

- [Hermes-Skill-Game-Developer](../game-development/game-developer/SKILL.md) - 游戏引擎深度使用
- [Hermes-Skill-IoT-Architect](../iot/iot-architect/SKILL.md) - IoT 与 AR 结合
- [Hermes-Skill-Performance-Optimizer](../performance/performance-optimizer/SKILL.md) - XR 性能调优

---

## 📊 技能统计

| 指标 | 数值 |
|------|------|
| 支持平台 | Meta Quest, Apple Vision Pro, HoloLens, PSVR2, WebXR |
| 引擎集成 | Unity 6, Unreal 5.4, Three.js r160+ |
| 代码示例 | C#, JavaScript/TypeScript, Blueprint |
| 交互模式 | 控制器、手势追踪、眼动追踪、语音 |
| 优化技术 | Foveated Rendering, ASW, Fixed Foveated Rendering |
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
**适用场景**: VR 游戏 | AR 应用 | 工业仿真 | 培训模拟 | 元宇宙社交
