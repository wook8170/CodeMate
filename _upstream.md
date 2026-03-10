# 🔄 Upstream Merge Guide: CodeMate & Cline

이 문서는 CodeMate 프로젝트가 Cline 오리지널 소스(Upstream)의 업데이트를 안전하게 수용하고 리브랜딩 정합성을 유지하기 위한 머지 전략을 기술합니다.

---

## 🏗️ 기본 머지 원칙

1. **로직은 수용, 브랜딩은 보존**: AI의 핵심 로직과 새로운 기능은 적극적으로 머지하되, 이름(CodeMate)과 고유 식별자(ID)는 우리 설정을 최우선으로 유지합니다.
2. **명령어 프리픽스 고정**: 내부 명령어는 항상 `cline.*`을 사용하도록 하여 Upstream 코드와의 로직 호환성을 깨뜨리지 않습니다.
3. **충돌 해결 우선순위**: `package.json`, `registry.ts`, 뷰 관련 설정은 항상 **CodeMate(Local)** 버전을 기준으로 해결합니다.

---

## 📑 핵심 파일별 충돌 해결 가이드

### 1. `package.json` (가장 중요)
Upstream 머지 시 가장 빈번하게 충돌이 발생하는 파일입니다.
- **[KEEP] `name`**: `"codemate"` (오리지널: `"claude-dev"`)
- **[KEEP] `publisher`**: `"codemate-ai"` (오리지널: `"saoudrizwan"`)
- **[KEEP] `displayName`**: `"CodeMate"` (오리지널: `"Cline"`)
- **[KEEP] `viewsContainers` & `views`**: 
  - `id`가 `codemate-ActivityBar`, `codemate.SidebarProvider` 등 **codemate** 프리픽스를 사용하는지 확인하십시오.
  - Upstream에서 새로운 뷰가 추가될 경우, 해당 ID를 `codemate.*` 형식으로 변경하여 추가해야 합니다.

### 2. `src/registry.ts`
- **[KEEP] `prefix`**: `const prefix = "cline"`
  - Upstream은 `name === "claude-dev" ? "cline" : name` 형식을 사용할 수 있으나, CodeMate는 하이브리드 모드(명령어 프리픽스는 cline, 폴더명은 codemate)이므로 반드시 `"cline"`으로 고정되어야 합니다.

### 3. `webview-ui/src/components/chat/ChatRow.tsx`
- **[LOGIC]**: 도구 실행 로직(`case "command"`, `"use_mcp_server"` 등)은 Upstream의 변경사항을 따릅니다.
- **[BRAND]**: 노출 텍스트(`"CodeMate wants to..."`, `"CodeMate has a question"`)는 우리 쪽의 수정한 텍스트를 유지하십시오.

### 4. `webview-ui/src/components/onboarding/data-steps.ts`
- **[BRAND]**: 온보딩 제목 `"How will you use CodeMate?"`를 유지하십시오.

---

## 📊 리브랜딩 수정 내역 및 충돌 해결 전략 (Conflict Resolution Table)

Upstream과 머지 시 아래 표를 참고하여 충돌을 해결하십시오. **"유지"** 항목은 CodeMate의 설정을 고수해야 하며, **"수용/수정"** 항목은 로직은 받되 텍스트만 다시 고치는 방식입니다.

| 대상 파일 | 수정 항목 | Upstream 값 (Cline) | CodeMate 값 (Local) | 충돌 발생 시 조치 방향 |
| :--- | :--- | :--- | :--- | :--- |
| `package.json` | 확장 식별자 | `"name": "claude-dev"` | `"name": "codemate"` | **CodeMate 값 유지** (가장 중요) |
| `package.json` | 발행자 ID | `"publisher": "saoudrizwan"` | `"publisher": "codemate-ai"` | **CodeMate 값 유지** |
| `package.json` | 표시 명칭 | `"displayName": "Cline"` | `"displayName": "CodeMate"` | **CodeMate 값 유지** |
| `package.json` | 뷰 컨테이너 ID | `"id": "claude-dev-ActivityBar"` | `"id": "codemate-ActivityBar"` | **CodeMate 값 유지** (아이콘 로딩 관련) |
| `package.json` | 뷰 ID | `"id": "claude-dev.SidebarProvider"` | `"id": "codemate.SidebarProvider"` | **CodeMate 값 유지** (사이드바 로딩 관련) |
| `src/registry.ts` | 명령어 프리픽스 | `prefix = ... (동적)` | `const prefix = "cline"` | **CodeMate 값 유지** (로직 호환성 필수) |
| `ChatRow.tsx` | 도구 실행 문구 | `"Cline wants to..."` | `"CodeMate wants to..."` | Upstream 로직 수용 후 **텍스트만 재수정** |
| `data-steps.ts` | 온보딩 질문 | `"How will you use Cline?"` | `"How will you use CodeMate?"` | **CodeMate 값 유지** |
| `*.tsx` (Pickers) | 모델 가이드 | `"Cline works best with..."` | `"CodeMate works best with..."` | **CodeMate 값 유지** |
| `extension.ts` | 뷰 등록 ID | `SIDEBAR_ID = ...` | `ExtensionRegistryInfo.viewId` | **CodeMate 값 유지** (registry.ts와 연동) |

---

## 🛠️ 머지 실행 표준 절차 (Step-by-Step)

1. **Upstream 동기화**:
   ```bash
   git fetch upstream main
   ```

2. **작업 브랜치 생성 (권장)**:
   ```bash
   git checkout -b feature/merge-upstream-$(date +%Y%m%d)
   ```

3. **머지 시도**:
   ```bash
   git merge upstream/main
   ```

4. **충돌 해결**:
   - 위 [핵심 파일별 가이드]를 참고하여 충돌을 수동으로 해결합니다.
   - 특히 `package.json`의 `version` 정보는 Upstream의 버전을 따르되, 패키징 시 우리 고유의 버저닝 정책이 필요한지 검토하십시오.

5. **종속성 및 빌드 검증**:
   ```bash
   npm install        # 새로운 라이브러리 수용
   npm run build:webview
   npx vsce package --out CodeMate-merge-test.vsix
   ```

6. **회귀 테스트**:
   - 사이드바 아이콘이 정상적으로 표시되는가?
   - 명령어(`CMD+Shift+P` -> `CodeMate`)가 정상 작동하는가?
   - 대화창의 'Cline' 잔여 명칭이 발생하지 않았는가?

---

## 🛑 주의사항 (Gotchas)

- **WebView 전용 명칭**: `App.stories.tsx` 등 테스트 코드에서 'Cline'을 찾는 테스트가 실패할 수 있습니다. 이미 교체된 텍스트(`CodeMate`)로 테스트 코드가 작성되어 있는지 확인하십시오.
- **식별자 매칭**: `extension.ts`에서 등록하는 `SIDEBAR_ID`가 `package.json`의 `view` ID와 일치하지 않으면 사이드바가 로드되지 않고 무한 로딩(새로고침 아이콘)이 발생합니다.

---
**최종 업데이트**: 2026-03-10
**작성자**: CodeMate Rebranding Team (Dean Borisov)
