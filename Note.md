# StudyDID

### 1. Terminology(용어)

(1) DID
-  분산 식별자(Decentralized Identifier). 생태계의 모든 주체(entity)들은 자신의 DID를 하나 이상 가진다


(2) DID Document
- 블록체인에 등록되어 있는 사용자 DID 관련 정보를 담고 있는 문서. 주로 blockchain에 저장된다


(3) Entity
- 생태계에서 하나 이상의 역할을 수행하는 사람, 조직 혹은 장치를 일컫는 말로, 여기서는 Issure, Verifier, Holder 등의 구성 요소를 표현하는데 쓰인다


(4) Issure
- 사용자 요청에 의해 사용자 정보를 인증하고 Credential을 발급하는 발급기관


(5) Verifier
- 서비스의 이용을 위해서 사용자로부터 전달받은 Presentation을 검증하고 서비스를 제공하는 서비스 제공자


(6) Holder
- 플랫폼에서 서비스를 이용하는 사용자. 이슈어로부터 Credential을 발급 받고 Verifier의 서비스를 이용하기 위해 Presentation을 만들어서 제출하는 사용자


(7) Blockchain
- 사용자의 DID 관련 정보가 저장되는 탈중앙화된 저장소


(8) Credential
- Issuer로부터 인증받고 발급받은 증명내역. 증명받은 실제 Claim Data와 이 Claim Data를 증명해줄 수 있는 Verifiable Credential로 구성된다
- 보통 Credential과 Verifiable Credential은 동일한 경우가 많다.


(9) Verifiable Credential
- Issuer로부터 발급받은 증명서로 위변조를 검증할 수 있다.
- 단, Credential 내에 Claim Data가 실제로 존재하지 않고, Claim Data를 검증할 수 있는 정보만 포함되어 있다.


(10) VC
- Verifiable Credential을 줄여서 VC로 표현하기도 한다


(11) Verifiable Presentation (Presentation)
- 서비스 이용을 위해, Verifier가 요구한 사용자 정보에 대하여 사용자(Holder)가 Credential을 포함한 후 사용자가 서명한 제출 데이터.
- 위변조 검증이 가능하다는 점에서 Verifiable Presentation이라고 불리며, 이는 Presentation과 기술적으로 동일하다.
- Credential과 Verifiable Credential은 다르지만, Verifiable Presentation과 Presentation은 동일하다.


(12) VP
- Verifiable Presentation을 줄여서 VP로 표현하기도 한다


(13) VCP (Message)
- Verifiable Credential, Verifiable Presentation의 약자로 VC를 발급하고 VP를 제출할 때 사용하는 메세지 프로토콜


(14) Claim
- Issuer로부터 인증받고 Credential로 발급 받은 사용자 정보


(15) JWT (JSON Web Token)
- Credential, Presentation 등의 발급에 사용되는 기본 포맷


(16) JWS (JSON Web Signature)
- JWT는 header, payload, signature로 구성되는데, JWT 내에 포함되는 서명(signature).


(17) JWE (JSON Web Encryption)
- Credential, Presentation의 데이터를 보호하기 위해 서버와 클라이언트간 암호화한 데이터 포맷.




### 2. DID Ecosystem

(1) W3C DID Ecosystem
- Issuer (issues)
    - Issue Credentials (to Holder)
    - Verify Identifiers (to Blockchain)
    - Issuer는 Credential을 발급합니다


- Holder (Acuires, Stores, Presents)
    - Send Presentation (to Verifier)
    - Register Identifiers (to Blockchain)
    - Holder는 Credential 발급 요청을 하고, Issuer로부터 받은 Credential을 저장합니다.
    - Verifier의 요청이 있을 때 Credential에 서명을 한 Presentation을 만들어 제출합니다.


- Verifier (Requests, Verifies)
    - Verify Identifiers (to Blockchain)
    - Verifier는 Holder로부터 Presentation을 제출 받아서 이를 검증하고 Credential의 내용에 따라 서비스를 제공합니다.


- Blockchain (Verifiable Data Registry)
    - Maintain Identifiers
    - Blockchain은 탈중앙화된 분산 ID인 DID를 저장합니다


(2) Credential, Presentation Message Exchange Concept

### - Process
    - 1. DID init (From Issuer to Holder)
        - Credential을 발급하는 서버는 자신의 DID를 포함한 DID init message를 Holder에게 전달합니다.
        - 이때 메세지 암호화를 위한 Issuer의 암호화키를 같이 전달합니다.
        - Holder는 Issuer의 DID를 blockchain의 DID score를 통해 검증하고 신뢰할 수 있는 Issuer인지 판단합니다.


    - 2. DID auth (From Holder to Issuer)
        - Holder는 DID init을 수락하고 Holder의 암호화 키를 전달하기 위해 DID auth message를 Issuer에게 전달합니다.
        - 이때 DID auth message는 Issuer의 암호화키와 Holder의 암호화키로 암호화해서 전달합니다.


    - 3. Create Credential (From Issuer)
        - Issuer는 DID auth message를 복호화하고, Holder의 DID를 검증합니다.
        - Issuer는 Holder에게 발급할 Credential을 만듭니다.


    - 4. Register Credential (From Issuer to Blockchain)
        - Issuer는 생성한 Credential message를 Blockchain에 등록합니다.


    - 5. Credential (From Issuer to Holder)
        - Issuer는 Credential을 암호화해서 Holder에게 전달합니다.


    - 6. Result (From Holder to Issuer)
        - Holder는 Credential message를 복호화하고 검증합니다.
        - 문제가 없으면 저장을 하고 그 결과를 Issuer에게 전달합니다.


    - 7. Request Presentation (From Verifier to Holder)
        - Verifier는 Credential을 Holder로부터 제출 받기 위해 request presentation message를 만들어서 Holder에게 전달합니다.
        - 이때 메세지 암호화를 위해 Verifier의 암호화키를 같이 전달합니다.
        - Holder는 Verifier의 DID가 신뢰할 수 있는 Entity인지 확인하고 DID의 주인이 보낸 Message가 맞는지 확인하기 위해 Blockchain을 이용해서 Request presentation message의 서명을 검증합니다.


    - 8. Create Presentation (From Holder)
        - Holder는 request presentation message를 분석해서 verifier가 어떤 credential과 어떤 claim을 요청하는지 파악합니다.
        - 요청 사항에 맞는 credential이 존재하고, 사용자가 제출을 승인하면 credential을 presentation message로 만듭니다.
        - 그리고 이것을 암호화합니다.


    - 9. Presentation (From Holder to Verifier)
        - Holder는 암호화된 Presentation message를 Verifier에게 제출합니다.


    - 10. Verify Presentation (From Verifier)
        - Verifier는 Presentation message를 복호화해서 presentation에 담긴 credential을 추출합니다.
        - credential에서 verifier가 필요로 하는 claim 정보를 추출할 수 있습니다.
        - 이때 verifier는 presentation message의 유효성 검증을 아래와 같이 수행합니다.
            - Issuer의 DID 검증: Credential을 발급한 Issuer를 신뢰할 수 있는지 검증합니다.
            - Credential의 발급대상 검증: Credential이 발급된 대상과 Presentation을 제출한 대상이 동일한지 검증합니다.
            - Credential의 서명 검증: Issuer가 발급한 Credential이 맞는지 검증합니다.
            - Presentation의 서명 검증: Holder가 제출한 Presentation이 맞는지 검증합니다.
