**[1] 정상망 케이스**
```mermaid
sequenceDiagram
    participant FBS as FBS탐지 모듈
    participant VIMSI as Virtual IMSI<br/>생성 모듈
    participant UE as 단말(UE)
    participant eNB as 기지국(eNB)
    participant MME as MME
    participant HSS as HSS

    Note over FBS,UE: 기알려진 FBS 탐지 패턴 감지
    FBS->>UE: 1. FBS 의심 판정
    Note over UE: Auth 트리거 Service Request
    UE->>MME: 2. Service Request (eKSI: 0)
    MME->>UE: 3. Authentication Request
    UE->>MME: 4. Authentication Response
    Note over FBS,UE: 정상망 확정
    MME->>UE: 5. Service Accept
```

---

**[2] FBS 판단 - 무응답 케이스**
```mermaid
sequenceDiagram
    participant FBS as FBS탐지 모듈
    participant VIMSI as Virtual IMSI<br/>생성 모듈
    participant UE as 단말(UE)
    participant eNB as 기지국(eNB)
    participant MME as MME
    participant HSS as HSS

    Note over FBS,UE: 기알려진 FBS 탐지 패턴 감지
    FBS->>UE: 1. FBS 의심 판정
    Note over UE: Auth 트리거 Service Request
    UE->>MME: 2. Service Request (eKSI: 0)
    Note over MME,UE: 3. 무응답
    Note over FBS,UE: FBS 확정
    FBS->>eNB: 4. Temporary Cell Barring
```

---

**[3] FBS 판단 - Service Reject 케이스 (정상망)**
```mermaid
sequenceDiagram
    participant FBS as FBS탐지 모듈
    participant VIMSI as Virtual IMSI<br/>생성 모듈
    participant UE as 단말(UE)
    participant eNB as 기지국(eNB)
    participant MME as MME
    participant HSS as HSS

    Note over FBS,UE: 기알려진 FBS 탐지 패턴 감지
    FBS->>UE: 1. FBS 의심 판정
    Note over UE: Auth 트리거 Service Request
    UE->>MME: 2. Service Request (eKSI: 0)
    MME->>UE: 3. Service Reject<br/>(EMM Cause #9 또는 #10)
    Note over UE,eNB: Challenge Response
    Note over UE,eNB: Re-Attach with Virtual IMSI
    VIMSI->>UE: 4. Virtual IMSI 생성 및 전달
    UE->>MME: 5. Re-Attach Request<br/>(Virtual IMSI)
    MME->>HSS: 6. Authentication Information Request<br/>(Virtual IMSI)
    HSS->>MME: 7. Authentication Information Answer<br/>(IMSI 미등록 → 인증정보 없음)
    MME->>UE: 8. Attach Reject<br/>(EMM Cause #2 또는 #8)
    Note over FBS,UE: 정상망 확정
```

---

**[4] FBS 판단 - Service Reject 케이스 (FBS)**
```mermaid
sequenceDiagram
    participant FBS as FBS탐지 모듈
    participant VIMSI as Virtual IMSI<br/>생성 모듈
    participant UE as 단말(UE)
    participant eNB as 기지국(eNB)
    participant MME as MME
    participant HSS as HSS

    Note over FBS,UE: 기알려진 FBS 탐지 패턴 감지
    FBS->>UE: 1. FBS 의심 판정
    Note over UE: Auth 트리거 Service Request
    UE->>MME: 2. Service Request (eKSI: 0)
    MME->>UE: 3. Service Reject<br/>(EMM Cause #9 또는 #10)
    Note over UE,eNB: Challenge Response
    Note over UE,eNB: Re-Attach with Virtual IMSI
    VIMSI->>UE: 4. Virtual IMSI 생성 및 전달
    UE->>MME: 5. Re-Attach Request<br/>(Virtual IMSI)
    MME->>HSS: 6. Authentication Information Request<br/>(Virtual IMSI)
    HSS->>MME: 7. Authentication Information Answer<br/>(IMSI 미등록 → 인증정보 없음)
    MME->>UE: 8. Attach Reject<br/>(EMM Cause #2 또는 #8)
    Note over FBS,UE: FBS 확정
    FBS->>eNB: 9. Temporary Cell Barring
```

3번은 정상망 확정, 4번은 FBS 확정 후 Temporary Cell Barring으로 마무리했어요! 추가 수정 있으면 말씀해주세요 😊
