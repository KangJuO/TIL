## 1. N + 1 이란?

### ⚙️ Lazy Loading

- 사원의 정보가 들어있는 People 테이블, 부서를 나열한 Companies 테이블

```jsx
mysql> SELECT * FROM Companies;
+----+-------------+
| id | department  |
+----+-------------+
|  1 | finance     |
|  2 | marketing   |
|  3 | development |
|  4 | design      |
|  5 | planning    |
+----+-------------+
5 rows in set (0.00 sec)

mysql> SELECT * FROM People;
+----+------------+------------+-------------+------------+
| id | first_name | last_name  | city        | company_id |
+----+------------+------------+-------------+------------+
|  1 | John       | Doe        | Berlin      |          1 |
|  2 | Warwick    | Hawkins    | Dublin      |          1 |
|  3 | Kobi       | Villarreal | Peking      |          2 |
|  4 | Winnie     | Roach      | Ulaanbaatar |          3 |
|  5 | Peggy      | Nguyen     | Hanoi       |          5 |
+----+------------+------------+-------------+------------+
5 rows in set (0.00 sec)
```

- 위의 데이터 베이스에서 사원들의 부서를 가져오고 싶다!

```jsx
const people = await People.query(`SELECT * FROM People`);

for (let person of people) {
  const department = await Companies.query(
    `
    SELECT department 
    FROM Companies c
    WHERE c.id = :personId
  `,
  ).setParam("personId", person.id);
}
```

- 코드를 살펴보면 People 테이블에서 사원의 정보를 받아오고
- 가져온 정보를 가지고 반복문을 통해서 사원의 부서를 얻기위해 다시 쿼리를 날림
- **여기서 필요한 N 개의 데이터를 얻기 위해 N번의 쿼리를 수행하는것이 아닌**
- **최초의 쿼리 하나(사원정보) + N개의 쿼리(부서정보)**
- **이렇게 1+N개의 쿼리를 날려야하는것을 말함**

```jsx
SELECT * FROM People;

SELECT department FROM Companies c WHERE c.id = 1; -- finance
SELECT department FROM Companies c WHERE c.id = 1; -- finance
SELECT department FROM Companies c WHERE c.id = 2; -- marketing
SELECT department FROM Companies c WHERE c.id = 3; -- development
SELECT department FROM Companies c WHERE c.id = 5; -- planning
```

### 😱 이게 왜 문제가 되는거지...

- 위 처럼 반복문안에서 N+1의 쿼리를 보내는것을 지연로딩, lazy loading 라고함
- 초기 로딩시간을 줄일 수 있고, 자원의 소비를 줄일 수 있는 장점이있지만
- 사용하지 않는 데이터를 결과 객체에 포함지키지 않기 때문에 CPU를 절약할 수 있지만, 데이터베이스로 더많은 쿼리를 날려보냄, 원치 않는 순간에 성능에 영향을 줄 수있다.

---

### ⚙️ Eager Loading

- 가능한 적은 쿼리를 날리기 위해 JOIN 사용

```jsx
const people = await People.query(`
  SELECT * 
  FROM People p
  JOIN Companies c
  ON p.id = c.id
`);

for (let person of people) {
  const department = person.company.department;
}
```

```jsx
mysql> SELECT * FROM People p JOIN Companies c ON p.id = c.id;
+----+------------+------------+-------------+------------+----+-------------+
| id | first_name | last_name  | city        | company_id | id | department  |
+----+------------+------------+-------------+------------+----+-------------+
|  1 | John       | Doe        | Berlin      |          1 |  1 | finance     |
|  2 | Warwick    | Hawkins    | Dublin      |          1 |  2 | marketing   |
|  3 | Kobi       | Villarreal | Peking      |          2 |  3 | development |
|  4 | Winnie     | Roach      | Ulaanbaatar |          3 |  4 | design      |
|  5 | Peggy      | Nguyen     | Hanoi       |          5 |  5 | planning    |
+----+------------+------------+-------------+------------+----+-------------+
5 rows in set (0.00 sec)
```

- 초기 로딩 시간이 길기때문에 불필요한 데이터를 너무 많이 로드하면 성능이 떨어진다.
- 하지만 쿼리의 수가 줄어든다..!

---

## 🛠TypeORM, Sequelize

- TypeORM은 스키마 선언부에서 eager loading을 할지 결정 가능

```tsx
// src/entities/People.ts
@ManyToOne(type => Company, { eager: true })
@JoinColumn()
company: Company
```

- Sequelize는 find~ 메서드의 옵션에 `include`를 추가해서 사용하면 eager loading 시킬 수 있음
- 반대로 lazy loading같은 경우에는 `include` 옵션을 사용하지 않으면 가능

```jsx
const people = await People.findOne({ include: Companies, where: { id: 1 } });
```

자료출처 : https://yangeok.github.io/orm/2020/11/23/typeorm-theoritical.html
