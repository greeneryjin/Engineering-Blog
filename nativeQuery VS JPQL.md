
nativeQuery 

: 

```java
// DB product , JPA Product 
@Query(value = "select * from product p inner join customer c on p.customer_id = c.customer_id "
			+ "where c.customer_id = :customerId", nativeQuery = true)
	Long findByCustomerId(@Param("customerId") Long customerId);
	List<ProductDto> findAllByCustomerId(@Param("customerId") Long customerId);

```
