# CX Agent Studio (CXAS) 마이그레이션 서비스

본 프로젝트는 기존 **Dialogflow CX (DFCX)** 기반의 대화형 에이전트를 차세대 생성형 AI 플랫폼인 **CX Agent Studio (CXAS)** 로 원활하고 안전하게 전환할 수 있도록 지원하는 자동화 마이그레이션 도구입니다.

---

## 🎯 프로젝트 목적
기존 Dialogflow CX의 Playbook 중심 구조를 CX Agent Studio의 동적인 **에이전트(Agent) 중심 구조**로 자동 분석 및 변환하여, 전환 비용을 최소화하고 마이그레이션의 신뢰성을 확보하는 것을 목표로 합니다.

---

## 🌟 주요 기능 (Key Features)

1. **자동화된 구조 분석 및 재설계**
   - **Flow & Telemetry Analyzer**: 기존 Dialogflow CX의 플레이북 설계를 분석합니다.
   - **Async Agent Designer**: 기존 시스템의 구조를 기반으로 CXAS 환경에 최적화된 에이전트 청사진을 자동으로 설계합니다.

2. **AI 기반 프롬프트 및 로직 변환 (Powered by Gemini)**
   - **Gemini Generate & AI Augment**: 최신 LLM을 활용하여 기존의 정답형 응답 로직을 자연스럽고 컨텍스트를 이해하는 프롬프트 기반 로직으로 변환합니다.
   - **Code Block Migrator**: 기존의 특정 코드 블록이나 웹훅 로직을 새로운 환경에 맞게 이관합니다.

3. **직관적인 인터랙티브 대시보드**
   - 마이그레이션 대상을 손쉽게 선택하고 진행 상황을 실시간으로 시각화하여 모니터링할 수 있는 위젯 기반의 대시보드를 제공합니다.

4. **체계적인 검증 및 테스트 체계**
   - **Test Runner & Eval Generator**: 자동화된 평가(Evals)를 생성하고 실행하여 성공적인 전환 여부를 검증합니다.

---

## 🧩 핵심 구성 요소 (Core Modules)

| 모듈명 | 기능 설명 |
| :--- | :--- |
| **DFCX Client** | 소스 Dialogflow CX 프로젝트 및 에이전트 정보를 추출하는 클라이언트 |
| **Flow / Telemetry Analyzer** | 기존 대화 흐름과 운영 데이터를 분석하여 전환 인사이트 도출 |
| **Async Agent Designer** | 분석 결과를 바탕으로 타겟 CXAS 에이전트의 구조(Playbook) 자동 설계 |
| **Gemini Generate / Augment** | 최신 LLM을 활용해 대화 로직을 고도화하고 프롬프트를 자동 생성 |
| **Agent Visualizer / UI Components** | 복잡한 마이그레이션 단계 및 구조를 대시보드 형태로 시각화 |
| **Test Runner / Reporter** | 전환 완료 후 테스트 케이스 실행 및 최종 결과 리포트 생성 |

---

## 🚀 Colab 기반 단계별 실행 가이드 (How-to)

업로드된 최종본 노트북 파일(`CXAS_Migration_Service_20260511.ipynb`)을 **Google Colab** 환경에서 설치 없이 즉시 띄우고 전환 작업을 수행해 보는 스토리텔링 가이드입니다.

### Step 1. 실행 환경 선택 및 노트북 열기
별도의 로컬 패키지 설치 없이 웹에서 즉시 구동할 수 있도록 다음 두 가지 클라우드 환경을 지원합니다.

#### 방식 A. 일반 Google Colab에서 열기 
1.  [Google Colab](https://colab.research.google.com/)에 접속합니다.
2.  시작 팝업창의 **'GitHub' 탭**을 선택하고, 아래의 원격 레포지토리 주소를 검색합니다.
    👉 `https://github.com/Jhko0404/migration-dfcx-cxas`
3.  검색된 저장소 목록에서 **`CXAS_Migration_Service_20260511.ipynb`** 파일을 클릭하여 엽니다.

#### 방식 B. Colab Enterprise에서 열기
GCP 콘솔에 완전히 통합된 런타임으로, 작업 계정의 IAM 권한을 그대로 상속받아 구동 안정성이 매우 높습니다.
1.  [Google Cloud Console](https://console.cloud.google.com/)에 접속하여 타겟 프로젝트를 선택합니다.
2.  좌측 메뉴에서 **Agent Platform > Notebooks > Colab Enterprise**로 이동합니다.
3.  **'Colab Enterprise'** 탭에서 import notebooks 아이콘을 클릭하여 로컬에서 다운로드 받은 **"CXAS_Migration_Service_20260511.ipynb 노트북 파일"** 을 업로드합니다.
    *(💡 이점: 콘솔 내부 런타임을 사용하므로 이어지는 인증 단계에서 복잡한 ADC 토큰 복사/붙여넣기 과정이 자동으로 생략됩니다.)*

### Step 2. 타겟 환경 변수 설정 (핵심 단계 ⭐)
노트북이 열리면 상단의 **[설정: 프로젝트 구성 및 인증]** 셀(세 번째 코드 셀)을 찾아 사용자님의 환경에 맞게 값을 입력합니다.

```python
# 전환 작업을 진행할 타겟 프로젝트 지정
PROJECT_ID = "PROJECT_ID를 입력하세요"

```
*새로운 프로젝트에서 처음 구동하시는 경우, 셀 내부의 `ENABLE_APIS` 체크박스를 선택(`True`)하여 필수 DFCX 및 생성형 AI 서비스 API가 자동 개통되도록 유도하는 것을 권장합니다.*

### Step 3. 셀 실행 및 이중 인증 진행
설정값을 입력한 셀의 좌측 **실행(▶) 버튼**을 누릅니다. 안전한 백엔드 권한 연동을 위해 다음 두 가지 인증이 연속으로 나타납니다.

1.  **Colab 계정 연결**: 팝업창에서 구글 계정 액세스를 허용합니다.
2.  **GCP 인증 토큰(ADC) 생성 및 복사**: 
    *   출력 로그 하단에 나타나는 **파란색 인증 URL 링크**를 클릭합니다.
    *   새 창에서 승인을 완료하고 화면에 출력된 **인증 코드(Authorization Code)를 복사**합니다.
    *   다시 Colab으로 돌아와 하단 입력창에 코드를 붙여넣고 `Enter` 키를 누릅니다.

### Step 4. 대시보드 구동 및 전환 수행
1.  이어지는 **Core Migration Code** 섹션 아래의 모듈 셀들을 차례로 모두 실행하여 백엔드 엔진을 메모리에 로드합니다.
    *(상단 메뉴의 `런타임 > 이후 셀 실행`을 누르면 원클릭으로 로딩됩니다.)*
2.  중반부의 **Interactive Migration Dashboard** 셀을 실행하면 출력 영역에 시각화된 **대시보드 UI 위젯**이 생성됩니다.
3.  위젯의 입력창에 소스 DFCX 에이전트 경로를 넣고 **[START MIGRATION]** 버튼을 누르면 전환 프로세스가 실시간 로그와 함께 수행됩니다!

### Step 5. 전환된 에이전트 테스트 및 검증
마이그레이션 완료 후 대시보드 하단 또는 이어지는 **Extras > Talk To Your Migrated Agent** 섹션을 통해 결과를 즉시 검증합니다.
- 제공되는 내장 테스트 코드(`session_client.run(...)`)를 구동하여 새로 생성된 CXAS 에이전트와 직접 대화하며 올바른 플로우 전개 여부를 확인합니다.

---

## 🛠 부가 기능 및 툴 관리 (Extras)

이 도구는 마이그레이션 기능 외에도 CX Agent Studio를 다루기 위한 **다양한 관리 및 실험용 스크립트**를 포함하고 있습니다.
- **수동 에이전트/앱 생성**: 코드를 통해 CXAS App 및 Agent를 직접 구성하는 예제.
- **도구(Tools) 연동 및 관리**: 
  - `OpenAPI Tool`: 외부 REST API 연동 규격 생성
  - `Data Store Tool`: 문서 검색 및 RAG 연동
  - `Python Tool`: 커스텀 파이썬 스크립트 실행 도구 연동
  - `Vertex RAG Engine Tool` 등 고급 연동 지원
- **리소스 정리(Cleanup)**: 생성된 테스트 예제(Examples), 앱(Apps), 도구(Tools) 등을 간편하게 삭제하는 기능.

> [!TIP]
> **콘솔 테스트 시 주의사항**
> 에이전트 설정 및 테스트 섹션에 안내된 바와 같이, 특정 환경에서 콘솔 링크를 통해 직접 테스트할 때 알려진 내부 버그(Internal Error)가 발생할 수 있으므로, 가급적 제공되는 세션 클라이언트(Sessions client) 코드를 통한 검증을 권장합니다.