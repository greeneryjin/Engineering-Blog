
### NatvieQuery 

: NatvieQuery는 JPQL이 아닌 SQL를 직접 정의하여 사용하는 방식입니다. 

```java
// DB product , JPA Product 
@Query(value = "select * from product p inner join customer c on p.customer_id = c.customer_id " + "where c.customer_id = :customerId", nativeQuery = true)
	Long findByCustomerId(@Param("customerId") Long customerId);
	List<ProductDto> findAllByCustomerId(@Param("customerId") Long customerId);

```
**장점**

1. DB에 직접적으로 쿼리를 실행시켜 속도가 빠릅니다.
   
2. SQL 언어를 전체적으로 사용 가능합니다.
   
3. 여러 조인이나 복잡한 쿼리를 작성할 때 직관적이며 이해하기 쉽습니다.


**단점**

1. 특정 DB에 대해 쿼리를 작성하기에 종속성이 높아질 수 있고 DB가 수정되면 쿼리를 다시 만들어야 합니다. (MySQL에서는 LIMIT / Oracle에서는 ROWNUM 등)
   
2. 결과를 Object 리스트로 반환 하기에 JPQL의 TypedQuery보다 안정성 부분에서 미흡합니다.
   
3. 주의할 점으로 사용자에게 입력된 값을 매개변수로 바인딩 하거나 따로 유효성 검사가 필요할 수 있습니다.
---


### JPQL

: Jpql 엔티티로 DB 쿼리를 작성하는 방식입니다


```java

// JPA Product, Customer 
@Query(value = "select "
	+ "new edu.fisa.lab.customer.dto.ProductDto(p.productName, p.price, p.brand, p.size, p.category)"
	+ "from Product p join customer c "
	+ "where c.customerId = :customerId")
	List<ProductDto> findAllByCustomerId(@Param("customerId") Long customerId)
```

**장점**

1. 객체 지향적인 쿼리로 DB 테이블 조인이 아닌 java에서 엔티티 관계 및 상속 개념으로 봐야 합니다.
   
2. 반환 유형을 지정할 수 있습니다.
   
3. 특정 유형의 DB(MySQL, Oracle)에 연결된 것이 아니기에 데이터베이스 변경 시에도 유지, 보수 부분에서 유연하며 쿼리를 다시 작성할 필요가 없습니다.
   
4. 코드 실행 단계가 아닌 코드를 작성하는 시점에서 빠르게 오류를 발견할 수 있습니다.
   

**단점**

1. 모든 SQL 기능을 지원하지 않아 쿼리 작성에 유연하지 않습니다
   
2. 여러 조인이나 복잡한 쿼리를 작성해야 할 때 어렵습니다.
   
3. 매핑하여 사용하는 만큼 DB에 직접적으로 쿼리를 실행하는 NativeQuery보다 속도적인 부분에서 차이가 있습니다.



**JPQL를 사용한 이유**

JPA는 ORM을 사용하기 때문에 DB를 변경할 때도 유연하게 처리할 수 있고 컴파일 시점에서 에러를 찾을 수 있기 때문에 JPQL를 사용했습니다.

