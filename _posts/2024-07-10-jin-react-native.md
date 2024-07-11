---
title: React Native
author: Jin
categories: [Blogging]
tags: [reactNative]
render_with_liquid: false
typora-root-url: ../
---

**Written by. Jin**

# React Native

**Learn once, Write anywhere.**

### 크로스 플랫폼 앱

크로스 플랫폼 앱은 여러 운영 체제 또는 플랫폼에서 원활하게 작동하도록 설계된 소프트웨어 프로그램입니다. 네이티브 앱 개발과 대조되는 개념으로, 네이티브 앱은 특정 모바일 OS에 맞춰 개발됩니다.

**특징**

1. 코드 재사용성
2. 일관된 사용자 경험
3. 빠른 업데이트 배포
4. 확장성 및 유지보수 용이성

따라서, 크로스 플랫폼 앱은 다양한 운영 체제에서 일관된 사용자 경험을 제공하면서도 개발 및 유지보수 비용을 절감할 수 있는 효과적인 모바일 앱 개발 방식입니다.

## **React Native의 장단점**

**장점**

1. **크로스 플랫폼 개발**: 단일 코드베이스를 사용하여 애플리케이션을 개발하여 개발 시간과 비용을 크게 절약해 줍니다.
2. [**Fast Refresh**](https://reactnative.dev/blog/2019/09/18/version-0.61#fast-refresh): 코드를 변경한 후 애플리케이션을 새로 시작하지 않고도 즉시 결과를 볼 수 있습니다.
3. **강력한 커뮤니티 및 지원**: Facebook의 지원을 받으며, 활발한 개발자 커뮤니티 덕분에 다양한 라이브러리와 도구가 제공됩니다. 특히, `JavaScript`로 작성된 라이브러리를 활용할 수 있는 점이 장점입니다.

**단점**

1. **네이티브 모듈 의존성**: 특정 플랫폼의 기능을 구현하기 위해 네이티브 모듈이 필요한 경우, 네이티브 작성이 필요합니다.
2. **학습 곡선**: `React Native`와 관련된 기술 스택(예: `React`, `JavaScript`, 네이티브 개발 지식)을 모두 학습해야 하므로, 초기 학습 곡선이 가파를 수 있습니다.
3. **디버깅의 어려움**: `JavaScript`와 네이티브 코드 사이의 상호작용으로 인해 디버깅이 복잡해질 수 있습니다.

## **React Native의 작동 원리**

1. **Native 부분과 JavaScript 부분의 분리**:React Native 앱은 네이티브 부분과 JavaScript 부분으로 구성됩니다.네이티브 부분은 UI를 렌더링하는 메인 스레드이고, JavaScript 부분은 비즈니스 로직과 뷰 관리를 담당하는 JS 스레드입니다. 
2. **Native Bridge를 통한 통신**:네이티브 부분과 JavaScript 부분은 Native Bridge를 통해 통신합니다.Native Bridge를 통한 정보 전달이 성능에 중요한 영향을 미치므로, 이를 최소화하는 것이 중요합니다. 
3. **React의 효율적인 Diffing 알고리즘**:React의 효율적인 Diffing 알고리즘을 통해 UI 변경 사항을 최소화하여 전달할 수 있습니다.이를 통해 성능 향상을 도모할 수 있습니다.
4. **JavaScript 실행 및 네이티브 API 접근**:React Native는 자체적으로 호스팅하는 특별한 스레드에서 JavaScript를 실행합니다.이 스레드는 "브릿지"라고 부르는 인터페이스를 통해 네이티브 UI 요소에 접근할 수 있습니다.

### **성능 향상 기법**

- Native 컴포넌트 사용: 네이티브 UI 컴포넌트를 사용하여 네이티브 수준의 성능을 제공합니다.
- Async/Await 활용: 비동기 작업을 효율적으로 처리하여 UI 반응성을 높입니다.
- Hermes 엔진 사용: Facebook이 개발한 고성능 JavaScript 엔진을 사용하여 앱 실행 속도를 높입니다.
- Flatlist 사용: 대량의 데이터를 효율적으로 렌더링할 수 있는 컴포넌트입니다.

## React Native vs Flutter

### **언어 및 아키텍처**

React Native: JavaScript와 React를 사용하여 개발합니다. 네이티브 UI 컴포넌트와 API를 JavaScript 브리지를 통해 연결합니다.

Flutter: Dart 언어를 사용하며, 자체 개발한 Widgets과 Rendering 엔진을 사용합니다. 네이티브 API와 직접 통합됩니다.

### 성능 및 반응성

React Native: JavaScript 기반이므로 JIT(Just-In-Time) 컴파일을 사용하여 개발 속도는 빠르지만, 초기 구동 시 성능이 다소 떨어질 수 있습니다.

Flutter: Dart는 AOT(Ahead-Of-Time) 컴파일을 사용하여 빠른 성능과 반응성을 제공합니다.

### 개발 생산성

React Native: JavaScript와 React 경험이 있는 개발자들이 쉽게 접근할 수 있습니다. 방대한 오픈소스 라이브러리를 활용할 수 있습니다.

Flutter: Dart 언어 학습이 필요하지만, 강력한 IDE 지원과 우수한 개발 도구로 생산성이 높습니다.

### 커뮤니티 및 생태계

React Native: Facebook에서 개발되어 더 큰 커뮤니티와 생태계를 가지고 있습니다.

Flutter: 구글에서 개발되어 상대적으로 작은 커뮤니티이지만, 빠르게 성장하고 있습니다.

요약하면, React Native는 JavaScript와 React 기반으로 개발 생산성이 높고 커뮤니티가 크지만, Flutter는 Dart 언어와 자체 렌더링 엔진을 사용하여 빠른 성능과 반응성을 제공합니다. 프로젝트의 요구사항과 개발자의 기술 스택에 따라 적절한 프레임워크를 선택하는 것이 중요합니다.

|  | React Native | Flutter |
| --- | --- | --- |
| 출시일 | 2015년 | 2017년 |
| 개발사 | Meta | Google |
| 언어 | JS | Dart |
| 학습 비용 | react 경험이 있다면 쉬움 | 새로 배워야 함 |
| 생태계 | 크다 | 비교적 작다 |
| 성능 | 비교적 낮음
(하지만. 스마트폰 성능이 높아짐에 따라 자연스레 해결됨) | 높음 |