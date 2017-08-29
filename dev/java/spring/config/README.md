# Spring 설정

## 환경설정

* context 파일 설정 [REF. 최하단 제타건담님 댓글](* [Spring transaction rollback 안되는 현상](https://okky.kr/article/281101)
	* root context와 servlet context로 context 영역을 나누어 관리한다.
		* root context
			* Service 단과 Repository 및 DB, 트랜잭션 등 Web 계층 다음 단계에서 진행하는 것
		* servlet-context
			* Web 계층에서만 진행하는 것들

## Exception

* [Spring transaction rollback 안되는 현상](https://okky.kr/article/281101)
* [Spring-MVC-Controller-클래스에-Transactional-사용](http://dimdim.tistory.com/entry/Spring-MVC-Controller-%ED%81%B4%EB%9E%98%EC%8A%A4%EC%97%90-Transactional-%EC%82%AC%EC%9A%A9%EC%97%90-%EB%8C%80%ED%95%9C-%EC%82%BD%EC%A7%88)
* [Transaction 트랜잭션 속성](http://springsource.tistory.com/136)
* [Spring Transaction](http://egloos.zum.com/springmvc/v/499291)
