# 효율적인 쿼리 작성 Tip-1
- Query Optimize Skill(Query 최적화 및 튜닝 기술)




<br/><br/>

## 1. SELECT 문에서 * 대신 열 이름 사용
필요한 일부 컬럼만을 선택함으로써 결과 테이블의 크기를 줄이고 네트워크 트래픽을 감소시킴으로써 쿼리의 평균 속도를 높일 수 있다.

Original:
```sql
SELECT * FROM SH.Sales;
```

Improved: **27% Time Reduction**
```sql
SELECT s.PROD_ID FROM SH.Sales s;
```





<br/><br/>

## 2. "SELECT" 문에 "HAVING" 절 사용 피하기

Original:
```sql
SELECT s.cust_id, count(s.cust_id) 
FROM SH.Sales s 
GROUP BY s.cust_id 
HAVING s.cust_id != '1660' AND s.cust_id != '2;
```

Improved: **31% Time Reduction**
```sql
SELECT s.cust_id, count(s.cust_id) 
FROM SH.Sales s 
WHERE s.cust_id != '1660' AND s.cust_id != '2' 
GROUP BY s.cust_id;
```





<br/><br/>

## 3. 불필요한 DISTINCT 제거
DISTINCT를 사용하면 정렬하는 과정이 들어가기 때문에, Query의 속도가 상당히 저하된다.

Original:
```sql
SELECT DISTINCT * 
FROM SH.Sales s 
JOIN SH.Customer c 
ON s.cust_id = c.cust_id 
WHERE c.cust_marital_status = 'single';
```

Improved: **85% Time Reduction**
```sql
SELECT * 
FROM SH.Sales s JOIN SH.Customer c 
ON s.cust_id = c.cust_id 
WHERE c.cust_marital_status = 'single';
```




<br/><br/>

## 4. 서브쿼리 하지 않고 다른 방법 찾기
중첩된 쿼리를 조인조건으로 재작성하는 것은 효율적인 실행과 효과적인 최적화를 불러일으킨다. 
일반적으로 서브쿼리를 푸는 것은 ANY, ALL, EXISTS 등이 사용되는 한개의 테이블에서 처리된다.

Original:
```sql
SELECT * 
FROM SH.products p 
WHERE p.prod_id = ( SELECT s.prod_id FROM SH.sales s WHERE s.cust_id = 100996 AND s.quantity_sold = 1 );
```

Improved: **61% Time Reduction**
```sql
SELECT p.* 
FROM SH.products p, sales s 
WHERE p.prod_id = s.prod_id AND s.cust_id = 100996 AND s.quantity_sold = 1;
```




<br/><br/>

## 5. 인덱스된 열을 쿼리할 때 IN를 사용 고려
IN-list는 index된 검색을 위해 활용될 수 있으며, Optimizer는 Index의 정렬 순서와 일치하도록  IN-list를 정렬하여 보다 효율적인 검색을 수행할 수 있다. IN-list에는 상수 또는 쿼리 블록이 실행되는 동안 일정한 값만 포함해야 한다.

Original:
```sql
SELECT s.* 
FROM SH.sales s 
WHERE s.prod_id = 14 
OR s.prod_id = 17;
```

Improved: **73% Time Reduction**
```sql
SELECT s.* 
FROM SH.sales s 
WHERE s.prod_id IN (14, 17);
```




<br/><br/>

## 6. 일대다 관계 테이블 조인(table join) 할 때 DISTINCT 대신 EXIST를 사용
DISTINCT 키워드는 테이블의 모든 열을 선택한 후에 중복되는 것들을 파싱하는 형태로 작동한다.  
만약 서브쿼리와 함께 EXISTS 키워드를 사용한다면, 전체 테이블을 조회하는 것을 피할 수 있다.

Original:
```sql
SELECT DISTINCT c.country_id, c.country_name 
FROM SH.countries c, SH.customers e 
WHERE e.country_id = c.country_id;
```

Improved: **61% Time Reduction**
```sql
SELECT c.country_id, c.country_name 
FROM SH.countries c 
WHERE EXISTS (SELECT 'X' FROM  SH.customers e WHERE e.country_id = c.country_id);
```




<br/><br/>

## 7. UNION 대신 UNION ALL 사용
UNION 문은 중복된 열의 존재 유무에 상관없이 열을 선택할 때 중복 검사를 하지만 UNION ALL은 중복검사를 하지 않으므로, UNION 보다는 UNION ALL을 사용하는 것이 빠르다.

Original:
```sql
SELECT cust_id FROM SH.sales 
UNION 
SELECT cust_id FROM customers;
```

Improved: **81% Time Reduction**
```sql
SELECT cust_id FROM SH.sales 
UNION ALL 
SELECT cust_id FROM customers;
```




<br/><br/>

## 8. 조인 조건에서는 OR 사용 금지
조인 조건에 'OR'을 사용할 때마다 쿼리는 최소한 2배 이상 느려진다. OR문을 사용하는 경우에는 Index를 활용한 검색을 하지 못하고, Full-Scan을 하기 때문이다.  
[참조 링크](http://intomysql.blogspot.com/2011/01/union-union-all.html)

Original:
```sql
SELECT * 
FROM SH.costs c 
INNER JOIN SH.products p 
ON c.unit_price = p.prod_min_price OR c.unit_price = p.prod_list_price;
```

Improved: **70% Time Reduction**
```sql
SELECT * FROM SH.costs c 
INNER JOIN SH.products p ON c.unit_price = p.prod_min_price 

UNION ALL 

SELECT * FROM SH.costs c 
INNER JOIN SH.products p ON c.unit_price = p.prod_list_price;
```




<br/><br/>

## 9. Avoid functions on the right hand side of the operator
집계 함수 기능을 제거하여 쿼리를 재작성하는 것은 성능을 상당히 높여줄 것이다.


Original:
```sql
SELECT * 
FROM SH.sales 
WHERE 
    EXTRACT (YEAR FROM TO_DATE (time_id, 'DD-MON-RR')) = 2001 
AND EXTRACT  (MONTH FROM TO_DATE (time_id, 'DD-MON-RR')) = 12;
```

Improved: **70% Time Reduction**
```sql
SELECT * 
FROM SH.sales 
WHERE 
    TRUNC (time_id) BETWEEN TRUNC(TO_DATE('12/01/2001', 'mm/dd/yyyy')) 
AND TRUNC (TO_DATE ('12/30/2001', 'mm/dd/yyyy'));
```



<br/><br/>

## 10. 중복 수학 연산 제거
수학연산은 매번 열을 찾아서 연산을 다시 하므로, 부적절하게 사용된다면 성능을 상당히 저하시킬 것이다. 


Original:
```sql
SELECT * 
FROM SH.sales s 
WHERE s.cust_id + 10000 < 35000;
```

Improved: **11% Time Reduction**
```sql
SELECT * 
FROM SH.sales s 
WHERE s.cust_id < 25000;
```




<br/><br/><br/><br/>


Reference
1. https://mangkyu.tistory.com/52