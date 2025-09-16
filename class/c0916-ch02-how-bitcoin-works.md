
# 📝 GPT가 수정/보완해준 강의 필기

### 1. Transactions, Blocks, Mining, and the Blockchain

- User는 모바일 **wallet** 앱을 통해 트랜잭션을 생성한다. 이때 **받는 사람의 주소**와 **보낼 금액**이 필요하다.
    
    👉 가장 중요한 건 wallet에 **개인 키(Private Key)** 가 있다는 점! 개인 키가 유출되면 자산이 전부 털린다.
    
- 트랜잭션은 wallet이 생성하며, 서명을 위해 개인 키를 사용한다.
- **주소(Address)** 는 개인 키 → 공개 키(Public Key) → 해시 과정을 거쳐 생성된다.
    
    👉 `1` 또는 `3`으로 시작하는 주소는 **주소 유형**을 의미하는 것이지 “개인키를 공개키로 바꿨다”는 뜻은 아니다. (❌ 수정 필요)
    
- **From**: 이전 트랜잭션의 Output = **UTXO(Unspent Transaction Output)**
- **To**: 수신자의 주소
- **서명(Signature)**: 개인 키로 만든 전자 서명 (개인 키 자체를 보내는 게 아니라, 서명으로 신원을 증명한다)
- 트랜잭션을 만드는 이유: **교환(exchange)**, **상품/서비스 결제(merchant payment)**
- 아직 블록에 포함되지 않은 트랜잭션(Mempool에 있는 것)을 **Miner**가 모아서 새 블록을 만든다.
- 블록 헤더에는 **이전 블록의 해시, 현재 블록의 트랜잭션 Merkle Root, nonce, timestamp 등**이 포함된다.
- Miner들은 **Proof-of-Work(PoW) 퍼즐(해시 난이도 조건 맞추기)**을 푼다. 가장 먼저 정답을 찾은 블록이 네트워크에 전파된다.
- 하지만 해당 블록이 "진짜"로 블록체인에 포함될지는 추후 더 긴 체인이 이어지면서 확정된다(즉, 여러 블록이 추가되며 **확정성(finality)** 이 보장된다).
- 예시:
    
    조로부터 환전한 돈이 UTXO가 되고, 앨리스가 커피를 사면 그 UTXO는 사용(spent)되어 사라진다. 대신 잔돈(change)이 새로운 UTXO로 생겨 앨리스의 지갑에 기록된다.
    

---

### 2. Bitcoin Transactions

- `inputs` = 사용자가 가진 UTXO들의 집합
    - input이 여러 개 → 잔돈(UTXO)이 여러 개 있어서 묶어서 쓰는 상황
- `outputs` = 비트코인이 보내질 주소들
- **총 입력 금액 ≥ 총 출력 금액** 이어야 한다.
    - 차이 = **Transaction Fee** (채굴자가 가져감)
    - 총 입력 < 총 출력이면 **유효하지 않은 트랜잭션** → 네트워크에서 거부됨
    - 총 입력 = 총 출력이면 수수료가 0이므로, 이론적으로 유효하지만 **채굴자들이 잘 안 담을 수 있다** (수익이 없으니까) → 따라서 블록에 늦게 포함될 수 있음
- 만약 트랜잭션이 블록에 오래 포함되지 않으면, 사용자가 사용하려 했던 input(UTXO)이 여전히 "unspent" 상태이므로, **Double Spending 위험**이 생길 수 있다.
- **Output #0**에서 #0은 인덱스 번호
    - 다음 트랜잭션이 이전 트랜잭션의 특정 UTXO를 참조하려면 `(이전 트랜잭션 ID + output 인덱스)`를 지정해야 한다
    - 이 output에 접근하려면 해당 UTXO의 **개인 키에 대응하는 서명**이 필요하다
- 앨리스가 이전 거래의 output(UTXO)을 밥의 카페에서 사용하면, 그 UTXO는 "spent" 상태로 바뀌고 새로운 UTXO(밥의 잔고, 혹은 앨리스의 잔돈)가 생긴다.
- 이런 방식으로 **트랜잭션 체인(Transaction Chains)** 이 이어진다.
- Wallet은 사용자가 내야 할 금액보다 **큰 UTXO**를 선택해서 사용한다. (필요시 change를 돌려준다)
- Input이 많아지면 트랜잭션 데이터 크기가 커져서 **수수료(fee)** 가 늘어난다.
- Input은 하나지만 Output이 여러 개인 경우도 가능하다 (예: 상대방 + 내 잔돈).

---


# 강의 필기 원본

## 1. Transactions, Blocks, Mining, and the Blockchain

- User는 가장 먼저 모바일 wallet에 들어감. 트랜젝션 생성을 위해 “받는 사람의 주소”와 “돈”이 필요하다. 그 중 가장 중요한 것은 wallet에 프라이빗 키가 있다는 것! 다른 사람한테 개인 키를 보여주면 내 돈이 다 털리는 것..!
- 트랜젝션은 wallet에서 만드는 것이다.
- 1로 시작하니까 개인키를 공개키로 바꿨다는 것을 알 수 있다.

- From: previous block/transaction output = UTXO
- To: 받는 사람의 주소
- signed by “key(=프라이빗 키)”: 나의 개인키를 갖고 서명을 만드는 것!

- transaction을 왜 만드냐? → exchange를 위해! or Merchant(=실물 경제나 서비스를 위해 pay하는 것!)를 위해!

- 아직 블록체인에 들어가지 않은 transaction을 miner가 끌어모아서 블록을 만든다.
- 블럭헤더는 해쉬를 갖고 있고, transaction들을 갖고 miner들이 퀴즈를 풀어낸 후 가장 먼저 푼 사람이 정답을 공유한다.
- 하지만 이 블록이 진짜 정답인지는 추후에 확인한다.

- 조로부터 환전한 돈이 UTXO가 됨.
- 커피를 구매하면서 조로부터 환전 받은 돈은 더 이상 UTXO가 아니게 되고, 밥으로 거래하면서 생긴 잔돈이 새로운 UTXO가 됨.

## 2. Bitcoin Transactions

- inputs에 input1, input2, …, inputN은 사용자가 가진 UTXO들임.
- input이 여러 개라는 것은 내 주머지에 잔돈이 많다는 것!
- output들에는 각각의 destination bitcoin address들이 있어야 함.

- total inputs > total outputs이어야 한다. 왜냐하면 transaction fee때문에
- total inputs < total outputs라면 노드가 검증해주지 않음 = 잘못된 트랜젝션이라 무시해버리며 다른 노드들에게 전파되지 않음.
- total inputs = total outputs라면 검증은 해주긴 함. 하지만, miner들이 fee가 0이라 블록에 넣을 transaction 우선순위를 뒤로 한다. 카페 사장인 밥이 이 트랜젝션을 승인해버리면 miner들이 이 트랜젝션을 블록에 안 넣어줘서 밥의 UTXO에 해당 트랜젝션이 엄청 늦게 들어올 수도 있음(miner들이 기피할 테니까)
- 추가적으로 트랜젝션이 블록에 들어가는 것이 늦어지면 앨리스가 해당 트랜젝션에 사용한 input들을 아직 unspent 상태이기때문에 double spend problem이 발생할 수 있음.

- output #0에서 #0은 인덱스임!
- 나중에 다른 transaction을 만들 때, 이전 transaction의 address와 해당 UTXO의 인덱스를 통해 연결함.
- 추가적으로 이전 transaction에서 unspent처리 되어있는 line에 접근하는 것은 해당 UTXO 소유자의 개인 키로만 접근할 수 있다.
- 앨리스가 이전 거래의 output에 있는 unspent line의 UTXO를 밥의 카페에서 커피를 구매하는데 사용하면, 이전 거래의 line이 spent로 바뀐다.
- 이런식으로 transaction chains가 생긴다.

- wallet은 자기가 써야할 돈보다 큰 UTXO를 찾는다.
- 사용하는 input이 많아지면 transaction 길이(transaction message 길이)가 길어진다. 즉, transaction fee가 늘어난다.
- 반면에, input은 하나인데, output이 긴 transaction도 존재할 수 있다.

