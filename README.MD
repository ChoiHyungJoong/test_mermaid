## ERD

**mermaid**
[https://mermaid.js.org/](https://mermaid.js.org/)
JS 기반의 다이어그램 및 차트 툴

**특징**
- md문서에 mermaid 문법에 맞게 텍스트를 작성하면, 그에 맞는 다이어그램 이미지를 출력한다.
- md를 지원하는 플랫폼은 별도의 플러그인을 설치할 필요가 없다.
    - github, notion의 경우 md 문서의 codespace를 통해 바로 적용이 가능하다.

**장점**
- 텍스트만 바꾸면 바로 반영이 가능하므로, 문서 최신화가 용이하다.
- 직관적인 문법으로 구성이 되어, 구상과 동시에 설계가 가능할 수 있다.

**단점**
- 각 구성요소간의 연결선들을 직접 조작할 수 없기 때문에, 일부 다이어그램은 구성이 복잡해질수록 가독성이 떨어질 수 있다.
- 일부 다이어그램은 비개발직군에게는 오히려 기존방식보다 진입장벽이 높을 수 있다.
- mermaid를 지원하지 않는 플랫폼에서는 오히려 사용이 불편할 수 있다.
## ERD
```mermaid
erDiagram
	Product {
		Long id PK "상품 식별자"
		String code "상품번호"
		String name "상품명"
		BigDecimal price "판매가격"
		int stock "재고수량"
	}
	Order {
		Long id PK "주문 식별자"
		BigDecimal totalAmount "주문 총 구매 가격"
		BigDecimal deliveryFee "주문 배송비"
		String status "주문 상태(ORDERED, PAYMENT_COMPLETED, DELIVERED, CANCELED)"
	}
	OrderProduct {
		Long id PK "주문 상품 식별자"
		Long orderId FK "주문 외래키"
		Long productId FK "상품 외래키"
		int quantity "주문 수량"
		BigDecimal totalPrice "주문 가격"
	}
	Payment {
		Long id PK "결제 식별자"
		Long orderId FK "주문 외래키"
		BigDecimal amount "결제 금액"
		String status "결제 상태('PAYMENT_COMPLETED', 'PAYMENT_FAILED')"
	}
	Order ||--|{ OrderProduct : ""
	Product ||--|{ OrderProduct : ""
	Payment ||--|| Order : ""
```

## SequenceDiagram
**주문 생성 api**
```mermaid
sequenceDiagram
	participant C as Console
	participant S as ApiServer
	participant D as Database
	C->>S : api 요청(POST /orders)
	S->>S : 요청 바디 구성(주문상품 번호 목록, 각 주문상품 구매 수량)
    S->>D : 상품번호로 상품 조회 쿼리 요청
    D-->>S : 상품 조회 쿼리 결과 수신
	loop 상품 목록 순회
		opt 상품 구매 수량보다 재고가 적을 경우
			S-->>C : 재고 부족으로 인한 주문생성 실패 응답 수신
		end
		S->>D : 상품 테이블에 주문상품 구매 수량 재고 차감 쿼리 요청
		D->>D : 상품 테이블 재고 차감 쿼리 적용
	end
	S->>S : 총 주문금액 계산
	opt 주문 금액이 5만원 미만일 경우
		S->>S : 주문금액 + 배송비(2500원)
	end
	S->>S : 주문과 주문상품 연관관계 형성
	S->>D : 주문 및 주문상품 목록 생성 쿼리 요청
	D->>D : 주문 및 주문상품 목록 생성 쿼리 적용
	D-->>S : 주문 생성 결과 수신
	S-->>C : 주문상품 목록 응답값 수신(주문ID, 주문상품 목록, 총 주문금액, 배송비)
```

## Git
```mermaid
gitGraph
    commit id: "init project"
    branch dev
    checkout dev
    commit
    branch feature
    checkout feature
    commit
    commit
    checkout dev
    merge feature
    branch realease
    commit
    checkout realease
    branch bugfix
    commit
    commit
    checkout realease
    merge bugfix
    commit id: "back merge"
    checkout dev
    cherry-pick id: "back merge"
    checkout main
    merge realease tag: "v0.38"
    branch hotfix
    checkout hotfix
    commit
    commit
    checkout main
    merge hotfix tag: "v0.3.8.1"
```
