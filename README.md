# Programming Model mapping between Kotlin Compose and Web UI

 **선언형 UI(Declarative UI)의 본질적인 멘탈 모델을 교육하기 위한 Playbook 접근 방식**

학생들이 처음부터 무거운 Android Studio와 에뮬레이터 환경에서 Compose를 배우다 보면, 언어(Kotlin)와 프레임워크의 복잡성에 짓눌려 '상태(State)에 따른 UI의 변화'라는 핵심 원리를 놓치기 쉽습니다. 브라우저에서 가벼운 Web Component(예: Lit) 기반의 Playbook으로 Recomposition과 상태 관리의 원리를 직관적으로 학습한 뒤, 실제 네이티브 환경(Android Studio)으로 넘어가는 단계적 접근은 인지적 과부하(Cognitive Load)를 크게 줄여줄 것입니다. 

특히, 코딩 에이전트를 활용해 **'Compose -> Semiformal DSL -> Self-contained HTML/JS'** 파이프라인을 구축하는 것은 교육 자료의 확장성과 유지보수 측면에서 매우 스마트한 설계입니다. '중간 표현체(IR)'를 정의하여, 

Compose의 고유한 생명주기 및 상태 관리 메커니즘(예: `remember`, `LaunchedEffect`)을 JavaScript의 반응형 속성(Reactive Properties)과 DOM 업데이트 주기 등에 이질감 없이 매핑합니다.

---

# 📘 Compose2Web Playbook Agent
**Learn Declarative UI Principles in the Browser, Build in Android Studio.**



## 🎯 Project Overview
**Compose2Web Playbook Agent**는 Android Jetpack Compose의 UI 코드와 실행 원리를 웹 브라우저 상에서 인터랙티브하게 학습할 수 있도록 돕는 교육용 파이프라인 및 Playbook 생성기입니다. 

이 프로젝트는 복잡한 Compose 코드를 플랫폼 독립적인 Semiformal DSL로 변환하고, 이를 다시 코딩 에이전트를 통해 독립적인 실행이 가능한(Self-contained) HTML/JS 코드(Web Components/Lit 기반)로 컴파일합니다. 학습자는 무거운 IDE 환경을 구축하기 전, 웹 브라우저에서 Playbook을 브라우징하며 선언형 UI의 상태 변화와 렌더링 원리를 직관적으로 체득할 수 있습니다.

## ✨ Key Features
* **Step-by-Step UI Learning:** 브라우저 기반의 가벼운 실습을 통해 선언형 UI의 멘탈 모델을 먼저 구축하고, 이후 Android Studio에서 네이티브 실습으로 이어지는 단계별 커리큘럼을 지원합니다.
* **Semiformal DSL Abstraction:** Compose의 구조(트리 형태의 UI, 상태 변수)를 Web Component의 생명주기와 1:1로 매핑할 수 있는 중간 언어(DSL)를 정의하여 복잡성을 낮춥니다.
* **Agent-Driven Generation:** 코딩 에이전트가 DSL을 해석하여 스타일(CSS), 로직(JS), 마크업(HTML)이 모두 포함된 단일 HTML 형태의 Playbook Case를 자동 생성합니다.
* **Live Recomposition Visualization:** Lit의 반응형(Reactive) 시스템을 활용하여, 상태(State)가 변경될 때 UI 트리가 어떻게 다시 그려지는지(Recomposition) 시각적으로 묘사합니다.

## 🏗️ Architecture Pipeline

1.  **Input (Kotlin):** 학습용으로 작성된 원본 Jetpack Compose UI Snippet.
2.  **Parser/Analyzer:** Compose 코드를 분석하여 UI 계층 구조와 State Dependency를 추출.
3.  **Semiformal DSL:** 추출된 데이터를 플랫폼에 종속되지 않는 형태의 DSL로 변환.
    * *Example:* `Component(name, states[], children[])`
4.  **Playbook Agent:** LLM 기반 코딩 에이전트가 DSL을 입력받아, Lit 기반의 Web Component로 동작하는 Self-contained HTML 파일을 생성.
5.  **Output (Playbook):** 브라우저에서 바로 실행하고 상호작용할 수 있는 웹 기반의 실습 교안.

## 📚 Educational Workflow
1.  **Browser Learning (Playbook):** 학생들은 제공된 HTML Playbook을 엽니다. 버튼 클릭, 텍스트 입력 등에 따라 Web Component의 상태가 변하고 화면이 업데이트되는 과정을 브라우저 개발자 도구(DOM 트리 변화)와 함께 학습합니다.
2.  **Concept Mapping:** Playbook 내에 병기된 원본 Compose 코드와 Web UI 코드를 비교하며 `remember` ↔ `State`, `Modifier` ↔ `CSS Flex/Grid`의 매핑 관계를 이해합니다.
3.  **Native Practice (Android Studio):** 웹에서 익힌 선언형 멘탈 모델을 바탕으로 Android Studio에서 실제 Compose 앱을 작성하고 구조화합니다.

## 🚀 Getting Started

...

---

Compose UI를 웹 환경으로 매핑하여 교육용 Playbook을 구성할 때, '선언형 UI(Declarative UI)와 상태(State)에 따른 화면 갱신'이라는 Compose의 핵심 멘탈 모델을 가르치기에는 Lit가 적합합니다.

### 📊 패러다임 비교

| 특성 | Jetpack Compose | Lit (Web Components) | Vanilla JS |
| :--- | :--- | :--- | :--- |
| **UI 패러다임** | 선언형 (Declarative) | 선언형 (Declarative) | 명령형 (Imperative) |
| **상태 관리** | `remember`, `MutableState` | `@state`, Reactive Properties | 변수 선언 및 수동 DOM 업데이트 |
| **화면 갱신** | Recomposition (자동) | Reactive Update (자동/비동기) | 수동 (`innerHTML`, `appendChild` 등) |
| **컴포넌트화** | `@Composable` 함수 | `HTMLElement` 상속 클래스 | 함수형 또는 클래스 패턴 직접 구현 |
| **레이아웃** | `Modifier` | CSS (Flexbox, Grid 등) | CSS (Flexbox, Grid 등) |

---

### 💡 Lit를 추천하는 이유 (The "Why")

**1. Compose의 멘탈 모델과 완벽한 일치**
Lit의 핵심은 상태(State)가 변하면 UI가 자동으로 다시 그려진다는 점입니다. Compose의 `State`가 변경될 때 발생하는 Recomposition 메커니즘을 Lit의 `@state`와 `render()` 함수 호출로 거의 1:1에 가깝게 설명할 수 있습니다. 

**2. 코드의 가독성과 Self-contained 구조**
에이전트가 생성해야 할 Playbook은 하나의 HTML 파일 안에서 직관적으로 읽혀야 합니다. Lit는 템플릿 리터럴(`html` 태그)을 사용하여 HTML, CSS, JS 로직을 하나의 클래스 안에 깔끔하게 캡슐화합니다. 반면 Vanilla JS로 상태 변화에 따른 UI 업데이트 로직을 짜면 코드가 방대해지고, '무엇(What)을 그릴지'보다 '어떻게(How) DOM을 조작할지'에 시선이 뺏기게 됩니다.



**3. 웹 기초 지식과의 자연스러운 연결**
대학 수업 등에서 DOM 구조나 CSS의 Flexbox, Grid 레이아웃을 다룰 때, Lit는 브라우저 표준인 Web Components 기반이므로 기존 지식을 그대로 활용할 수 있습니다. 수동으로 DOM API를 호출하던 방식이 선언형 프레임워크를 만나 어떻게 우아하게 추상화되는지 보여주는 훌륭한 징검다리 역할을 합니다.

### 🛠️ Vanilla JS가 의미 있는 예외적인 상황

만약 이 Playbook의 목적이 **"Compose 컴파일러가 내부적으로 어떻게 명령형 UI(View 시스템)로 코드를 번역하는가?"**를 보여주는 것이라면 Vanilla JS가 의미를 가질 수 있습니다. 
즉, Compose 코드가 최종적으로 어떻게 노드를 찾고 속성을 변경하는지에 대한 '로우레벨 엔진'의 원리를 설명하고 싶다면, 에이전트가 극도로 최적화된 바닐라 JS 코드를 뱉어내게 하여 두 패러다임의 차이를 극명하게 보여줄 수는 있습니다.

---
DSL의 기준점을 어디에 둘 것인가는 이 파이프라인 아키텍처에서 가장 핵심적인 질문입니다. 

결론부터 말씀드리면, Compose나 Lit 어느 한쪽의 문법에 종속되기보다는 **선언형 UI의 보편적 추상 구문 트리(AST, Abstract Syntax Tree) 역할을 하는 '중간 표현체(IR, Intermediate Representation)' 기준**으로 설계하는 것이 가장 좋습니다. 

에이전트가 코드를 변환할 때, 플랫폼에 종속된 문법적 노이즈를 걷어내고 **"어떤 상태(State)가 있고, 어떤 트리 계층(Tree)을 가지며, 레이아웃(Layout)은 어떠한가"**라는 본질적인 의도(Intent)만 DSL에 담아야 양방향 매핑과 확장이 쉬워집니다.

특히 학생들에게 Compose의 `Modifier`가 웹의 `Flexbox`나 `Grid`로 어떻게 치환되는지, `remember`가 컴포넌트의 로컬 상태로 어떻게 매핑되는지 가르치려면, DSL은 이 둘 사이의 '개념적 교집합'을 표현해야 합니다.



---

### 📝 Semiformal DSL 설계 원칙 및 샘플 코드

LLM 기반의 코딩 에이전트가 파싱하기 쉽고, 사람(학생)이 읽어도 직관적으로 이해할 수 있도록 구조화된 YAML 형태의 DSL을 제안합니다. 앞서 보여드린 '카운터 UI'를 표현한 샘플입니다.

```yaml
# Semiformal DSL: Declarative UI Abstract Representation
Component:
  name: ComposeCounter
  
  # 1. 상태 (State): Compose의 remember / Lit의 @state
  state:
    - name: count
      type: Int
      initialValue: 0

  # 2. UI 트리 및 레이아웃 (Tree & Layout)
  ui:
    type: Column # Compose의 Column (Web의 flex-direction: column)
    layout:
      alignItems: center
      gap: 16px
      padding: 24px
      border: "1px solid #ccc"
      borderRadius: "8px"
    
    # 3. 하위 노드 (Children)
    children:
      - type: Text
        content: "현재 카운트: ${count}" # 상태 바인딩
        style:
          fontSize: 24px
          fontWeight: bold
          
      - type: Button
        content: "클릭하여 증가"
        style:
          padding: "8px 16px"
          fontSize: "16px"
        # 4. 이벤트 및 상태 변경 로직 (Actions)
        onClick:
          action: "increment"
          targetState: "count"
          expression: "count + 1"
```

### 💡 이 DSL 구조의 장점

1. **플랫폼 독립성 (Platform Agnostic):** 이 DSL 안에는 Kotlin의 `val`, `Modifier` 등의 키워드나, JavaScript의 `this`, `html` 같은 플랫폼 종속적인 문법이 없습니다. 오직 UI의 구조와 상태만 정의됩니다.
2. **명확한 레이아웃 매핑:** `type: Column`과 하위의 `alignItems: center` 같은 속성은 에이전트가 Lit의 CSS를 생성할 때 자연스럽게 `display: flex; flex-direction: column; align-items: center;`로 번역할 수 있는 완벽한 힌트가 됩니다. 브라우저의 DOM/CSS 렌더링 원리를 수업할 때 설명하기 매우 좋습니다.
3. **에이전트 친화적 (Agent-Friendly):** JSON이나 YAML 형태의 스펙(Spec)은 LLM이 가장 정확하게 이해하고 생성할 수 있는 포맷입니다. 에이전트에게 "이 DSL을 기반으로 Lit 웹 컴포넌트를 작성해"라고 지시할 때 환각(Hallucination)을 최소화할 수 있습니다.

이러한 형태의 중간 DSL을 거치게 되면, 복잡한 Compose 코드를 먼저 이 DSL로 정제(Step 1)하고, 정제된 DSL을 바탕으로 HTML/Lit 코드를 생성(Step 2)하는 견고한 에이전트 파이프라인을 구축할 수 있습니다.

---

<h2>Jetpack Compose UI Chapters</h2>
<ol>
  <li>First App</li>
  <li>Building User Screen Flows</li>
  <li>Developing the UI with Jetpack Compose</li>
  <li>Building App Navigation</li>
  <li>Essential Libraries – Ktor, Kotlin Serialization, and Coil</li>
  <li>Building Lists with Jetpack Compose</li>
  <li>Android Permissions and Google Maps</li>
  <li>Services, WorkManager, and Notifications</li>
  <li>Testing with JUnit, Mockito, MockK, and Compose</li>
  <li>Coroutines and Flow</li>
  <li>Android Architecture Components</li>
  <li>Persisting Data</li>
  <li>Dependency Injection with Dagger, Hilt, and Koin</li>
  <li>Architecture Patterns</li>
  <li>Advanced Jetpack Compose</li>
  <li>Launching Your App on Google Play</li>
</ol>

