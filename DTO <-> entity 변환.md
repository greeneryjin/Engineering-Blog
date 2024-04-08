Entity와 DTO를 변환할 때, 두 가지 방법을 사용했습니다.

1. DTO에 @Builder를 사용해서 변환

2. ModelMapper()를 사용해서 변환

---

1. DTO에 @Builder를 사용해서 변환 방식

<br>

 ## entity <- DTO

```java
//dto
public Product toEntity(ProductDto productDto) {
		return Product.builder()
				.productName(productDto.getProductName())
				.price(Integer.valueOf(productDto.getPrice()))
				.brand(productDto.getBrand())
				.size(Integer.valueOf(productDto.getSize()))
				.category(Category.top)
				.build();	
	}

@Transactional
public ProductDto productOne(Long productId) {
		Optional<Product> one = productRepository.findById(productId);
		ProductDto dto = new ProductDto().toDto(one.get());
	return dto;
}
```

## entity -> DTO

```java
//dto
public ProductDto toDto(Product product) {
		return ProductDto.builder()
				.productName(product.getProductName())
				.price(product.getPrice())
				.brand(product.getBrand())
				.size(product.getSize())
				.category(product.getCategory())
				.build();
	}

@Transactional
public List<ProductDto> productAll() {
		List<Product> pro = productRepository.findAll();
		List<ProductDto> pList = pro.stream().map(p -> new ProductDto().toDto(p)).toList();
		return pList;
}
```

dto 클래스에 두 설정을 넣어서 변경하는 방식입니다.

<br>

---

<br>

2. ModelMapper()를 사용해서 변환

<br>

## entity -> DTO

```java
private ModelMapper mapper = new ModelMapper();

	public BoardDTO getBoardById (Long id){
		Optional<BoardDTO> board = queryBoardRepository.queryFindBoardById(id);
		return board.map(b -> mapper.map(b, BoardDTO.class)).orElse(null);
	}

  // 컬렉션 조회
	public BoardDTO getBoardById (Long id){
		Optional<BoardDTO> board = queryBoardRepository.queryFindBoardById(id);
		return board.map(b -> mapper.map(b, BoardDTO.class)).orElse(null);
	}

```
이렇게 사용하면 됩니다. 
