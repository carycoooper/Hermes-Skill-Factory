---
name: mobile-app-guide
description: "Hermes-Skill-Mobile-App-Guide - 移动应用开发全指南，覆盖iOS/SwiftUI/Android/Jetpack Compose跨平台开发、UI设计规范、性能优化和应用商店发布。"
version: 2.0.0
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [mobile-development, ios, android, swiftui, jetpack-compose, flutter, react-native, app-store, performance, ux-mobile]
    related_skills: [frontend-design-assistant, api-testing-suite, cicd-pipeline-manager, security-auditor]
    requires_toolsets: [web, terminal]
    config:
      - key: primary_platform
        default: cross_platform
      - key: ui_framework
        default: declarative
---

# Hermes-Skill-Mobile-App-Guide (移动应用开发全指南)

## 概述

**Hermes-Skill-Mobile-App-Guide** 是一个移动端开发的完整知识体系。从平台选型到架构设计，从 UI 实现到性能优化，从测试策略到上架发布，覆盖移动开发生命周期的每个关键决策点。

### 核心能力

- **平台选型**: Native vs Cross-platform 决策框架
- **iOS 开发**: Swift / SwiftUI / UIKit / Combine / Async/Await
- **Android 开发**: Kotlin / Jetpack Compose / Material Design 3 / Coroutines
- **跨平台方案**: Flutter / React Native / KMM 性能对比
- **架构模式**: MVVM / MVI / Clean Architecture / Unidirectional Data Flow
- **性能优化**: 启动速度 / 内存 / 电量 / 包体积 / 动画帧率

---

## 平台选型决策矩阵

| 维度 | Native iOS | Native Android | Flutter | React Native | KMM |
|------|-----------|---------------|---------|--------------|-----|
| **性能** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **开发效率** | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| **代码共享** | 0% (per platform) | 0% | ~90% | ~80% | ~70% (biz logic) |
| **UI 保真度** | 100% | 100% | 95% | 85% | 100% (per platform) |
| **团队技能要求** | Swift | Kotlin | Dart | JS/TS + Platform | Kotlin |
| **包体积增量** | 0 | 0 | +~5MB | +~10MB | +~1MB |
| **生态成熟度** | 成熟 | 成熟 | 快速成长 | 成熟 | 早期 |
| **适用场景** | 追求极致体验 | Android为主 | 统一UI强 | Web团队转型 | 共享业务逻辑 |

### 选型推荐

```
选择 Native (Swift/Kotlin) 如果:
✅ 追求平台原生体验和最新 API
✅ 团队已有对应平台专家
✅ 应用需要深度集成平台特性 (AR/Widgets/HCE)
❌ 不适合: 小团队 / 快速验证想法 / 预算有限

选择 Flutter 如果:
✅ 一套代码覆盖 iOS + Android + Web/Desktop
✅ UI 高度统一且复杂
✅ 团队偏好 Dart 或愿意学习
✅ 60fps 动画要求高
❌ 不适合: 极致包体积要求 (<10MB) / 需要大量原生模块

选择 React Native 如果:
✅ 团队主要是 Web 开发者
✅ 已有大量 JS/TS 代码可复用
✅ 需要快速迭代和热重载
❌ 不适合: 复杂动画 / 高性能计算 / 特殊硬件交互

选择 KMM (Kotlin Multiplatform Mobile) 如果:
✅ 业务逻辑需要高度共享
✅ 愿意为每个平台写原生 UI
✅ 团队已精通 Kotlin
❌ 不适合: UI 也想共享 / 团队没有 Kotlin 经验
```

---

## iOS 开发核心 (SwiftUI)

### 项目结构 (Clean Architecture)

```
HermesSkillsApp/
├── App/
│   ├── HermesSkillsApp.swift        @main
│   └── AppDelegate.swift
│
├── Core/                             # Business Logic
│   ├── Domain/
│   │   ├── Models/                   # Pure Swift structs
│   │   │   ├── Skill.swift
│   │   │   ├── Category.swift
│   │   │   └── User.swift
│   │   ├── Repositories/
│   │   │   ├── Protocol/
│   │   │   │   └── SkillRepositoryProtocol.swift
│   │   │   └── Implementation/
│   │   │       └── RemoteSkillRepository.swift
│   │   └── UseCases/
│   │       ├── GetSkillListUseCase.swift
│   │       └── SearchSkillsUseCase.swift
│   │
│   └── Data/
│       ├── Network/
│       │   ├── APIClient.swift
│       │   ├── Endpoints/
│       │   └── Interceptors/
│       ├── Local/
│       │   ├── CoreData Stack (or Realm)
│       │   └── UserDefaults Wrapper
│       └── DTOs/                            # API Response Models
│
├── Features/                          # Feature Modules
│   ├── SkillList/
│   │   ├── Views/
│   │   │   ├── SkillListView.swift
│   │   │   ├── SkillRowView.swift
│   │   │   └── SkillDetailView.swift
│   │   ├── ViewModels/
│   │   │   └── SkillListViewModel.swift
│   │   └── Services/
│   │       └── SkillService.swift
│   │
│   ├── Search/
│   ├── Settings/
│   └── Auth/
│
├── Shared/                            # Common Components
│   ├── Components/
│   │   ├── PrimaryButton.swift
│   │   ├── LoadingView.swift
│   │   ├── ErrorView.swift
│   │   └── EmptyStateView.swift
│   ├── Extensions/
│   │   ├── Color+Hex.swift
│   │   ├── View+RoundedCorner.swift
│   │   └── String+Validation.swift
│   └── Theme/
│       ├── Colors.swift
│       ├── Typography.swift
│       └── Spacing.swift
│
├── Resources/
│   ├── Assets.xcassets/
│   ├── Localizable.strings
│   └── Info.plist
│
└── HermesSkillsAppTests/
    ├── UnitTests/
    └── UITests/
```

### SwiftUI 最佳实践

```swift
// MARK: - State Management (ObservableObject + Published)
@MainActor
class SkillListViewModel: ObservableObject {
    @Published private(set) var skills: [Skill] = []
    @Published private(set) var isLoading = false
    @Published var error: String?
    @Published var searchText = ""
    
    private let getSkillsUseCase: GetSkillsUseCase
    private var searchTask: Task<Void, Never?
    
    init(getSkillsUseCase: GetSkillsUseCase) {
        self.getSkillsUseCase = getSkillsUseCase
    }
    
    func loadSkills() async {
        isLoading = true
        error = nil
        do {
            skills = try await getSkillsUseCase.execute()
        } catch {
            self.error = error.localizedDescription
        }
        isLoading = false
    }
    
    func search(_ query: String) {
        searchText = query
        searchTask?.cancel()
        
        guard !query.isEmpty else {
            loadSkills()
            return
        }
        
        searchTask = Task {
            let filtered = skills.filter { skill in
                skill.name.localizedCaseInsensitiveContains(query) ||
                skill.category.rawValue.localizedCaseInsensitiveContains(query)
            }
            await MainActor.run {
                self.skills = filtered
            }
        }
    }
}

// MARK: - View (View Protocol)
struct SkillListView: View {
    @StateObject private var viewModel: SkillListViewModel
    @State private var selectedCategory: SkillCategory?
    
    var body: some View {
        NavigationStack {
            Group {
                if viewModel.isLoading {
                    ProgressView("Loading skills...")
                } else if let error = viewModel.error {
                    ErrorView(message: error, retryAction: { await viewModel.loadSkills() })
                } else if viewModel.skills.isEmpty {
                    EmptyStateView(icon: "doc.text.magnifyingglass", title: "No Skills Found")
                } else {
                    skillContent
                }
            }
            .navigationTitle("Hermes Skills")
            .toolbar {
                ToolbarItem(placement: .topBarTrailing) {
                    Menu {
                        Button("Sort by Name") { viewModel.sortBy(.name) }
                        Button("Sort by Popularity") { viewModel.sortBy(.popularity) }
                        Button("Filter by Category") { showCategoryFilter = true }
                    } label: {
                        Image(systemName: "arrow.up.arrow.down")
                    }
                }
            }
            .searchable(text: $viewModel.searchText)
            .task { await viewModel.loadSkills() }
            .refreshable { await viewModel.loadSkills() }
        }
    }
    
    private var skillContent: some View {
        List(viewModel.skills) { skill in
            NavigationLink(value: skill) {
                SkillRowView(skill: skill)
            }
        }
        .listStyle(.inset(grouped: true))
        .animation(.default, value: viewModel.skills)
    }
}

// MARK: - Preview Provider
#Preview {
    SkillListView(
        viewModel: SkillListViewModel(
            getSkillsUseCase: MockGetSkillsUseCase()
        )
    )
}
```

---

## Android 开发核心 (Jetpack Compose)

### Modern Android Stack (2026)

```kotlin
// build.gradle.kts (Module :app)
dependencies {
    // Jetpack Compose BOM
    val composeBom = platform("androidx.compose:compose-bom:2026.04.01")
    implementation(composeBom)
    implementation("androidx.compose.ui:ui")
    implementation("androidx.compose.material3:material3")
    implementation("androidx.compose.activity:activity-compose")
    
    // Architecture
    implementation("androidx.lifecycle:lifecycle-viewmodel-compose")
    implementation("androidx.navigation:navigation-compose")
    implementation("io.coil-kt:coil-compose:3.0.0")  // Image loading
    
    // Networking
    implementation("com.squareup.retrofit2:retrofit:2.11.0")
    implementation("com.squareup.okhttp3:okhttp:5.0.0-alpha.14")
    implementation("com.jakewharton.retrofit:retrofit2-kotlinx-serialization-converter:1.0.0")
    
    // DI (Hilt)
    implementation("com.google.dagger:hilt-android:2.51.1")
    kapt("com.google.dagger:hilt-compiler:2.51.1")
    
    // Room Database
    implementation("androidx.room:room-runtime:2.6.1")
    kapt("androidx.room:room-compiler:2.6.1")
    
    // Testing
    testImplementation("junit:junit:4.13.2")
    androidTestImplementation("androidx.compose.ui:ui-test-junit4")
}
```

### Compose Screen Example

```kotlin
@Composable
fun SkillListScreen(
    viewModel: SkillListViewModel = hiltViewModel(),
    onNavigateToDetail: (Skill) -> Unit = {}
) {
    val uiState by viewModel.uiState.collectAsStateWithLifecycle()
    
    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("Hermes Skills") },
                actions = {
                    IconButton(onClick = { }) {
                        Icon(imageVector = Icons.Default.FilterList)
                    }
                }
            )
        },
        floatingActionButton = {
            FloatingActionButton(onClick = { }) {
                Icon(imageVector = Icons.Default.Search)
            }
        }
    ) { paddingValues ->
        when {
            uiState.isLoading -> {
                Box(
                    modifier = Modifier.fillMaxSize(),
                    contentAlignment = Alignment.Center
                ) {
                    CircularProgressIndicator()
                }
            }
            
            uiState.error != null -> {
                ErrorScreen(
                    message = uiState.error!!,
                    onRetry = { viewModel.refresh() }
                )
            }
            
            uiState.skills.isEmpty() -> {
                EmptyStateScreen()
            }
            
            else -> {
                LazyColumn(
                    contentPadding = paddingValues,
                    verticalArrangement = Arrangement.spacedBy(8.dp)
                ) {
                    items(
                        items = uiState.skills,
                        key = { it.id }
                    ) { skill ->
                        SkillCard(
                            skill = skill,
                            onClick = { onNavigateToDetail(skill) }
                        )
                    }
                }
            }
        }
    }
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun SkillCard(
    skill: Skill,
    onClick: () -> Unit,
    modifier: Modifier = Modifier
) {
    ElevatedCard(
        onClick = onClick,
        modifier = modifier
            .fillMaxWidth()
            .animateContentSize()
    ) {
        Column(modifier = Modifier.padding(16.dp)) {
            Row(verticalAlignment = Alignment.CenterVertically) {
                // Category badge
                SuggestionChip(
                    onClick = {},
                    label = { Text(skill.category.name) },
                    colors = SuggestionChipDefaults.suggestionChipColors(
                        containerColor = skill.category.color.copy(alpha = 0.2f),
                        labelColor = skill.category.color
                    )
                )
                
                Spacer(modifier = Modifier.weight(1f))
                
                // Popularity indicator
                Icon(
                    imageVector = if (skill.isPopular) Icons.Default.Star else Icons.Default.StarOutline,
                    contentDescription = null,
                    tint = if (skill.isPopular) Color(0xFFFFD700) else Color.Gray
                )
                
                Text(
                    text = formatPopularity(skill.popularity),
                    style = MaterialTheme.typography.labelSmall,
                    color = MaterialTheme.colorScheme.onSurfaceVariant
                )
            }
            
            Spacer(modifier = Modifier.height(8.dp))
            
            Text(
                text = skill.name,
                style = MaterialTheme.typography.titleMedium,
                maxLines = 2,
                overflow = TextOverflow.Ellipsis
            )
            
            Spacer(modifier = Modifier.height(4.dp))
            
            Text(
                text = skill.shortDescription,
                style = MaterialTheme.typography.bodySmall,
                color = MaterialTheme.colorScheme.onSurfaceVariant,
                maxLines = 2,
                overflow = TextOverflow.Ellipsis
            )
        }
    }
}
```

---

## 性能优化清单

### 启动优化 (Cold Start < 1.5s)

```
iOS:
├── 减少 AppDelegate 中的同步操作
├── 使用 App Launch Storyboard (而非代码创建 window)
├── 延迟非首屏数据的加载 (lazy initialization)
├── 预缓存关键数据 (CoreData pre-populate)
└── 使用 MetricKit 监控真实用户启动时间

Android:
├── 启用 Baseline Profiles
├── 减少 Application.onCreate() 工作
├── 使用 App Startup library
├── 异步初始化非必要组件
└── 检查 StrictMode 在 Release 模式关闭
```

### 内存优化

```
通用:
├── 使用 Instruments (XAS) / Android Profiler 检测泄漏
├── 图片使用适当分辨率 (@1x/@2x/@3x)
├── 列表/集合使用懒加载 (LazyVStack / LazyColumn)
├── 及时释放大对象 (设为 nil / WeakReference)
├── 缓存大小限制 (LRU Cache with max size)
└── 避免循环引用 (weak self in closures)

iOS Specific:
├── Instruments Allocations 检测 Retain Cycle
├── 避免在 Cell 中捕获强引用
├── 使用 Value Type (struct) 替代 Class (where possible)
└── 注意 UIImage 缓存策略

Android Specific:
├── LeakCanary 检测内存泄漏
├── 避免 Activity 泄漏 (静态引用 Context)
├── 正确使用 ViewModel 生命周期
├── 大图使用 Glide/Coil 的采样加载
└── 检查 Bitmap 未回收问题
```

---

*基于 Apple HIG / Material Design 3 / Google Android Developers Best Practices*
*版本: 2.0.0 | 最后更新: 2026-04-27*
