# 블록체인 중간

날짜: 2022년 10월 18일 → 2022년 10월 24일
태그: 중간

T801,802 오후 6시 학과홈페이지에서 자리 확인할 것!!

→ T802 자리는 지정석??

# 비트코인이란? (문제와 해결을 중심으로)

- 비스코인 시스템의 원리
- 탈중앙화 시스템
- 여러가지 문제들의 해결 방법

## 비트코인의 원리

비트코인은 프로토콜의 집합이다. 즉 규칙이 정해져 있는 통신시스템이다.

- 비트코인은 transaction 이라는 거래명부(돈의 흐름을 기록)로 이뤄진다.
    - transaction 이란?
        - “A가 B에게 1BTC 보냈음” 의 정보가 담긴 것
- transaction 들을 여러 개 모아서 block 안에 기록한다.
- 이래서 이름이 `블록체인` 인거임

## 의문점과 문제점

- 명부(transaction) 는 누가 관리합니까?
    - 누구나 열람할 수 있도록
        - 즉, account 만 있다면 누구나 열여보고 확인할 수 있다.. 완전 naked…
        - 그러나 권리에 따른 책임은 없는 시스템!! : 언제든지 들어오고 나갈 수 있음

- 계정(account) 는 누가, 어떻게 생성합니까?
    - public key 로 계정 생성
        - 생성된 public key 가 사용자의 아이디 및 adress 가 된다.
        - public key 이므로 누구나 개인의 adress 에 접근할 수 있음
    - 계정의 비밀번호는 `디지털 서명` 으로 작동
        - `디지털 서명` 을 만들기 위해서는 private key가 필요
        - 즉, public key(adress) 의 짝이 되는 private key 가 계정 생성 시 부여됩니다~~
    - 계정 생성에는 횟수 제한이 없음

- 거래의 순서 :  A→B→C vs B→C→A 처럼 양립할 수 없는 거래의 순서는 매우 중요하다.
    - 송출의 기록(transaction)을 여러개로 묶어서 블록 안에 기록
        - 블록 단위로 기록되므로 거래의 순서는 곧 블록의 순서가 될 것임
        - 즉, 다음과 같은 과정을 통해 누군가의 소비가 인정된다.
            - A는 transaction : A→B 를 채굴자에게 요구
            - transaction 이 유효한지 검사 (유효하면,)
            - 채굴자는 transaction 을 블록 안에 넣어줌
            - 블럭이 채굴되면
            - 다른 채굴자들에게 유효한지 시험받음
            - 최종적으로 의견일치가(6블럭 정도) 되면 거래가 인정됨
        - 6블럭 정도 이후에 B는 ‘안전하게’ A가 요청한 transaction 을 통해 새로운 transaction 을 요구할 수 있음

# 블록체인 암호학

## 암호학의 4대 목표

- Confidentiality(기밀성) : 데이터가 유출되지 않았는가(나에게만 보이는가)?
- Data integrity(무결성) : 데이터가 훼손되지 않았는가
- Authentication(신빙성) : 데이터가 원본인가? 진짜를 흉내낸 가짜가 아닌가?
- Non-repudiation(부인봉쇄) : 검증절차를 서로간에 거절할 수 없는가?

## 암호학 용어 및 가정

### 용어

- `cipher`, `cryptosystem` : `plaintext` 를 `encrypt` (암호화) 시키는데 사용되는 알고리즘
    - `plaintext` : 암호화의 대상. 수신자에게 유출되지 않고 전달되어야 하는 목표가 있음
    - `encrypt(plaintext,key)=ciphertext`
    - `ciphertext` : 암호화된 `plaintext`
- `ciphertext` 를 `decrypt` (복호화) 시켜 `plaintext` 추출해냄
    - `plaintext=decrypt(ciphertext,key)`
    - `private_key` : 수신자만이 가지고 있는 복호화의 열쇠(key)
- symmetric key system : `encrypt` 와 `decrypt` 하는데 유일한(하나의) 키가 사용되는 시스템
    
    ![Untitled](src/Untitled.png)
    
- asymmetric key system : `public_key` 로 `encrypt` , `decrypt` 시 `private_key` 사용됨

### 가정

- 모든 암호학 시스템은 공격자에게 알려져 있다.
    - 알려져도 상관없다는 말
    - 즉, 알려져도 안전한 시스템을 만드는 것이 목표

## `cipher` 기법

- random functions - `hash` functions
    - input : 길이의 제한이 없다 $0\sim \infty$ , output : 길이가 고정되어 있다
    - 입력의 길이엔 제한이 없으나 출력의 길이가 일정하므로 `충돌 crash` 이 발생할 수 있음
        - 충돌 : 서로 다른 input 에 대해서 output 이 똑같은 상황
- random generator - stream ciphers
    - input 의 길이가 짧고 output 의 길이가 길다
- random permutations - block ciphers
    - key 를 통해 input 을 output 으로 output 을 input 으로
    - input, output 모두 일정한 길이
- public key encryption
    - public key 를 통해 encryption 을 누구나 할 수 있음
    - 열어보는 것은 private key 소유자만 가능
- digital key signatures
    - private key 소유자만 사인 가능
    - 누구든지 public key 통해 사인의 유효성 검증 가능

## hash collision issue :  해시함수에서의 충돌 문제

![Untitled](src/Untitled%201.png)

- collision resistance : 같은 해시값을 출력하는 두 개의 입력값을 찾기 어렵다.
- preimage resistance : 특정 해시값을 만족시키는 입력값을 찾기 어렵다(POW).
- second-preimage resistance : 특정 입력값에 해당하는 해시값을 동일하게 가지는 다른 입력값을 찾기 어렵다.

모두 다 같은 말처럼 보이지만 공격자의 목표에 따라 각각 다른 의미를 지닌다.

### cryptographic hash function 에서 요구되는 것

- 압축성 compression  : output(해시값)의 길이가 짧아야 한다.
- 효율성 efficiency : 입력값으로 해시값을 얻어내는 데 계산이 쉬워야(빨라야)한다. $hash(x)\text{ is fast and easy}$
- 단방향 one-way : 해시값을 얻는 입력값을 찾기 어려워야 한다. $\text{hard to find } x\text{ that                   } h(x)=y$
- 충돌안정성 collision resistance : 충돌이 일어나는 것을 완전히 막을 수 없으나, 충돌이 발생하는 입력을 찾는 게 매우매우매우 어려워야 한다.

### hash function 의 안정성

- **Birthday Paradox**
    
    n 명이 있을 때, 어느 두명이 생일을 공유할 확률
    
    $$
    _nC_2\cdot \left(\frac{1}{365}\right)^2\left(\frac{364}{365}\right)^{n-2} =\frac{1}{2}
    $$
    
    위 식에서 $(364/365)^{n-2} \to 1$ 로 하면 다음과 같이 간단하게 $n$ 을 구할 수 있다.
    
    $$
    \frac{n(n-1)}{2}\left(\frac{1}{365}\right)^2=\frac{1}{2}\;\;\;\;\; take\;\;
    \frac{n(n-1)}{2}\approx \frac{n^2}{2}\\
    \therefore n=\sqrt{365}
    $$
    
    여기서 중요한 것은 $\sqrt{\;\;\;}$ 임을 알아두자.
    

위의 birthday paradox를 통해 hash function 을 기반으로 하는 보안 시스템의 취약점을 계산할 수 있다.

먼저 주어진 시스템의 collision resistance 가 매우 높다 가정하고, output 의 길이가 $N\,bit$ 라 하자.

output 의 가능한 경우의 수는 $2^N$ 개일 것이다.

그렇다면 birthday paradox 의 birthday 에 해당하는 것이 $2^N$ 이므로, 최초 collision 이 일어날 수 있는 합리적인(확률=1/2) 횟수는 $\sqrt{2^N}=2^{\frac{N}{2}}$이 된다.

즉, $2^{\frac{N}{2}}$연산을 하면 hash function 을 깰(break) 수 있다.

### hash function 의 사용

사이트에 로그인 하는 상황을 생각해보자.

![Untitled](src/Untitled%202.png)

- 로그인하기 위해서는 아이디와 패스워드가 필요하다.
- 기본적으로 사이트의 DB 에는 $\text{[id,password]}$ 의 쌍이 저장되는 것을 기대할 수 있다.