2026년 현재, Android 진영은 Kotlin 2.x 시대가 완전히 안착하면서 K2 컴파일러의 성능 향상과 언어적 통합을 기본으로 누리는 환경이 되었습니다. 특히 Compose 컴파일러가 Kotlin 저장소로 공식 병합(Merged)된 이후, 버전 관리가 훨씬 직관적으로 변했습니다.

학생들이 웹 브라우저 기반의 Playbook(Lit)에서 선언형 멘탈 모델을 학습한 후, 실제 Android Studio에서 네이티브 앱을 구축할 때 기준이 될 2026년 표준 환경 설정 가이드를 정리해 드립니다.

---

### 📦 2026년 Android + Compose 표준 버전 매트릭스

현재 시점(2026년 1분기 기준)에서 안정성과 최신 기능(Navigation 3.x 포함)을 모두 잡을 수 있는 권장 스펙입니다.

| 구성 요소 | 권장 버전 | 비고 |
| :--- | :--- | :--- |
| **Android API Level** | `compileSdk = 35` (또는 36) | Android 15/16 타겟 |
| **Kotlin** | `2.1.20` | K2 컴파일러 기본 활성화 |
| **AGP (Android Gradle Plugin)** | `8.8.x` | 성능 및 빌드 캐시 최적화 |
| **KSP (Kotlin Symbol Processing)**| `2.1.20-1.0.x` | Kotlin 버전과 정확히 일치해야 함 |
| **Compose BOM** | `2026.02.00` | Compose 라이브러리 버전 일괄 관리 |
| **Compose Navigation** | `3.0.x` | Type-Safe 라우팅 및 Kotlin Serialization 기본 통합 |

---

### 🛠️ 프로젝트 설정: `libs.versions.toml` (Version Catalog)

최근 Android 프로젝트는 Version Catalog를 사용한 의존성 관리가 표준입니다. 학생들에게 배포할 실습용 템플릿의 `gradle/libs.versions.toml` 파일은 다음과 같이 구성하는 것이 좋습니다.

```toml
[versions]
agp = "8.8.0"
kotlin = "2.1.20"
coreKtx = "1.15.0"
lifecycleRuntimeKtx = "2.8.x"
activityCompose = "1.10.x"
composeBom = "2026.02.00"
navigationCompose = "3.0.0"
ksp = "2.1.20-1.0.30"
serialization = "1.8.0"

[libraries]
androidx-core-ktx = { group = "androidx.core", name = "core-ktx", version.ref = "coreKtx" }
androidx-lifecycle-runtime-ktx = { group = "androidx.lifecycle", name = "lifecycle-runtime-ktx", version.ref = "lifecycleRuntimeKtx" }
androidx-activity-compose = { group = "androidx.activity", name = "activity-compose", version.ref = "activityCompose" }
androidx-compose-bom = { group = "androidx.compose", name = "compose-bom", version.ref = "composeBom" }
androidx-ui = { group = "androidx.compose.ui", name = "ui" }
androidx-ui-tooling-preview = { group = "androidx.compose.ui", name = "ui-tooling-preview" }
androidx-material3 = { group = "androidx.compose.material3", name = "material3" }
androidx-navigation-compose = { group = "androidx.navigation", name = "navigation-compose", version.ref = "navigationCompose" }
kotlinx-serialization-json = { group = "org.jetbrains.kotlinx", name = "kotlinx-serialization-json", version.ref = "serialization" }

[plugins]
android-application = { id = "com.android.application", version.ref = "agp" }
kotlin-android = { id = "org.jetbrains.kotlin.android", version.ref = "kotlin" }
kotlin-compose = { id = "org.jetbrains.kotlin.plugin.compose", version.ref = "kotlin" }
kotlin-serialization = { id = "org.jetbrains.kotlin.plugin.serialization", version.ref = "kotlin" }
ksp = { id = "com.google.devtools.ksp", version.ref = "ksp" }
```

### ⚙️ 빌드 설정: `build.gradle.kts` (App Level)

Kotlin 2.0 이후부터는 Compose 컴파일러 확장을 명시적으로 선언해야 합니다.

```kotlin
plugins {
    alias(libs.plugins.android.application)
    alias(libs.plugins.kotlin.android)
    alias(libs.plugins.kotlin.compose) // Kotlin 2.x Compose 플러그인
    alias(libs.plugins.kotlin.serialization) // Nav 3.0 Type-Safe 라우팅용
    alias(libs.plugins.ksp)
}

android {
    namespace = "com.example.compose2web"
    compileSdk = 35

    defaultConfig {
        applicationId = "com.example.compose2web"
        minSdk = 26
        targetSdk = 35
        versionCode = 1
        versionName = "1.0"
    }

    buildTypes {
        release {
            isMinifyEnabled = true
            proguardFiles(getDefaultProguardFile("proguard-android-optimize.txt"), "proguard-rules.pro")
        }
    }
    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_17
        targetCompatibility = JavaVersion.VERSION_17
    }
    kotlinOptions {
        jvmTarget = "17"
    }
    buildFeatures {
        compose = true
    }
}

dependencies {
    implementation(libs.androidx.core.ktx)
    implementation(libs.androidx.lifecycle.runtime.ktx)
    implementation(libs.androidx.activity.compose)
    
    // Compose BOM
    implementation(platform(libs.androidx.compose.bom))
    implementation(libs.androidx.ui)
    implementation(libs.androidx.ui.tooling.preview)
    implementation(libs.androidx.material3)
    
    // Navigation 3.x & Serialization
    implementation(libs.androidx.navigation.compose)
    implementation(libs.kotlinx.serialization.json)
}
```

---

### 🗺️ Compose Navigation 3의 핵심과 웹 매핑 포인트

Compose Navigation 3.x의 가장 큰 특징은 **문자열 기반 라우팅에서 벗어나 Kotlin Serialization 기반의 완전한 Type-Safe 라우팅을 지원**한다는 점입니다. 객체(Object)나 데이터 클래스(Data Class)를 정의하여 화면 간 인자를 안전하게 넘깁니다.

```kotlin
// Android: Navigation 3 Type-Safe Route 정의
@Serializable
object Home

@Serializable
data class Profile(val userId: String, val showDetails: Boolean)
```

**💡 Compose2Web 아키텍처 관점에서의 제언**
이러한 네이티브 환경의 라우팅 구조는, 작성 중이신 Semiformal DSL에서 웹의 URL Path (예: `/profile?userId=123&showDetails=true`)와 매핑되는 훌륭한 브릿지 역할을 할 수 있습니다. Lit Component 내에서 브라우저의 History API나 가벼운 클라이언트 사이드 라우터(Client-side Router)를 사용하도록 에이전트를 학습시키면, 화면 전환 개념까지 웹 Playbook에서 완벽하게 커버할 수 있습니다.
