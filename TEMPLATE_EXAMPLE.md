# Code Comprehension Training — 학습 기록 예시

## 기본 정보

| 항목 | 내용 |
|---|---|
| 날짜 | 2026-11-19 |
| 분석 대상 | 재고 차감 로직 (`decreaseStock`) |
| 언어 | Java |
| 코드 분량 | 14줄 |
| 소요 시간 | 15분 |

## 분석 대상 코드

``` java
public void decreaseStock(Long productId, int quantity) {
    if (quantity <= 0) {
        throw new IllegalArgumentException();
    }

    Product product = productRepository.findById(productId)
            .orElseThrow(ProductNotFoundException::new);

    if (product.getStock() < quantity) {
        throw new OutOfStockException();
    }

    product.decreaseStock(quantity);
}
```

## 1. 코드 구조 파악

**전체 구조**
- 조건문: 2개
- 반복문: 없음
- 메서드 호출: `findById()`, `orElseThrow()`, `getStock()`, `decreaseStock()`
- 중첩 깊이: 조건문 내부 1단계
- 기타 특징: 예외를 통한 조기 종료, 평탄한 코드 구조

**코드의 기능 요약**

상품의 재고를 차감하는 메서드다. 요청 수량을 검증한 후 상품을 조회하고, 재고가 충분한지 확인한 다음 재고를 차감한다.

## 2. 표식(Beacon) 탐색

| 표식 | 추론한 의미 |
|---|---|
| `decreaseStock` | 재고 차감 기능 |
| `quantity <= 0` | 요청 수량 유효성 검사 |
| `findById` | 상품 ID를 이용한 조회 |
| `ProductNotFoundException` | 상품이 존재하지 않는 상황 |
| `OutOfStockException` | 재고가 부족한 상황 |
| `product.decreaseStock()` | 실제 재고 차감 처리 |

**표식을 통해 추론한 전체 기능**

상품의 존재 여부와 재고 수량을 확인한 후, 요청한 수량만큼 재고를 차감하는 기능이다.

## 3. 청킹(Chunking)

| 청크 | 역할 | 주요 동작 |
|---|---|---|
| 1 | 입력값 검증 | 수량이 0 이하이면 예외 발생 |
| 2 | 상품 조회 | 상품 ID로 조회하고 존재하지 않으면 예외 발생 |
| 3 | 재고 검증 | 요청 수량보다 재고가 적으면 예외 발생 |
| 4 | 재고 차감 | 상품 객체의 재고 감소 메서드 호출 |

**전체 실행 흐름**

`입력값 검증 → 상품 조회 → 재고 검증 → 재고 차감`

## 4. 코드 재현

원본 코드를 보지 않고 기억한 구조와 실행 흐름을 바탕으로 다시 작성한다.

**재현 코드 위치**

`Solution.java`

**재현 코드**

``` java
public void decreaseStock(Long productId, int quantity) {
    if (quantity <= 0) {
        throw new IllegalArgumentException();
    }

    Product product = productRepository.findById(productId).get();

    if (product.getStock() < quantity) {
        throw new OutOfStockException();
    }

    product.decreaseStock(quantity);
}
```

**재현 결과**

- **쉽게 기억한 부분:** 입력값 검증, 재고 부족 검사, 재고 차감 순서
- **기억하지 못한 부분:** `orElseThrow()`를 이용한 예외 처리 방식
- **원본과 다르게 작성한 부분:** `orElseThrow()` 대신 `get()`을 사용함

## 5. 회고 및 원인 분석

| 구분 | 부족하거나 이해하지 못한 내용 |
|---|---|
| 문법 | 특이사항 없음 |
| 라이브러리/API | `Optional`의 `orElseThrow()` 사용 방법 |
| 알고리즘 | 특이사항 없음 |
| 도메인 지식 | 특이사항 없음 |
| 코드 구조 | 특이사항 없음 |

**기억하지 못한 원인**

`findById()`의 반환 타입이 `Optional<Product>`라는 사실은 알고 있었지만, 값이 없을 때 원하는 예외를 발생시키는 `orElseThrow()` 사용 방법이 익숙하지 않았다.

**잘못 추론한 부분**

`Optional.get()`을 사용해도 상품 객체를 가져올 수 있으므로 동일하게 동작한다고 생각했다.

그러나 조회 결과가 비어 있을 때 `get()`은 `NoSuchElementException`을 발생시키고, 원본의 `orElseThrow(ProductNotFoundException::new)`는 `ProductNotFoundException`을 발생시킨다.

따라서 두 코드는 정상적인 조회 상황에서는 동일하게 동작하지만, 상품이 존재하지 않는 경우에는 예외 동작이 다르다.

## 6. 보완 학습

**새롭게 학습한 내용**

- `Optional<T>`는 값이 존재할 수도 있고 존재하지 않을 수도 있음을 표현하는 컨테이너다.
- `get()`은 값이 없으면 `NoSuchElementException`을 발생시킨다.
- `orElseThrow()`는 값이 없을 때 지정한 예외를 발생시킬 수 있다.
- Repository 조회 시 도메인에 맞는 예외를 발생시키기 위해 `orElseThrow()`를 사용할 수 있다.

**추가로 확인할 내용**

- `Optional.orElse()`, `orElseGet()`, `orElseThrow()`의 차이
- `Optional`을 반환하는 Repository 메서드의 예외 처리 방식

## 7. 최종 회고

**코드의 기능을 설명할 수 있는가?**

- [x] 전체 실행 흐름을 설명할 수 있다.
- [x] 주요 표식의 의미를 이해했다.
- [x] 코드를 기능 단위로 나눌 수 있다.
- [x] 주요 로직을 원본 없이 재현할 수 있다.
- [x] 이해하지 못한 부분의 원인을 파악했다.

**한 줄 회고**

전체적인 재고 차감 로직은 쉽게 이해하고 재현할 수 있었지만, `Optional`의 예외 처리 API에 대한 이해가 부족하다는 점을 발견했다.

**다음 훈련에서 개선할 점**

- 코드에 사용된 라이브러리 API의 반환 타입과 예외 동작을 함께 파악한다.
- 단순히 정상적인 실행 흐름뿐 아니라 예외 상황에서의 동작도 이해하도록 한다.