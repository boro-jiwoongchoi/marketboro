---
title: Naming Convention 정리
author: hayun.lee
layout: post
---


# Naming Convention 정리
식별자의 이름을 지을 때 지켜야 할 규칙에 대해 정리하려고 합니다.

## 카멜 표기법(Camel Case : 낙타 표기법)
두단어 이상의 변수명을 표현할 때, 두번째 단어부터 첫 글자를 대문자로 표기하는 방법
 
`marketBoro`, `camelCase`, `myCompany` 

## Package
lowercase 사용하며, 표준패턴을 따름

`com.marketboro.gred.admin.goods`

 
## Class(UpperCamel Case)
대문자로 시작하고, 명사사용
  ```java
public class GoodsController {}
public class StudentService {}
public class BookRepository {}
public class GoodsModel {}
public class GlobalExceptionHandler {}
  ```

## Method
* 입력
``` java
public void insertGoods(dto)
public void insertStudentAgeAndName(Integer age, String name)
```

* 조회(속성에 접근하는 메소드명의 접두사는 'get', 'set'을 사용)
  * controller, service 에서 사용
    ```java
    public List<GoodsModel> getGoodsList()
    public List<GoodModel> getGoodsList(Integer goodsId, Pageable pageable)
    public List<StudentModel> getStudentList()
    public Book getBookName()
    ```
  * repository 조회시 사용
    ```java
        public List<GoodsModel> findGoods()
        public List<GoodsModel> findGoods(Integer goodsId, Pageable pageable)
        public List<StudentModel> findStudent()
        public Book findOneBookName()
       ```
  * repository 수정
    ```java
       public void updateGoodsCategoryName(Integer goodsId, String name)
       public void updateStudentName(Integer goodsId, name)
       public void updateStudentAge(Integer goodsId, Integer age)
    ```
  * repository 삭제
    ```java
       public void deleteByGoodsId(Integer goodsId)
       public void deleteByName(String name)
    ```

## Variable
```java
private Integer studentId;
private String studentName;
private Integer count;
```

## Comment
```java
**
* Created by hayun.lee@marketboro.com on 2019-11-15.
*/
```
![image](https://user-images.githubusercontent.com/57780013/69030161-1bcbf400-0a1a-11ea-9b11-83394eb64d6e.png)

## Java
* JOOQ 테이블에 as(Alias) 사용은 경우에 따라 사용
```java
GoodsCategory goodsCategory = GoodsCategory.GOODS_CATEGORY.as("goodsCategory");
```
* JOOQ 테이블 select시, 전체컬럼 조회가 아닌 필요한 컬럼만 조회
```` java
dSLContext
.select(goodsCategory.goodsId, goodsCategory.name)
.from(goodsCategory)
````

## API 개발 프로세스
* api 개발시 프로세스 구조는 기본적으로 controller -> service -> repository 로 합니다.
  * Controller
    * 클라이언트와 통신시, 필요한 서비스를 호출한후 리턴하는 역할을 수행합니다.
  ```java
  @Restcontroller
  @RequiredArgsConstructor
  public class GoodsController {

    private final GoodsService goodsService;

    @GetMapping("/goods")
    public GoodsModel getGoods(SearchModel searchModel) {
      return goodsService.getGoods(searchModel);
    }

  }
  ```
  * Service  
    * 비지니스 로직을 수행합니다.
  1. 하나의 서비스 및 레포지토리를 호출할 경우의 클래스명 입니다.
  ```java
  @Service
  @RequiredArgsConstructor
  public class GoodsService {

    private final GoodsRepository goodsRepository;

    public GoodsModel getGoods(SearchModel searchModel) {
      return goodsRepository.findGoods(searchModel.getCategoryId, searchModel.getCategoryName);
    }

  }
  ```
   2. 여러 서비스 및 레포지토리를 호출할 경우에는 Colla(협업 어플리케이션)라는 네이밍을 붙여 클래스명을 짓습니다.
    ```java
  @Service
  @RequiredArgsConstructor
  public class GoodsCollaService {

    private final CodeService codeService;
    private final GoodsRepository goodsRepository;
    private final UserRepository userRepository;

    public GoodsModel getGoods(SearchModel searchModel) {
      GoodsModel goodsModel = goodsRepository.findGoods(searchModel.getCategoryId(), searchModel.getCategoryName);
      Integer code = codeSerivce.findCode(searchModel.getCode());
      String name = userRepository.findUsername(searchModel.getName());

      goodsModel.setCode(code);
      goodsModel.setName(name);
      return goodsModel;
      }

  }  
  ```
  * Repository
    * DB와의 통신하는 로직을 수행합니다.
  ```java
  @Repository
  @RequiredArgsConstructor
  public class GoodsRepository {
    private final DSLContext dslContext;

    public GoodsModel findGoods(Integer id, String categoryName) {
      return dslContext
      .select(GOODS.ID, GOODS.CATEGORY_NAME)
      .from(GOODS)
      .fetchOneInto(GoodsModel.class);
    }
  }
  ```