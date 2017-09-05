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



### AOP

실제 비즈니스 로직과 그 전에 행할 유효성 체크를 분리하기



## Javascript

### Callback

callback function에 대해 더 알아보기
* 변수의 범위

함수의 선언과 호출부에 대해 명확하게 구분해서 사용하기


## SQL

### JOIN

평소 `LEFT JOIN`을 쓸 때 `JOIN` 에는 `타 테이블과의 관계`만 쓰고 그 외 조건은 `WHERE` 에 적고 있었다. 출처는 기억나지 않지만 SQL을 처음 배울 때 그런 말을 들었었고, 그게 맞다고 생각이 되서 지금까지 그렇게 사용해오고 있었다.

그런데 오늘 이 맹신이 깨졌다. 평소라면 `WHERE` 에 적었을 쿼리 덕분에 시간을 한참 허비했다. 아직 확실하진 않다. `GROUP BY`, `COUNT` 함수와 같이 쓰면서 발생한 문제인지, 아니면 해당 문제의 경우 `JOIN` 에 거는 게 맞는 것인지. 좀 더 확인해봐야겠다.
