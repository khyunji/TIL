# 201007 SQL & ORM Practice

### 실습 전 세팅

sql_orm_practice.zip 파일 압축 해제한 뒤

```bash
$ python manage.py migrate
$ python manage.py loaddata users/users.json

$ python manage.py shell_plus --print-sql
$ User.objects.count()
# 100개 출력되는지 확인
```



1. user 테이블 전체 데이터를 조회하시오.

```python
# ORM
User.objects.all()
```

```sql
-- SQL
SELECT * FROM users_user;
```



2. id가 19인 사람의 age를 조회하시오.

```python
# ORM
User.objects.filter(pk=19).values('age')
```

```sql
-- SQL
SELECT age FROM users_user WHERE id = 19;
```



3. 모든 사람의 age를 조회하시오.

```python
# ORM
User.objects.all().values('age')
User.objects.values('age')
```

```sql
-- SQL
SELECT age FROM users_user;
```



4. age가 40 이하인 사람들의 id와 balance를 조회하시오.

```python
# ORM
users = User.objects.filter(age__lte=40).values('id', 'balance')
for user in users:
	print(user.get('id'))
	print(user.get('balance'))
```

```sql
-- SQL
SELECT id, balance FROM users_user
WHERE age <= 40;
```

`lte`, `lt`, `gte`, `gt` 

`<=`, `<`, `>=`, `>`



5. last_name이 '김'이고 balance가 500 이상인 사람들의 first_name을 조회하시오.

```python
# ORM
User.objects.filter(last_name='김', balance__gte=500).values('first_name')
```

```sql
-- SQL
SELECT first_name FROM users_user
WHERE last_name = '김' AND balance >= 500;
```



6. first_name이 ‘수’로 끝나면서 행정구역이 경기도인 사람들의 balance를 조회하시오.

```python
# ORM
User.objects.filter(first_name__endswith='수', country='경기도').values('balance')
```

```sql
-- SQL
SELECT balance FROM users_user
WHERE first_name LIKE '%수' AND country = '경기도';
```

`__endswith='수'`

`LIKE '%수'`



7. balance가 2000 이상이거나 age가 40 이하인 사람의 총 인원수를 구하시오.

```python
# ORM
from django.models import Q
User.objects.filter(Q(balance__gte=2000)|Q(age__lte=40)).count()
```

```sql
-- SQL
SELECT COUNT(*) FROM users_user
WHERE balance >= 2000 OR age <= 40;
```

`Q()|Q()` / `.count()`

`OR` / `SELECT COUNT(*)`



8. phone 앞자리가 ‘010’으로 시작하는 사람의 총원을 구하시오.

```python
# ORM
User.objects.filter(phone__startswith='010').count()
```

```sql
-- SQL
SELECT COUNT(*) FROM users_user
WHERE phone LIKE '010%';
```



9. 이름이 ‘김옥자’인 사람의 행정구역을 경기도로 수정하시오.

```python
# ORM
User.objects.filter(first_name='옥자', last_name='김').update(country='경기도')

# 결과 확인 방법
User.objects.get(first_name='옥자', last_name='김').country
```

```sql
-- SQL
UPDATE users_user SET country = '경기도'
WHERE first_name = '옥자' AND last_name = '김';
```

`.update(country='경기도')`

`UPDATE users_user SET country = '경기도'`



10. 이름이 ‘백진호’인 사람을 삭제하시오.

```python
# ORM
user = User.objects.filter(first_name='진호', last_name='백')
user.delete()
```

```sql
-- SQL
DELETE FROM users_user
WHERE first_name = '진호' AND last_name = '백';
```

`DELETE **FROM**`



11. balance를 기준으로 상위 4명의 first_name, last_name, balance를 조회하시오.

```python
# ORM
users = User.objects.order_by('-balance').values('first_name', 'last_name', 'balance')[:4]

for user in users:
	print(user.get('first_name'))
	print(user.get('last_name'))
	print(user.get('balance'))
	print('--------------')
	# 엔터 두 번 치면 조건문 종료
```

```sql
-- SQL
SELECT first_name, last_name, balance FROM users_user
ORDER BY balance DESC LIMIT 4;
```

`order_by('-balance')`, `order_by('balance')` / `[:4]`

`ORDER BY balance DESC`, `ORDER BY balance ASC` / `LIMIT 4`



12. phone에 ‘123’을 포함하고 age가 30미만인 정보를 조회하시오.

```python
# ORM
User.objects.filter(phone__contains='123', age__lt=30)
```

```sql
-- SQL
SELECT * FROM users_user
WHERE phone LIKE '%123%' AND age < 30;
```

`phone__contains='123'`

`phone LIKE '%123%'`



13. phone이 ‘010’으로 시작하는 사람들의 행정 구역을 중복 없이 조회하시오.

```python
# ORM
User.objects.filter(phone__startswith='010').values('country').distinct()
```

```sql
-- SQL
SELECT DISTINCT country FROM users_user
WHERE phone LIKE '010%';
```

`.distinct()`

`SELECT DISTINCT`



14. 모든 인원의 평균 age를 구하시오.

```python
# ORM
from django.db.models import Avg
User.objects.aggregate(Avg('age'))
```

```sql
-- SQL
SELECT AVG(age) FROM users_user;
```

`.aggregate(Avg('age'))`

`SELECT AVG(age)`



15. 박씨의 평균 balance를 구하시오.

```python
# ORM
User.objects.filter(last_name='박').aggregate(Avg('balance'))
```

```sql
-- SQL
SELECT AVG(balance) FROM users_user
WHERE last_name = '박';
```



16. 경상북도에 사는 사람 중 가장 많은 balance의 액수를 구하시오.

```python
# ORM
from django.db.models import Max
User.objects.filter(country='경상북도').aggregate(Max('balance'))
```

```sql
-- SQL
SELECT MAX(balance) FROM users_user
WHERE country = '경상북도';
```



17. 제주특별자치도에 사는 사람 중 balance가 가장 많은 사람의 first_name을 구하시오.

```python
# ORM
User.objects.filter(country='제주특별자치도').order_by('-balance').values('first_name')[0]
```

```sql
-- SQL
SELECT first_name FROM users_user
WHERE country = '제주특별자치도' ORDER BY balance DESC LIMIT 1;
```

