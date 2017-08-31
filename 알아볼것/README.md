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
	*

### mybatis를 이용한 mapper 작성시 insert, update, delete 구문의 위치

mybatis를 사용하는 경우 select의 경우에는 워낙 여러 테이블이 join 될 수 있으니 위치가 개발자의 판단에 따라 여러 곳일 수 있다고 생각한다. 하지만 insert, update, delete의 경우에는 하나의 테이블만 참여하니 테이블과 mapper가 1:1이 되어야 한다고 생각하는데 내가 잘못 생각하고 있는 것일까?


### Exception
