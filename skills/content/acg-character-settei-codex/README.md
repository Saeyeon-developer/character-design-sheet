# ACG Character Settei (Codex)

`acg-character-settei-codex`는 Codex의 내장 이미지 생성 기능으로 애니메이션·게임 제작용 **한 장짜리 캐릭터 설정화**를 만드는 Skill입니다.

한 장의 가로형 settei sheet에 정면·측면·후면 전신, 선택적 3/4 view, 표정 시트, 헤어·의상·액세서리 디테일, 색상 팔레트를 배치합니다. 모든 패널에서 같은 헤어스타일, 얼굴, 신체 비율, 의상 구조, 액세서리, 색상을 유지하는 것을 가장 중요하게 다룹니다.

## 기능

- 텍스트 character brief만으로 설정화 생성
- 캐릭터 identity reference 이미지 1장 또는 여러 장 반영
- 선택적인 template image를 레이아웃·패널 구성 참고용으로 사용
- Codex built-in image generation(`image_gen`)으로 한 장에 여러 뷰 생성
- front / side / back 전신 turnaround 구성
- neutral, smile, angry, surprised, sad, embarrassed 등 4~6개 표정 배치
- hair, eyes, outfit, shoes, accessory 디테일 callout 구성
- 주요 색상 4~8개의 palette swatch 구성
- 생성 결과 시각 QA와 최대 1회의 targeted correction

## 사용 시점

다음과 같은 요청에 사용합니다.

- “캐릭터 설정화를 만들어줘.”
- “정면/측면/후면이 있는 캐릭터 시트를 만들어줘.”
- “표정 시트와 의상 디테일을 포함한 애니메이션 settei가 필요해.”
- “이 캐릭터 이미지를 바탕으로 production model sheet를 만들어줘.”

단일 일러스트, 스토리 장면, 포스터, 또는 여러 뷰가 필요 없는 일반 concept art 요청에는 사용하지 않습니다.

## 입력 방법

### 텍스트 brief

가능한 범위에서 다음 정보를 포함합니다.

- 나이감과 성별/표현
- 성격, 분위기, 세계관·장르
- 헤어 길이·형태·앞머리·색상·장식
- 얼굴형, 눈 색상·형태, 특징적인 표식
- 신체 비율과 전체 실루엣
- 의상 레이어, 칼라·소매·여밈·기장, 색상·문양
- 신발, 액세서리, 선택적 소품
- 원하는 표정, 3/4 view 여부, 제외할 요소

설명이 짧아도 생성에 필요한 production detail은 내부적으로 보강합니다. 사용자가 요청하지 않은 이름, 무기, 로고, 장면 소품, 복잡한 배경은 추가하지 않습니다.

### 참고 이미지

- 캐릭터 이미지: identity reference로 사용합니다. 헤어, 얼굴, 눈, 실루엣, 의상 구조, 신발, 액세서리, 색상을 보존합니다.
- 캐릭터 이미지가 여러 장이면 가장 완성도가 높고 전체 디자인을 잘 보여 주는 이미지를 main anchor로 선택하고, 나머지는 누락된 디테일 확인에 사용합니다.
- template image: panel hierarchy, 간격, 선, 배경, annotation 스타일만 참고합니다. 캐릭터 identity를 결정하지 않습니다.
- identity와 layout이 충돌하면 identity를 우선합니다.

이미지 역할은 생성 프롬프트에도 명시합니다.

```text
Image A: character identity reference. Preserve the character design.
Image B: layout/style reference for sheet composition only.
```

첨부 이미지에 로컬 경로가 있으면 먼저 시각적으로 확인한 후 `referenced_image_paths`를 사용합니다. 경로가 없으면 모든 참고 이미지를 포함하는 최소 `num_last_images_to_include`를 사용합니다. 두 방식을 동시에 사용하지 않습니다.

## 생성 결과

최종 결과는 흰색 또는 밝은 중립 배경의 가로형 raster image 1장입니다.

- 큰 영역: full-body FRONT / SIDE / BACK
- 보조 영역: 필요할 때만 3/4 view
- 표정 영역: 최소 4개, 가능하면 6개
- 디테일 영역: hair, eyes, outfit, shoes, accessory
- 팔레트 영역: 주요 색상 4~8개
- 짧은 라벨: `FRONT`, `SIDE`, `BACK`, `EXPRESSIONS`, `DETAILS`, `PALETTE`

장식적인 장면 배경, 과도한 이펙트, 포스터형 구도, 동세 중심 포즈는 배제합니다. 긴 일본어 문장은 이미지 안에서 정확하게 렌더링되지 않을 수 있으므로 짧은 라벨과 시각 정보 중심으로 구성합니다.

## 에이전트 동작 순서

1. 요청에서 나이감, 분위기, 헤어, 얼굴, 실루엣, 의상, 액세서리, 팔레트, 참고 이미지 역할을 추출합니다.
2. 모든 패널에서 변하면 안 되는 요소를 짧은 **Master Identity Summary**로 고정합니다.
3. `character design sheet`, `settei sheet`, `full body turnaround`, `front, side, back views`, `expression sheet`, `detail callouts`, `color palette`, `same character consistently`를 포함한 한 장짜리 프롬프트를 구성합니다.
4. Codex built-in image generation을 한 번 호출해 전체 시트를 생성합니다. front, side, back, 표정 이미지를 별도 호출로 나누지 않습니다.
5. 생성 이미지를 직접 확인하고 required view, identity consistency, 표정 수, 디테일, palette, 배경, 불필요한 요소를 검사합니다.
6. 필수 요소 누락이나 명확한 identity drift가 있으면 가장 영향이 큰 한 가지 문제만 지정해 최대 1회 수정합니다. 수정 후에도 실패하면 결과와 남은 한계를 간단히 보고합니다.

## 사용 예시

### 텍스트만 입력

```text
일본풍 청소년 여성 캐릭터 설정화를 만들어줘.
검은 긴 생머리, 붉은 리본, 흰색과 남색의 무녀풍 의상,
차분하지만 강단 있는 성격이야.
정면/측면/후면, 표정 6종, 의상 디테일, 색상 팔레트를 넣어줘.
```

### 캐릭터 reference 포함

```text
첨부한 캐릭터 이미지를 바탕으로 애니메이션용 settei sheet를 만들어줘.
동일한 캐릭터로 정면/측면/후면, 표정 6종,
의상 디테일, 색상 팔레트를 포함해줘.
흰색 배경으로 하고 장면 연출은 넣지 마.
```

### 성인 여성 캐릭터

```text
딸 캐릭터의 엄마 포지션 캐릭터가 필요하다.
성인 여성, 일본풍, 기모노 또는 무녀복, 딸과는 다른 배색.
character sheet 형식으로 정면/측면/후면과 표정 시트를 만들어줘.
무기와 복잡한 배경은 제외해줘.
```

### Skill 명시 호출

```text
$acg-character-settei-codex
위 brief와 첨부 reference를 사용해 한 장의 anime character settei sheet를 생성해줘.
```

## 품질 기준과 제한사항

- identity drift를 view 누락보다 높은 우선순위로 처리합니다.
- 페이지가 복잡해지면 optional 3/4 view나 긴 라벨을 먼저 줄이고, front / side / back과 identity anchor는 유지합니다.
- 한 장 생성과 reference anchor는 일관성을 높이지만 100% 동일성을 보장하지 않습니다.
- 참고 이미지의 디자인이 서로 다르면 main anchor를 기준으로 선택하며, 충돌하는 요소를 평균내지 않습니다.
- built-in image generation이 없는 환경에서는 외부 이미지 API나 다른 backend로 전환하지 않습니다.
- 작은 표정이나 긴 라벨은 축소 시 읽기 어려울 수 있습니다.

## 관련 파일

- [SKILL.md](SKILL.md): Codex가 실제 작업에 적용하는 동작 규칙
- [brief-template.md](references/brief-template.md): 입력 brief 작성 양식
- [prompt-guide.md](references/prompt-guide.md): 생성 프롬프트와 targeted correction 가이드
