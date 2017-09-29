# 실제 업무하다 막히는 부분, 알아볼 부분들

## Java

### API 형식에서 리턴의 형식은 어떻게 해야하는가

하나의 API로 일단락 지었다고 생각한 메소드가 있어서 service에서 controller로 다음과 같이 리턴을 주었다.

```json
{
	"RESULT" : "SUCCESS",
	"RESULT_CODE" : "000",
	"DATA" : {
		"count" : 25,
		"list" : [
			{
				"name" : "john",
				"age" : 36
			},
			{
				"name" : "eric",
				"age" : 24
			}
		]
	}
}

```

문제는 그 후에 개발된 메소드에서 이 service를 호출하고 데이터를 조작할 일이 생겼다는 것이다.

```Java
pubilc String example() {
	Map<String, Object> resultMap = service.getPersonList();
	int age = ((Person) resultMap.get("DATA")).getAge();
}
```

잘 포장된 상태로 받아온 데이터를 굳이 헤집어서 데이터를 가져오는 방식이 영 못나보인다.

어떻게 해결하면 좋을까? 아래는 자문자답 형식으로 생각을 정리한 것이다. 아직 명쾌한 답을 얻진 못했다.

* 서비스에서 DATA만 받아오면 되지 않을까?
	* 정상적으로 DATA를 가져왔을 때는 문제 없을 거 같으나 실패 시에는 에러코드 값을 받아와야하는데 이 경우에는 어떻게 처리해야할지 모르겠음

### mybatis를 이용한 mapper 작성시 insert, update, delete 구문의 위치

mybatis를 사용하는 경우 select의 경우에는 워낙 여러 테이블이 join 될 수 있으니 위치가 개발자의 판단에 따라 여러 곳일 수 있다고 생각한다. 하지만 insert, update, delete의 경우에는 하나의 테이블만 참여하니 테이블과 mapper가 1:1이 되어야 한다고 생각하는데 내가 잘못 생각하고 있는 것일까?


### Exception & Transaction

Spring에서 @service의 메소드에 @transactional 걸어주고 root-context.xml 파일에 <tx:annotation-driven transaction-manager="transactionManager" proxy-target-class="true"/> 설정 등, 구글링한 내용들이 기존부터 다 설정되어 있는데 정작 transaction이 안된다.

DB handling에서 가장 중요한 요소 중 하나인데 내용 정리가 필요하다.


### Controller, Service, Dao 의 역할 구분

코딩을 하다보면 이 경계를 명확히 구분 짓기가 어렵다. 정리가 필요하다.
* service는 다른 service를 호출할 수 있는가?

	#### 170929 idea
	- controller

		mvc에서 controller는 (business) model과 view를 연결해주기 위한 중개자로서의 역할만을 해야 한다고 생각한다.

	- service

	- dao

	REF::[business의 복잡도에 따라 service layer가 복잡해지는 현상](https://slipp.net/questions/386)

	REF::[mvc 나누기](http://egloos.zum.com/zeous/v/2087967)

	> 일반적인 형태는 service-dao 는 1:1 , service 간 호출은 ok. 단, 이때 service 간 호출 rule 은 엄격하게 정의되어야 한다.

	> A->B 는 허용, B->A는 절대 불가.

	> 단순 CRUD 위주라면 괜찮지만 결제같이 로그도 남기고, 중간에 transaction 끊고 한다면 확실히 1 service, multi dao 는 어려울 듯. 이때 dao는 진짜 CRUD 수준, service 는 validation, process 등등 수행.. 상위 서비스는 여러 service 를 거느린다. (구매서비스가 사용자서비스-요청서비스-결제서비스-상품제공서비스 콜하는 그런 수순이 될 듯 하네요.)
근데 여기서도 고민되는게, 예를 들어 요청서비스에 대한 validation 로직 중 "사용자가 미성년자여서는 안된다" 라는 것이 있을 경우, 이를 어떻게 알까? 하는 게 궁금하네요. 요청서비스 call 할 때 사용자 정보를 넘기는 형태인가? 아니면 요청서비스가 또 사용자서비스를 call 하는 형태일까? 또, 상위 서비스와 하위 서비스는 어떻게 나눌 것인가? (이걸 진짜 잘 해야겠지요. 한번 엉클어지면 개판이 될 테니...)


### AOP

실제 비즈니스 로직과 그 전에 행할 유효성 체크를 분리하기



## Javascript

### Callback

callback function에 대해 더 알아보기
* 변수의 범위

함수의 선언과 호출부에 대해 명확하게 구분해서 사용하기

### 책임과 역할
데이터를 받아 팝업을 띄울 때, 팝업을 여는 것은 caller와 callee 중 누구의 책임인가
