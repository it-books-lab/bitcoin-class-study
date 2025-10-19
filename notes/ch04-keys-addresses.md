
<img width="1662" height="1044" alt="image" src="https://github.com/user-attachments/assets/4f077a2e-c87c-4a8e-a8f6-77f62196e429" />

<img width="1667" height="1041" alt="image" src="https://github.com/user-attachments/assets/e5ec777c-bea1-4642-9092-c03412350d5c" />

## 공개키 암호 방식에는 두 가지 사용 목적이 있습니다

| 목적 | 예시 | 사용하는 키 |
| --- | --- | --- |
| **① 비밀 메시지 전달 (기밀성, Confidentiality)** | 송신자가 수신자에게 “남이 못 보는 메시지”를 보내고 싶을 때 | 암호화: **수신자의 공개키**복호화: **수신자의 개인키** |
| **② 송신자 신원 증명 (서명, Authenticity)** | 송신자가 “이 메시지가 내가 보낸 게 맞다”는 걸 증명하고 싶을 때 | 서명(=암호화): **송신자의 개인키**검증(=복호화): **송신자의 공개키** |

---

<img width="1744" height="1219" alt="image" src="https://github.com/user-attachments/assets/2fe2fa1f-1018-4a25-accc-3a2c1932aaef" />

<img width="1837" height="1277" alt="image" src="https://github.com/user-attachments/assets/9b060473-c678-4de8-afad-45db738fc10c" />

이 두 장의 PPT는 **비트코인의 공개키(public key)가 타원곡선 암호(ECC)를 통해 어떻게 생성되는지를 시각적으로 설명하기 위한 자료**입니다.

즉, "비트코인에서 공개키가 어떻게 만들어지는가?"를 보여주는 슬라이드예요.

---

## 🧩 1️⃣ 첫 번째 슬라이드: **Generate Public Key (ECC)**

이 슬라이드는 **타원곡선 암호(Elliptic Curve Cryptography, ECC)** 의 기본 개념을 설명합니다.

### 🔹 주요 내용

- **Elliptic Curve Cryptography**
    
    → 공개키 암호 방식의 한 종류로, 수학적으로 “타원곡선 위의 점 연산”을 사용합니다.
    
    (공개키 = 개인키 × 기준점 G)
    
- **P “dot” Q = R**
    
    → 타원곡선 위 두 점 P, Q를 더하면, 또 다른 점 R이 만들어집니다.
    
    이 덧셈(+) 연산이 바로 ECC의 핵심 연산이에요.
    
    - 그림에서 선이 곡선을 두 번 교차하고, 그 점을 대칭시켜 새로운 점 R을 얻는 과정이 그려져 있죠.
- **Key Length 비교 (RSA vs ECC)**
    - RSA는 3072비트 키를 써야 ECC 256비트 수준의 보안이 나옵니다.
    - 즉, **ECC는 훨씬 더 짧은 키로 같은 보안 강도를 제공**합니다.
        
        → 비트코인은 효율성을 위해 ECC를 채택한 것.
        

---

## 🔢 2️⃣ 두 번째 슬라이드: **Generate Public Key (secp256k1)**

이건 비트코인에서 실제로 사용하는 **특정 곡선(secp256k1)** 을 소개하는 슬라이드예요.

<img width="1421" height="997" alt="image" src="https://github.com/user-attachments/assets/5d5d2db5-f325-4246-9fa2-c372e3dd13b0" />

- **그림의 의미**
    - 타원곡선 위에서 G를 반복 덧셈하면, 새로운 점들이 생깁니다 (2G, 3G, 4G, …).
    - 이렇게 생긴 점 중 하나가 공개키 K입니다.
    - 이 연산은 “한 방향(곱셈)”은 쉽지만, “반대로(나눗셈)”은 극도로 어렵습니다.
        
        → 이것이 비트코인 보안의 핵심입니다.
        

---

## 3️⃣ 두 슬라이드의 연결 요약

| 구분 | 내용 |
| --- | --- |
| **슬라이드 1 (ECC)** | 타원곡선 덧셈의 기본 원리 (P + Q = R) 소개 |
| **슬라이드 2 (secp256k1)** | 비트코인에서 실제로 사용하는 ECC 곡선(secp256k1)과 공개키 생성 공식 설명 |
| **핵심 메시지** | “비트코인의 공개키는 개인키 × 생성점(G)으로 계산된다. 이때의 곱셈은 타원곡선 덧셈을 반복한 결과다.” |

---


<img width="1375" height="897" alt="image" src="https://github.com/user-attachments/assets/b382ae0a-840b-4120-bc2c-5d3b47d26a0e" />

<img width="1293" height="1051" alt="image" src="https://github.com/user-attachments/assets/344dc360-df8b-4182-ac75-76ed1bf24734" />


---

## 🧩 1️⃣ Base58Check란?

**Base58Check**는 비트코인에서 사용하는 **문자열 인코딩 방식**이에요.
비트코인 주소는 그냥 숫자(바이너리 데이터)가 아니라,
사람이 복사·붙여넣기하거나 타이핑할 수 있도록 **영문자+숫자 조합의 문자열**로 표시됩니다.

예:

```
1BoatSLRHtKNngkdXEeobR76b53LETtpyT
```

이 주소는 사실 내부적으로 **버전 + 공개키 해시 + 체크섬(checksum)** 이
Base58 방식으로 인코딩된 결과예요.

---

## 🔢 2️⃣ 왜 “Base58”인가?

비트코인 주소에 **0, O, I, l** 같은 헷갈리는 문자를 제거해서
사람이 읽기 쉬운 58개 문자만 사용하기 때문이에요.

> ✅ 사용되는 문자:
> `123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz`
>
> ❌ 제외된 문자:
> `0` (숫자 0), `O` (대문자 O), `I` (대문자 i), `l` (소문자 L)

그래서 “Base58”이라고 부릅니다 (즉, 58진수 표현).

---

## 🧮 3️⃣ “Check”가 붙은 이유: **오류 방지용 Checksum**

이제 ‘Base58Check’의 핵심인 **체크섬(checksum)** 을 설명할 차례입니다.

체크섬은 비트코인 주소 끝부분에 **4바이트(=8자리 16진수)** 가 추가되는 값이에요.
이 값은 **주소 데이터에서 해시를 두 번 돌려 계산**합니다:

```
checksum = SHA256(SHA256(version + payload))의 앞 4바이트
```

이걸 주소 끝에 붙여서 인코딩합니다.

---

## 💡 4️⃣ 예시로 보기

가상의 데이터를 예로 들어볼게요 👇

| 단계  | 내용                      | 결과 (예시)                                              |
| --- | ----------------------- | ---------------------------------------------------- |
| 1️⃣ | 버전 바이트 (P2PKH 주소는 0x00) | `00`                                                 |
| 2️⃣ | 공개키 해시 (20바이트)          | `89ABCDEF1234567890ABCDEFFEDCBA0987654321`           |
| 3️⃣ | 합치기                     | `0089ABCDEF1234567890ABCDEFFEDCBA0987654321`         |
| 4️⃣ | 더블 SHA256 해시            | `SHA256(SHA256(...)) = 12AB34CD56EF78...`            |
| 5️⃣ | 앞 4바이트를 checksum으로 사용   | `12AB34CD`                                           |
| 6️⃣ | 합치기                     | `0089ABCDEF1234567890ABCDEFFEDCBA098765432112AB34CD` |
| 7️⃣ | Base58로 변환              | `1BoatSLRHtKNngkdXEeobR76b53LETtpyT` 🎉              |

이 문자열이 **최종 비트코인 주소(Base58Check 인코딩 결과)** 입니다.

---

## 🧠 5️⃣ 체크섬의 역할

이제 **체크섬이 왜 필요한지** 이해해볼까요?

* 사용자가 주소를 입력할 때 **한 글자라도 틀리면**,
  지갑 프로그램은 다시 체크섬을 계산해서
  주소 안에 포함된 원래 체크섬과 비교합니다.
* 만약 두 값이 다르면? → ❌ “이 주소는 잘못된 형식입니다.” 오류 발생.
* 즉, **실수로 오타를 입력해도 잘못된 주소로 송금되는 걸 방지**합니다.

> 예를 들어:
>
> ```
> 올바른 주소: 1BoatSLRHtKNngkdXEeobR76b53LETtpyT
> 오타 주소:   1BoatSLRHtKNngkdXEeobR76b53LETtpyX
> ```
>
> Base58Check 검증 시 checksum 불일치 → 오류 감지 ✅
> 그래서 지갑이 전송을 막습니다.

---

## ✅ 6️⃣ 요약 정리

| 개념         | 설명                         |
| ---------- | -------------------------- |
| **Base58** | 사람이 헷갈리지 않도록 만든 58진수 인코딩   |
| **Check**  | 더블 SHA256을 통해 계산된 4바이트 체크섬 |
| **목적**     | 타이핑/복사 실수 등 오류 탐지          |
| **동작 원리**  | 디코딩 시 다시 해시 계산 후 체크섬 비교    |
| **결과**     | 틀린 주소는 자동으로 “유효하지 않음” 처리됨  |

---

📘 **한 문장으로 요약하자면:**

> **Base58Check**는 비트코인 주소를 사람이 안전하게 다루도록 만든 인코딩 방식입니다.
> 주소 끝에 4바이트짜리 **체크섬(checksum)** 을 붙여
> 오타나 전송 오류를 자동으로 탐지하고,
> 잘못된 주소로 송금되는 걸 방지합니다. ✅

---

<img width="1424" height="904" alt="image" src="https://github.com/user-attachments/assets/b931ec4b-eef9-4afc-8cea-c25bfaa92ee1" />

이 표는 **비트코인 주소나 키가 어떤 종류(type)의 데이터인지를 구분하기 위해 Base58Check 인코딩할 때 맨 앞에 붙이는 ‘버전 프리픽스(version prefix)’를 정리한 표**입니다.
즉, 이 표는

> “이 Base58 문자열이 비트코인 주소인지, 테스트넷 주소인지, 개인키인지, 확장 공개키인지 어떻게 구분할 수 있을까?”
> 를 알려주는 표예요.

---

## 🧩 1️⃣ 배경: 왜 ‘버전 프리픽스(version prefix)’가 필요할까?

Base58Check 인코딩은 “바이너리 데이터를 사람이 읽을 수 있는 문자열로 바꾸는 과정”이죠.
그런데 비트코인에는 여러 종류의 데이터가 있습니다:

* 일반 지갑 주소 (P2PKH)
* 스크립트 주소 (P2SH)
* 테스트넷 주소
* 개인키(WIF)
* 암호화된 개인키 (BIP-38)
* 확장 공개키 (BIP-32)

이들을 모두 같은 방식(Base58Check)으로 인코딩하면 **모양은 비슷해 보이지만 서로 완전히 다른 의미**를 갖게 됩니다.
그래서, **맨 앞에 구분자(prefix byte)** 를 붙여서 인코딩합니다.

> 🧠 쉽게 말하면:
> “이건 비트코인 메인넷 주소야”
> “이건 테스트넷 주소야”
> “이건 개인키야”
> 이런 걸 구분하는 **‘태그’ 역할**이에요.

---

## 🧮 2️⃣ 표의 구성

| Type | Version Prefix (hex) | Base58 Result Prefix | 설명 |
| ---- | -------------------- | -------------------- | -- |

### 🟢 Bitcoin Address

* **Prefix (hex)**: `0x00`
* **Base58 시작 문자**: `1`
* **설명**:
  일반적인 **P2PKH 주소** (공개키 해시 기반 주소)
  → 예:

  ```
  1BoatSLRHtKNngkdXEeobR76b53LETtpyT
  ```

  맨 앞의 ‘1’은 바로 prefix 0x00이 Base58로 변환된 결과예요.

---

### 🟠 Pay-to-Script-Hash (P2SH) Address

* **Prefix (hex)**: `0x05`
* **Base58 시작 문자**: `3`
* **설명**:
  스크립트를 통한 다중 서명 주소 등 (예: 스마트 계약용)
  → 예:

  ```
  3J98t1WpEZ73CNmQviecrnyiWrnqRhWNLy
  ```

  ‘3’으로 시작하면 **P2SH 주소**라는 뜻이에요.

---

### 🔵 Bitcoin Testnet Address

* **Prefix (hex)**: `0x6F`
* **Base58 시작 문자**: `m` 또는 `n`
* **설명**:
  메인넷이 아닌 **테스트 네트워크용 주소**
  (개발·실험용)
  → 예:

  ```
  mipcBbFg9gMiCh81Kj8tqqdgoZub1ZJRfn
  ```

---

### 🟣 Private Key WIF (Wallet Import Format)

* **Prefix (hex)**: `0x80`
* **Base58 시작 문자**: `5`, `K`, `L`
* **설명**:
  개인키를 사람이 입력할 수 있는 Base58Check 형태로 바꾼 것
  → 예:

  ```
  5HueCGU8rMjxEXxiPuD5BDu...
  L3oJc2y7DqMZ7zjZt3Vj9u...
  ```

  압축 키냐 비압축 키냐에 따라 5 또는 K/L로 시작합니다.

---

### 🟤 BIP-38 Encrypted Private Key

* **Prefix (hex)**: `0x0142`
* **Base58 시작 문자**: `6P`
* **설명**:
  비밀번호로 암호화된 개인키
  (BIP-38 표준 방식)
  → 예:

  ```
  6PYSx6ZsM5QbyP6k3kz...
  ```

---

### ⚫ BIP-32 Extended Public Key (xpub)

* **Prefix (hex)**: `0x0488B21E`
* **Base58 시작 문자**: `xpub`
* **설명**:
  **계층적 지갑(HD Wallet)** 구조에서 상위 공개키를 표현하는 값
  → 예:

  ```
  xpub6CUGRUonZSQ4TWtTMmzXdrXDtyPWKiKpDd...
  ```

  `xpub`로 시작하면 “이건 여러 지갑 주소를 파생시킬 수 있는 공개키”입니다.

---

## 🧠 3️⃣ 정리 요약

| 구분                             | Prefix (hex) | Base58 시작 | 의미         |
| ------------------------------ | ------------ | --------- | ---------- |
| Bitcoin Address (P2PKH)        | 0x00         | 1         | 일반 주소      |
| P2SH Address                   | 0x05         | 3         | 스크립트 주소    |
| Testnet Address                | 0x6F         | m, n      | 테스트 주소     |
| Private Key (WIF)              | 0x80         | 5, K, L   | 개인키        |
| Encrypted Private Key (BIP-38) | 0x0142       | 6P        | 암호화된 개인키   |
| Extended Public Key (BIP-32)   | 0x0488B21E   | xpub      | 계층형 지갑 공개키 |

---

## 📘 4️⃣ 요약 문장

> 이 표는 Base58Check 인코딩 시 **데이터의 종류를 구분하기 위한 “버전 프리픽스”와, 그 결과로 만들어지는 주소의 시작 문자**를 보여줍니다.
>
> 즉, 비트코인 지갑은 문자열의 첫 글자(예: `1`, `3`, `xpub`, `5` 등)를 보고
> “이게 어떤 타입의 데이터인지(주소인지, 개인키인지, 테스트넷인지)”를 즉시 식별할 수 있습니다. ✅

---


<img width="1790" height="1097" alt="image" src="https://github.com/user-attachments/assets/279e0a59-0335-47e8-800e-f023f678284f" />


이 슬라이드는 **“비트코인 개인키(Private Key)를 다양한 형식으로 변환하는 과정(Base58Check ↔ Hex)”** 을 보여주는 예제입니다.
즉, **Base58Check 형식(WIF)과 16진수 형식(Hex)** 이 서로 어떻게 변환되는지를 명령어 예시로 보여주는 거예요.

---

## Base58Check → Hex (디코딩)

```bash
$ sx base58check-decode 5J3mBbAH58CpQ3Y5RNJpUKPE62SQ5tfcvU2JpbnkeyhfsYB1Jcn
1e99423a4ed27608a15a2616a2b0e9e52ced330ac530edcc32c8ffc6a526aedd 128
```

📘 해석:

| 부분                 | 의미                                         |
| ------------------ | ------------------------------------------ |
| `5J3mBbAH...`      | Base58Check 형식의 WIF 개인키 (사람이 읽는 형태)        |
| `1e99423a...6aedd` | 디코딩 후 원래의 16진수(HEX) 개인키                    |
| `128 (0x80)`       | Base58Check 인코딩 시 앞에 붙어 있던 prefix (버전 바이트) |

즉, WIF 형식의 키를 Base58Check로 디코딩하면:

* 16진수 개인키 본체
* prefix `0x80` (WIF용)
* checksum
  이렇게 분리된다는 뜻이에요.

---

## Hex → Base58Check (인코딩)

```bash
$ sx base58check-encode 1e99423a4ed27608a15a2616a2b0e9e52ced330ac530edcc32c8ffc6a526aedd 128
5J3mBbAH58CpQ3Y5RNJpUKPE62SQ5tfcvU2JpbnkeyhfsYB1Jcn
```

📘 해석:

| 부분                 | 의미                                      |
| ------------------ | --------------------------------------- |
| `1e99423a...6aedd` | 32바이트짜리 개인키 (16진수)                      |
| `128`              | Base58Check 인코딩 시 버전 바이트 (`0x80` → 128) |
| 결과: `5J3mBbAH...`  | Base58Check로 인코딩된 WIF 키 (‘5’로 시작)       |

> 즉, “Hex 개인키 + prefix 0x80 + checksum” → Base58 인코딩 → 사람이 읽을 수 있는 문자열(WIF)
> 이 과정을 보여주는 거예요.

---

## 🔵 4️⃣ 세 번째: 압축된 개인키 (Compressed Key) 인코딩

```bash
$ sx base58check-encode 1e99423a4ed27608a15a2616a2b0e9e52ced330ac530edcc32c8ffc6a526aedd01 128
KxFC1jmwwCoACiCAWZ3eXa96mBM6tb3TYzGmf6YwgdGWZgawvrtJ
```

📘 해석:

| 부분                | 의미                                      |
| ----------------- | --------------------------------------- |
| 끝의 `01`           | 압축 공개키용 구분자 (압축된 공개키를 사용함을 표시)          |
| `128`             | 동일한 prefix (WIF 표준용)                    |
| 결과: `KxFC1jmw...` | 압축 WIF 형식의 Base58Check 키 (‘K’나 ‘L’로 시작) |

즉,
“압축 공개키를 사용하는 지갑”에서는 개인키 끝에 `0x01`을 추가한 다음
Base58Check로 인코딩해서 `K` 또는 `L`로 시작하는 문자열을 만듭니다.

---

## 전체 흐름 요약

| 변환 방향                 | 입력                             | 출력                             | 설명                           |
| --------------------- | ------------------------------ | ------------------------------ | ---------------------------- |
| Base58Check → Hex     | `5J3mBbAH...`                  | `1e99423a...6aedd`, prefix=128 | 사람이 읽는 키(WIF)를 내부 16진수 키로 변환 |
| Hex → Base58Check     | `1e99423a...6aedd`, prefix=128 | `5J3mBbAH...`                  | 내부 키를 WIF 문자열로 변환            |
| Hex(압축) → Base58Check | `1e99...6aedd01`, prefix=128   | `KxFC1jmww...`                 | 압축 공개키용 개인키 인코딩 (K/L로 시작)    |

---

<img width="1625" height="999" alt="image" src="https://github.com/user-attachments/assets/7bbcc119-2ee5-4e4f-ba04-fd0c9a59db29" />

해당 내용은 비트코인과 같은 **타원곡선 암호(Elliptic Curve Cryptography, ECC)** 의 공개키 표현 방식을 다루는 내용입니다.

---

## 🔐 1️⃣ 공개키의 정의

공개키(Public Key) **K**는 타원곡선 상의 한 점으로 정의됩니다.
즉,
[
K = (x, y)
]
로 표현됩니다.

예를 들어,

```
x = F028892BAD...DC341A
y = 07CF33DA18...505BDB
```

처럼 각각 256비트(32바이트)의 정수 좌표값으로 이루어집니다.

즉, 하나의 공개키는 `(x좌표, y좌표)`로 구성된 **점(point)** 입니다.

---

## 🔑 2️⃣ 공개키 표현 방식 (Public Key Formats)

타원곡선 암호에서 공개키는 **여러 형식으로 표현될 수 있는데**, 가장 대표적인 두 가지는 다음과 같습니다.

* **압축되지 않은 공개키 (Uncompressed Public Key)**
* **압축된 공개키 (Compressed Public Key)**

---

## 🧩 3️⃣ Uncompressed Public Key (압축되지 않은 공개키)

이 방식은 공개키를 다음과 같이 표현합니다.

```
04 + x좌표 + y좌표
```

즉,

* **04** : “압축되지 않은(uncompressed)” 공개키임을 나타내는 **prefix(접두 바이트)**
* 그 뒤에 32바이트짜리 **x좌표**
* 그 뒤에 32바이트짜리 **y좌표**

를 그대로 이어붙입니다.

예시로,

```
K = 04F028892BAD...505BDB
```

이런 식으로 됩니다.
여기서 `04`가 접두어(prefix)이고, 뒤에는 x값과 y값이 16진수(hex)로 이어집니다.

### 🧮 길이 계산

* `04`: 1바이트 (2 hex digits)
* `x`: 32바이트 (64 hex digits)
* `y`: 32바이트 (64 hex digits)
  → 총 65바이트 = 130 hex digits
  즉, **520비트 공개키**로 표현됩니다.

---

## 🧮 4️⃣ Compressed Public Key (압축된 공개키)

압축된 공개키는 전체 y좌표를 저장하지 않고, **y좌표가 짝수인지 홀수인지**만 저장합니다.
그 이유는 타원곡선 방정식이 (y^2 = f(x)) 형태이므로, x만 알면 y는 ± 두 개의 값 중 하나로 정해지기 때문입니다.

이때 사용되는 형식은:

```
02 + x   (y가 짝수인 경우)
03 + x   (y가 홀수인 경우)
```

즉, 압축된 공개키는 **33바이트(66 hex digits)** 만 사용하므로,
저장 공간이 절반으로 줄어듭니다.

---

## 🧠 5️⃣ 예시로 정리하면

| 구분                        | 설명           | 예시                    |
| ------------------------- | ------------ | --------------------- |
| x좌표                       | 256비트 값      | F028892BAD...DC341A   |
| y좌표                       | 256비트 값      | 07CF33DA18...505BDB   |
| **Uncompressed key**      | `04 + x + y` | 04F028892BAD...505BDB |
| **Compressed key (짝수 y)** | `02 + x`     | 02F028892BAD...DC341A |
| **Compressed key (홀수 y)** | `03 + x`     | 03F028892BAD...DC341A |

---

## ✍️ 6️⃣ 요약 정리

* 공개키는 **타원곡선 위의 점 (x, y)** 로 표현된다.
* **Uncompressed 형식**은 `04` + `x` + `y` (총 130 hex digits).
* **Compressed 형식**은 `02` or `03` + `x` (총 66 hex digits).
* `04`, `02`, `03`은 각각의 형식을 구분하는 **prefix 바이트**이다.

---

## 📘 추가 예시

예를 들어 비트코인에서 어떤 공개키가 다음과 같다고 하자:

```
x = 79BE667EF9DCBBAC55A06295CE870B07
    029BFCDB2DCE28D959F2815B16F81798
y = 483ADA7726A3C4655DA4FBFC0E1108A8
    FD17B448A68554199C47D08FFB10D4B8
```

* Uncompressed 형식:

  ```
  0479BE667E...F81798483ADA77...0D4B8
  ```
* y가 짝수이므로 Compressed 형식은:

  ```
  0279BE667E...F81798
  ```

---



