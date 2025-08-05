# Varion (Variant + Aeon)

Varion은 대화형 시스템 설계를 위한 텍스트 기반의 도메인 특정 언어(DSL)입니다. 사람이 쉽게 읽고 쓸 수 있으면서도, 기계가 구조적으로 파싱하기 용이하도록 설계되었습니다.

## 주요 특징

- **직관적인 구조**: 노드 선언, 메타 정보, 대사, 선택지 등 대화의 구성 요소를 명확하게 구분하여 가독성이 높습니다.
- **유연한 흐름 제어**: 조건부 선택지와 액션 실행을 지원하여 복잡한 대화 흐름을 만들 수 있습니다.
- **쉬운 확장성**: 간단한 텍스트 기반 형식이므로 다양한 시스템에 쉽게 통합하고 확장할 수 있습니다.
- **유니코드 지원**: 기본적으로 UTF-8 인코딩을 사용하여 다국어 환경에서도 문제없이 작동합니다.

## 파일 확장자

Varion 스크립트 파일은 일반적으로 `.va` 또는 `.vion` 확장자를 사용합니다.

## 문법

Varion의 문법은 몇 가지 간단한 규칙으로 이루어져 있습니다.

### 1. 노드 선언 (`::`)

대화의 각 단락은 `::`으로 시작하는 노드(Node)로 정의됩니다. 노드 이름은 대화의 흐름을 제어하는 식별자로 사용됩니다.

```
::start
```

### 2. 메타 정보 (`@`)

`@` 기호는 해당 노드의 배경 이미지, 등장인물, 화자 등 부가적인 정보를 설정하는 데 사용됩니다.

```
@background: images/bg.png
@characters: images/npc.png, images/player.png
@who: NPC
```

### 3. 대사 본문

노드 선언과 메타 정보 다음에 오는 텍스트는 화면에 표시될 대사입니다.

```
어서 와! 무슨 일이야?
```

### 4. 선택지 (`*`)

`*` 기호는 플레이어가 선택할 수 있는 선택지를 나타냅니다. 각 선택지는 `=>` 기호를 통해 다음으로 이동할 노드를 지정합니다.

```
* 도와줘요! => ask_help
* 그냥 구경 왔어요. => end_neutral
```

### 5. 다음 노드 지정 (`@next`)

선택지가 없는 다이얼로그에서 다음으로 이동할 노드를 명시적으로 지정할 때 사용합니다. `@next` 지시어가 사용되면, 해당 노드에 선택지가 없더라도 지정된 다음 노드로 자동으로 진행됩니다.

```
@next: next_node_name
```

### 6. 액션 (`@action`)

선택지나 노드에 진입했을 때 특정 변수를 설정하거나 함수를 호출하는 등의 액션을 정의할 수 있습니다.

```
@action: set help_requested = 1
```

### 6. 조건부 선택지 (`@if`)

특정 조건이 만족될 때만 선택지가 나타나도록 설정할 수 있습니다.

```
* 미안해요, 시간이 없어요. => help_declined @if reputation < 3
```

## 구현

현재 Varion을 파싱할 수 있는 라이브러리는 Rust용으로만 제공됩니다.

- **varion-rust**: [https://github.com/KimKyuRae/Varion/tree/main/varion-rust](https://github.com/KimKyuRae/Varion/tree/main/varion-rust)

## 전체 예시

```text
::start
@background: images/bg.png
@characters: images/npc.png, images/player.png
@who: NPC

어서 와! 무슨 일이야?

* 도와줘요! => ask_help
* 그냥 구경 왔어요. => end_neutral

::ask_help
@who: NPC
@action: set help_requested = 1

정말? 도와줄 수 있다면 고맙지!

* 네, 도울게요. => help_accepted
* 미안해요, 시간이 없어요. => help_declined @if reputation < 3
```
