# ACG Character Settei (Codex)

`acg-character-settei-codex`는 Codex의 내장 이미지 생성 기능으로 애니메이션/게임 제작용 **캐릭터 설정화 한 장**을 만드는 Skill입니다.

정면·측면·후면 전신, 표정 시트, 헤어/의상/액세서리 디테일, 기본 색상 팔레트를 한 장의 가로형 model sheet에 묶습니다. 예쁜 단일 일러스트보다 여러 뷰 사이의 identity consistency와 설정자료로서의 가독성을 우선합니다.

## 무엇을 해결하나요?

캐릭터 이미지를 장면별로 따로 생성하면 머리 모양, 의상 구조, 액세서리, 색상이 쉽게 달라집니다. 이 Skill은 생성 전에 짧은 **Master Identity Summary**를 만들고, 모든 패널을 한 번에 생성하도록 프롬프트를 고정합니다.

지원 입력은 다음과 같습니다.

- 캐릭터 설명만 있는 텍스트 brief
- 캐릭터 identity를 보여 주는 참고 이미지 한 장 또는 여러 장
- 설정화의 배치/선/배경을 참고할 template image

캐릭터 참고 이미지는 identity용, template image는 layout용으로 분리해 처리합니다. identity와 layout이 충돌하면 identity를 우선합니다.

## 기존 `acg-character-settei`와 다른 점

원본 Skill의 좋은 구조인 `layout lock + identity lock + QA`를 유지합니다. 달라진 점은 다음과 같습니다.

| 항목 | 기존 Skill | Codex 버전 |
|---|---|---|
| 이미지 backend | Gemini 이미지 생성 | Codex built-in `image_gen` |
| 템플릿 | 사실상 template + character 두 이미지 전제 | 텍스트만으로도 생성 가능, template은 선택 |
| reference 우선순위 | template을 먼저 전달하는 패턴 | `Image A = identity`, `Image B = layout` 역할을 명시하고 identity 우선 |
| 결과 중심 | 3-view + 표정 + callout + palette | 동일한 한 장 중심, optional 3/4 view와 1회 보정 규칙 추가 |
| 인증/외부 의존 | Gemini 키와 `google-genai` 필요 | 외부 API, Gemini, CLI fallback, API key 불필요 |

Codex의 공식 GPT Image 2 문서는 이미지 입력과 고충실도 image input을 지원한다고 설명합니다. 이 Skill은 해당 API를 직접 호출하지 않고, Codex에서 제공하는 built-in image generation 도구 경로를 사용합니다. [GPT-Image-2 공식 문서](https://developers.openai.com/api/docs/models/gpt-image-2)

## 사용 방법

이 디렉터리를 Codex가 읽는 Skills 위치에 설치하거나 프로젝트 Skill로 등록한 뒤 다음처럼 요청합니다.

```text
$acg-character-settei-codex
일본풍 청소년 여성 캐릭터 설정화를 만들어줘.
검은 긴 생머리, 붉은 리본, 흰색과 남색의 무녀풍 의상,
차분하지만 강단 있는 성격. 정면/측면/후면과 표정 시트를 포함해줘.
```

설치 환경에 따라 `$CODEX_HOME/skills/` 아래에 폴더를 복사해 사용할 수 있습니다. 이 저장소의 구현 위치는 다음과 같습니다.

```text
skills/content/acg-character-settei-codex/
```

실행 시 Skill은 다음 순서로 동작합니다.

1. 설명과 참고 이미지를 brief로 정리합니다.
2. 변하면 안 되는 헤어, 얼굴, 실루엣, 의상 구조, 액세서리, 신발, 색상을 Master Identity Summary로 고정합니다.
3. 한 장의 가로형 settei prompt를 구성합니다.
4. Codex built-in image generation으로 한 번에 생성합니다.
5. front/side/back, 표정, detail, palette, identity consistency를 시각적으로 확인합니다.
6. 필수 요소가 빠졌을 때만 한 번의 targeted edit/regeneration을 수행합니다.

## 출력 형태

권장 구성은 다음과 같습니다.

- 왼쪽 또는 중앙: 큰 full-body front / side / back lineup
- 한쪽 보조 영역: optional 3/4 view
- 오른쪽 또는 하단: neutral, smile, angry, surprised, sad, embarrassed 등 4~6개 표정
- 하단 callout: hair, eyes, outfit, shoes, accessory 디테일
- 한쪽 여백: 주요 색 4~8개의 palette swatches
- 흰색 또는 밝은 중립색 배경, 얇은 선, 절제된 음영, 짧은 라벨

생성 모델의 이미지 내 텍스트는 완전히 정확하지 않을 수 있으므로 긴 일본어 설명문은 요구하지 않습니다. `FRONT`, `SIDE`, `BACK`, `EXPRESSIONS`, `DETAILS`, `PALETTE` 같은 짧은 표시는 보조 정보로만 사용합니다.

## 입력 예시

### 1. 텍스트만

```text
일본풍 청소년 여성 캐릭터 설정화를 만들어줘.
검은 긴 생머리, 붉은 리본, 흰색과 남색의 무녀풍 의상,
차분하지만 강단 있는 성격이야.
정면/측면/후면, 표정 6종, 의상 디테일, 색상 팔레트를 넣어줘.
```

예상 결과: 흰색 배경의 한 장짜리 가로형 settei sheet. 세 전신 뷰는 같은 긴 생머리·붉은 리본·무녀풍 의상을 유지하고, 보조 영역에 표정 6종과 디테일/색상칩을 배치합니다.

### 2. 캐릭터 참고 이미지 포함

```text
첨부한 캐릭터 이미지를 바탕으로 애니메이션용 settei sheet를 만들어줘.
동일한 캐릭터로 정면/측면/후면, 표정 6종,
의상 디테일, 색상 팔레트를 포함해줘.
배경은 흰색으로 하고 장면 연출은 넣지 마.
```

예상 결과: 첨부 이미지는 Image A identity reference로 사용됩니다. 얼굴 특징, 헤어 실루엣, 의상 구조, 색상, 액세서리는 유지하고, 시트 배치만 새로 구성합니다.

### 3. 성인 여성 / 가족 포지션

```text
딸 캐릭터의 엄마 포지션 캐릭터가 필요하다.
성인 여성, 일본풍, 기모노 또는 무녀복, 딸과는 다른 배색.
character sheet 형식으로 정면/측면/후면과 표정 시트를 만들어줘.
무기와 복잡한 배경은 제외해줘.
```

예상 결과: 성인다운 비율과 분위기를 유지한 단정한 의상 설계자료. 사용자가 지정하지 않은 무기나 장면 소품은 추가하지 않습니다.

### 4. template image까지 포함

```text
첫 번째 이미지는 레이아웃 참고용 settei template이고,
두 번째 이미지는 캐릭터 identity reference야.
캐릭터는 두 번째 이미지와 동일하게 유지하고,
첫 번째 이미지의 패널 밀도와 깨끗한 흰색 배치를 참고해서
front/side/back, 표정 6종, 디테일, palette를 한 장에 담아줘.
```

실행 시 입력 이미지의 역할을 프롬프트에 다시 적습니다. 도구가 입력 순서를 바꿀 수 있으면 identity를 먼저, template을 다음에 전달하고 `Image A/B`로 명시합니다. 첨부 이미지 순서를 바꿀 수 없으면 실제 이미지 인덱스를 정확히 적습니다. 어느 경우에도 template은 레이아웃만 결정하고 캐릭터 identity를 덮어쓰지 않습니다.

## Brief 템플릿

상세 입력이 필요하면 [references/brief-template.md](references/brief-template.md)를 복사해 채울 수 있습니다. 실제 생성 프롬프트의 기본 구조와 보정 예시는 [references/prompt-guide.md](references/prompt-guide.md)에 있습니다.

## 제한사항

- 생성 모델은 서로 다른 뷰에서 100% 동일한 캐릭터를 보장하지 않습니다. 이 Skill은 한 장 생성, 고정 anchor, reference 역할 명시, 1회 보정으로 drift를 줄입니다.
- 참고 이미지가 많고 서로 일관될수록 안정적입니다. 서로 다른 의상/헤어 디자인이 섞여 있으면 가장 완성도 높은 최신 이미지를 main anchor로 선택합니다.
- 텍스트 설명이 너무 빈약하면 얼굴·체형·의상 구조가 모델의 일반적인 해석으로 채워질 수 있습니다.
- 16:9 한 장에 많은 정보를 넣으므로 작은 표정이나 라벨은 축소 시 읽기 어려울 수 있습니다.
- 이미지 내 긴 일본어 라벨은 정확한 문서 텍스트로 신뢰하지 말고, 필요하면 결과 후 별도 편집을 요청하세요.
- built-in image generation 도구가 제공되지 않는 실행 환경에서는 Gemini나 다른 외부 backend로 자동 전환하지 않습니다.

## 범위 밖

웹앱, GUI, 데이터베이스, 장문의 캐릭터 바이블, Gemini/HuggingFace/Fal/Replicate 연동, 별도 멀티파일 생성 파이프라인은 이 Skill의 범위가 아닙니다.
