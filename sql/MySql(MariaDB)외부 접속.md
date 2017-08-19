# 외부 접속

[REF1](https://zetawiki.com/wiki/MySQL_%EC%9B%90%EA%B2%A9_%EC%A0%91%EC%86%8D_%ED%97%88%EC%9A%A9#cite_ref-1)

[REF2](http://zolasse.blogspot.kr/2014/04/mysqlmaria-db-root.html)

[REF3](http://blog.kgoon.net/27)

## SQL 버전에 따른 보안 관련 추가 사항 [REF4.  ](http://www.webs.co.kr/?document_srl=39586&mid=db&sort_index=readed_count&order_type=desc)

```
ERROR 1364 (HY000): Field 'ssl_cipher' doesn't have a default value
```
* user 생성 시 ssl_cipher, x509_issuer, x509_subject, authentication_string 필드 값을 '' 으로 넣는다.

```
INSERT INTO mysql.user (
	host, user, password,
	ssl_cipher, x509_issuer, x509_subject, authentication_string
) VALUES (
	'localhost', '사용자명', password('비밀번호'),
	'', '', '', ''
)
```
