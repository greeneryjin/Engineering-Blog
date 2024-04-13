JPA를 사용하면서 JPA의 기능을 제대로 사용하지 못한 부분를 발견했습니다. 객체를 수정할 때 DB에서 조회한 다음, 수정 코드를 적용하고 다시 저장을 했습니다. 

이렇게 할 필요가 전혀 없었습니다. 왜냐하면 JPA는 @Transactional 사용하면 영속성 컨텍스트에 있던 객체데이터와 방금 수정한 데이터를 비교해서 변경이 발생한 부분을 자동으로 Update를 해줍니다. 

수정 전 코드 

```java
//식물 사전 수정 전
    @Transactional
    public PlantDic updatePlant(Long id, PlantChangeDto plantChangeDto){
        PlantDic plantDic = findByPlantId(id);
        plantDic.changePlant(plantChangeDto);
	plantDicRepository.save(plantDic);
        return plantDic;
    }
```

수정 후 코드

```java
//식물 사전 수정 후
    @Transactional
    public PlantDic updatePlant(Long id, PlantChangeDto plantChangeDto){
        PlantDic plantDic = findByPlantId(id);
        plantDic.changePlant(plantChangeDto);
        return plantDic;
    }
```

저 코드가 가능한 이유는 JPA는 조회한 객체를 캐시에 보관하고 해당 객체를 스냅샷으로 저장해놓기 때문입니다. 수정이 발생하면 수정된 객체와 기존 스냅샷을 비교한 다음, 변경이 발생한 부분을 수정해서 DB에 쿼리를 날리게 됩니다.
