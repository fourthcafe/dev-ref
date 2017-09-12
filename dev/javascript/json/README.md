# Javascript

## Json

### javascript에서 java object 사용하기

페이지는 일반 http 통신으로 jsp를 사용해서 넘겨줬는데 데이터 핸들링을 Javascript에서 해야하는 상황이다.

java단에서부터 json형식으로 넘겨주고 javascript에서 파싱하여 사용하였다.

```java
model.addAttribute("dataListJson", new Gson().toJson(dataList));
```

```javascript
JSON.parse('${dataListJson}'),
```
