# Log4j

## spring-log4j 설정

웹개발을 하면서 DB 연동은 기본 중의 기본 사항이고, 개발 중 현재 실행 쿼리를 보는 것 역시 마찬가지다. 그런데 웹을 처음부터 끝까지 혼자서 개발해본 적이 없다. 기본이 갖춰진 환경에서부터 개발을 시작하다보니 이런 기본적인 설정에 대해 모르고 있는 게 많다.


다음은 `PleaseBuy` API 서버를 개발하면서 SQL에 대한 Log를 개발 중에 보고 싶어서 설정한 방식이다. 개발 환경은 `spring 3.2`, `Mybatis 1.2.2` 이다.

먼저, 기존에 log4 설정이 안되어 있던 환경설정이다.

- `id="dataSource"` :: DB 관련 설정
- `id="sqlSessionFactory"` :: Spring-Mybatis 연동 설정
	- REF :: [spring-mybatis 연동](http://www.mybatis.org/spring/ko/factorybean.html)

`sqlSessionFactory`에서 `dataSource`를 참조하고 있다.

```xml
<beans:bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
	<beans:property name="driverClassName" value="${jdbc.driverClassName}" />
	<beans:property name="url" value="${jdbc.url}" />
	<beans:property name="username" value="${jdbc.username}" />
	<beans:property name="password" value="${jdbc.password}" />
	...
</beans:bean>


<beans:bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
	<beans:property name="dataSource" ref="dataSource" />
	...
</beans:bean>
```

REF :: [Mybatis 쿼리 로그 출력과 정렬](http://addio3305.tistory.com/66)

다음은 `PleaseBuy` API서버를 개발하면서 `log4j`를 통해 실행된 쿼리를 보기 위해 설정한 항목이다.

위의 설정과 비교하여 `dataSourceLog4j` 설정이 추가되었다. 그리고 `sqlSessionFactory`, `dataSource`의 참조 관계가 이를 거치는 방식으로 변경되었다.

- `dataSourceLog4j` :: log 설정이 추가된 dataSource

```xml
<beans:bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
	<beans:property name="driverClassName" value="${jdbc.driverClassName}" />
	<beans:property name="url" value="${jdbc.url}" />
	<beans:property name="username" value="${jdbc.username}" />
	<beans:property name="password" value="${jdbc.password}" />
	...
</beans:bean>

<beans:bean id="dataSourceLog4j" class="net.sf.log4jdbc.Log4jdbcProxyDataSource">
	<beans:constructor-arg ref="dataSource" />
	<beans:property name="logFormatter">
		<beans:bean class="net.sf.log4jdbc.tools.Log4JdbcCustomFormatter">
			<beans:property name="loggingType" value="MULTI_LINE" />
			<beans:property name="sqlPrefix" value="SQL:::" />
		</beans:bean>
	</beans:property>
</beans:bean>

<beans:bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
	<beans:property name="dataSource" ref="dataSourceLog4j" />
	...
</beans:bean>
```
