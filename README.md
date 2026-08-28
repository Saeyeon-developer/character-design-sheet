# ACG Character Settei (Codex)

이 저장소의 메인 README입니다. Codex에서 `$acg-character-settei-codex` Skill을 사용해 애니메이션·게임 제작용 캐릭터 설정화 1장을 생성할 수 있습니다.

## 제공 기능

- 텍스트 character brief 또는 캐릭터 reference image 입력
- 선택적인 layout/template image 지원
- 한 장의 가로형 anime settei sheet 생성
- full-body front / side / back views
- 최소 4개, 권장 6개의 표정 시트
- hair, eyes, outfit, shoes, accessory 디테일
- 4~8개 주요 색상의 palette swatches
- 동일 캐릭터 유지 중심의 Master Identity Summary와 시각 QA
- 누락이나 명확한 identity drift가 있을 때 최대 1회의 targeted correction

## 다른 agent가 사용하는 방법

요청이 캐릭터 설정화·캐릭터 시트·정면/측면/후면 turnaround·표정 시트 생성에 해당하면 `skills/content/acg-character-settei-codex/SKILL.md`를 적용합니다.

가장 간단한 호출 예시는 다음과 같습니다.

```text
$acg-character-settei-codex
일본풍 청소년 여성 캐릭터 설정화를 만들어줘.
검은 긴 생머리, 붉은 리본, 흰색과 남색의 무녀풍 의상,
차분하지만 강단 있는 성격.
정면/측면/후면, 표정 6종, 의상 디테일, 색상 팔레트를 포함해줘.
```

참고 이미지가 있으면 캐릭터 identity용인지 layout용인지 구분해 설명합니다.

```text
첨부한 이미지는 캐릭터 identity reference야.
헤어, 얼굴, 눈, 실루엣, 의상 구조, 액세서리, 색상을 유지해서
정면/측면/후면과 표정 6종이 있는 한 장의 settei sheet를 만들어줘.
흰색 배경, 짧은 라벨, 디테일 callout, 색상 팔레트를 포함해줘.
```

## 입력 규칙

- 텍스트만 있어도 생성합니다.
- 캐릭터 reference가 여러 장이면 가장 완성도 높고 전체 디자인을 잘 보여 주는 이미지를 main identity anchor로 사용합니다.
- template image는 패널 배치와 스타일만 결정하며 캐릭터 identity에는 사용하지 않습니다.
- 사용자가 지정하지 않은 무기, 로고, 장면 소품, 복잡한 배경은 추가하지 않습니다.
- 나이감, 헤어, 얼굴, 눈, 실루엣, 의상 구조, 신발, 액세서리, 색상은 모든 view에서 고정합니다.

## 출력 기준

최종 출력은 흰색 또는 밝은 중립 배경의 raster image 1장입니다. 한 장 안에 다음을 읽을 수 있어야 합니다.

1. full-body FRONT
2. full-body SIDE
3. full-body BACK
4. 4~6개의 표정
5. 헤어·눈·의상·신발·액세서리 디테일
6. 주요 색상 4~8개의 팔레트

예쁜 단일 일러스트보다 제작용 model sheet처럼 정보가 잘 읽히는 배치를 우선합니다. built-in image generation이 없으면 외부 이미지 API로 전환하지 않습니다.

## 저장소 구성

- [Skill 동작 규칙](skills/content/acg-character-settei-codex/SKILL.md)
- [Skill 사용 가이드](skills/content/acg-character-settei-codex/README.md)
- [Brief 템플릿](skills/content/acg-character-settei-codex/references/brief-template.md)
- [Prompt 가이드](skills/content/acg-character-settei-codex/references/prompt-guide.md)
- [테스트 결과](examples/character-sheet-test.png)
- [CodeRabbit 설정](.coderabbit.yaml)

## 테스트 결과

3개의 reference를 사용해 생성한 예시입니다. 한 장에 front / side / back, 6개 표정, 디테일 callout, 색상 팔레트를 배치했습니다.

![Character settei test](examples/character-sheet-test.png)
