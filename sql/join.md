# LEFT JOIN

평소 `LEFT JOIN`을 쓸 때 `JOIN` 에는 `타 테이블과의 관계`만 쓰고 그 외 조건은 `WHERE` 에 적고 있었다. 출처는 기억나지 않지만 SQL을 처음 배울 때 그런 말을 들었었고, 그게 맞다고 생각해서 지금까지 그렇게 사용해오고 있었다.

그런데 오늘(2017-09-05), 이 맹신이 깨졌다. 평소라면 `WHERE` 에 적었을 쿼리 덕분에 시간을 한참 허비했다.

다음은 수업(class)과 그 수업의 예약자(reserver)를 `JOIN` 하는 쿼리이다. 수업의 이름과 수업의 예약 정원, 현재 예약 인원을 조회하는데, 예약한 사람이 없는 수업도 조회하기 위해 `LEFT JOIN`을 사용하였다.

```sql
SELECT
	class.class_name,
	class.limit_cnt,
	count(reserver.pk) AS reserver_cnt
FROM class
	LEFT JOIN reserver
		ON reserver.pk = class.fk_reserver
WHERE
	reserver.use_yn = ‘Y’
GROUP BY
	class.pk
```

여기서 문제가 되는 부분은 `WHERE`의 `reserve.use_yn = 'Y'`이다. 이 문장으로 인해 예약이 없는 수업은 조회가 되지 않았다. 마치 `INNER JOIN`처럼 작동한다.

<del>아직 확실하진 않다. `GROUP BY`, `COUNT` 함수와 같이 쓰면서 발생한 문제인지, 아니면 해당 문제의 경우 `JOIN` 에 거는 게 맞는 것인지. 좀 더 확인해봐야겠다.</del>

해당 문제는 `reserve.use_yn = 'Y'`가 `WHERE`에 있어서 발생한 오작동이 맞다. 원인을 분석해보자면 `LEFT JOIN` 후에 `WHERE`에서 오른쪽에 있는-값이 있을 수도 없을 수도 있는- 컬럼을 조건으로 거니 해당 컬럼이 없는 레이블은 조회가 안되는 것이다. 쿼리를 다음처럼 변경하여 문제를 해결하였다.

```sql
SELECT
	class.class_name,
	class.limit_cnt,
	count(reserve.pk) AS reserve_cnt
FROM class
	LEFT JOIN reserve
		ON reserve.pk = class.fk_reserve
			AND reserve.use_yn = ‘Y’
GROUP BY
	class.pk
```

# Join, Where의 차이

위에 LEFT JOIN 글에 이어서, `join`과 `where`의 차이에 대해 고민해보았다.

[on, where 의 차이](http://egloos.zum.com/pdw213/v/4171203)

요약
- `LEFT JOIN` 시 `ON`에는 우측(`NULL`로 채워지는 쪽)의 추가 제약조건을 넣고 좌측의 추가 제약조건은 `WHERE`에 넣어야 한다.
- 내부 조인일 경우에는 `ON`과 `WHERE` 어디에 넣어도 결과는 같다.
