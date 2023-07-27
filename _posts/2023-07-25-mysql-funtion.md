---
title: Mysql 기본 함수
date: 2023-07-25 18:00:00 +0800
categories: [database]
tags: [database]
---

# 숫자 함수

## ABS(value)

절대값을 구하는 함수

```sql
SELECT ABS(-100);
```

```
|ABS(-100)|
|      100|
```

## CEILING(value), FLOOR(value), ROUND(value)

CEILING : 올림 , FLOOR : 내림 , ROUND : 반올림

```sql
SELECT CEILING(3.7), FLOOR(3.7), ROUND(3.7);
```

```
|CEILING(3.7)| FLOOR(3.7)| ROUND(3.7)|
|           4|          3|          4|
```

## MOD(value1, value2)

MOD는 value1을 value2로 나눈 나머지 값을 구하는 함수

```sql
SELECT MOD(14, 3), 14 % 3, 14 MOD 3
```

```
|MOD(14,3) | 14 % 3| 14 MOD 3|
|         2|      2|        2|
```

## POW(value1,value2), SQRT(value)

POW 는 value1 ^ value2를 출력하는 함수, SQRT 는 value 루트 값을 출력하는 함수

```sql
SELECT POW(3, 2), SQRT(16)
```

```
|POW(3,2)| SQRT(16)|
|       9|        4|
```

## RAND

RAND는 0~1 무작위 실수를 반환하는 함수

```sql
SELECT RAND(), FLOOR(1 + (RAND() * 6));
```

```
|       RAND()| FLOOR(1+(RAND() * 6))|
|  0.931233123|                     3|
```

## TRUNCATE(value1,value2)

TRUNCATE는 소숫점을 기준으로 value1의 value2 번 째 정수 위치를 구하고 나머지는 버리는 함수

```sql
SELECT TRUNCATE(1234.1234, 2), TRUNCATE(1234.1234, -2);
```

```
|TRUNCATE(1234.1234,2)| TRUNCATE(1234.1234,-2)|
|               1234.2|                  1200|
```

## CONV(value1,value2,value3)

CONV는 기존 value2 진수인 value1을 value3진수로 변환하는 함수

```sql
SELECT CONV(100, 10, 2);
```

```
|CONV(100, 10, 2)|
|         1100100|
```

## DEGREES(value), PI, RADIANS(value)

DEGREES는 라디안 함수, RADIANS는 각도 값을 라디안 값으로 변환하는 함수, PI()는 3.14152fmf 반환하는 함수

```sql
SELECT DEGREES(PI()), RADIANS(180)
```

# 문자 함수

## SUBSTRING(value, start, length)

SUBSTRING 은 value를 start점부터 length만큼 출력하는 함수

```sql
SELECT SUBSTRING('abcd', 1, 3);
```

```
|SUBSTRING('abcd',1,3)|
|                  abc|
```

## SUBSTRING_INDEX(value, separator, count)

## ASCII(value)

ASCII는 value의 아스키 코드값을 반환하는 함수

```sql
SELECT ASCII('A');
```

```
|ASCII('A')|
|         A|
```

## CHAR(value)

CHAR는 value의 아스키 코드값에 해당하는 문자를 반환하는 함수

```mysql
SELECT CHAR(65);
```

```
|CHAR(65)|
|       A|
```

## BIT_LENGTH(value), CHAR_LENGTH(value), LENGTH(value)

BIT_LENGTH는 value에 할당된 Bit 크기, CHAR_LENGTH는 value문자의 개수, LENGTH는 value에 할당된 Byte수

```sql
SELECT BIT_LENGTH('abc'), CHAR_LENGTH('abc'), LENGTH('abc');
```

```
|BIT_LENGTH('abc')| CHAR_LENGTH('abc')| LENGTH('abc')|
|               24|                  3|             3|
```

## CONCAT(value1,value2,value3...), CONCAT_WS(ws,value1,value2,value3...)

CONCAT 은 value들을 하나로 합치는 함수, CONCAT_WS는 value를 합칠 때 사이에 ws를 넣어주는 함수

```sql
SELECT CONCAT('1', '2', '3', '4'), CONCAT_WS('/', '1', '2', '3', '4');
```

```
|CONCAT('1','2','3','4')| CONCAT_WS('/','1','2','3','4')|
|                   1234|                        1/2/3/4|
```

## ELT, FIELD, FIND_IN_SET, INSTR, LOCATE

## FORMAT(value1,value2)

value1를 소숫점 아래 자릿수 value까지 표시 추가적으로 1000 단위마다 콤마를 표시

```sql
SELECT FORMAT(1234.1234, 2);
```

```
|FORMAT(1234.1234,2)|
|           1,234.12|
```

## BIN(value), HEX(value), OCT(value)

BIN은 value를 2진수, HEX는 16진수, OCT는 8진수

```sql
SELECT BIN(31), HEX(31), OCT(31);
```

```
|BIN(31)| HEX(31)| OCT(31)|
|  11111|      1f|      37|
```

## INSERT(value,start,count,change)

INSERT는 value의 start부터 start + count 를 지우고 change를 삽입하는 함수

```sql
SELECT
INSERT
(
'abcde'
,
2
,
2
,
'fff'
);
```

```
|INSERT('abcde',2,2,'fff')|
|                   afffde|
```

## LEFT(value,length) , RIGHT(value, length)

LEFT는 value를 왼쪽에서 length만큼 반환하는 함수, RIGHT는 오른쪽부터 반환하는 함수

```sql
SELECT LEFT ('abcde', 3), RIGHT ('abcde', 3);
```

```
|LEFT('abcde',3)| RIGHT('abcde',3)|
|            abc|              cde|
```

## LCASE(value) , UCASE(value)

LCASE는 모두 소문자로 반환하는 함수, UCASE는 모두 대문자로 반환하는 함수

```sql
SELECT LCASE('aBcDe'), UCASE('aBcDe');
```

```
|LCASE('aBcDe')| UCASE('aBcDe')|
|         abcde|          ABCDE|
```

## LPAD(value, length, insert), RPAD(value, length, insert)

LPAD는 value를 length만큼 늘리고 왼쪽에 insert를 넣는 함수 사이즈가 넘어갈 경우 insert가 짤린다, RPAD는 오른쪽에 넣는 함수

```sql
SELECT LPAD('abc', 5, '**'), RPAD('abc', 5, '**');
```

```
|LPAD('abc', 5, '**')| RPAD('abc', 5, '**')|
|               **abc|                abc**|
```

## LTRIM(value), RTRIM(value), TRIM(value)

LTRIM은 value의 왼쪽 공백, RTRIM은 오른쪽 공백, TRIM은 모든 공백 제거하는 함수

```sql
SELECT LTRIM('  abc '), RTRIM(' abc '), TRIM(' abc ');
```

```
|LTRIM('  abc ')| RTRIM(' abc ')| TRIM(' abc ')|
|           abc |            abc|           abc|
```

## TRIM FROM

## REPEAT(value, count)

REPEAT는 value를 count 만큼 반복해서 반환하는 함수

```sql
SELECT REPEAT('abc', 3);
```

```
|REPEAT('abc',3)|
|      abcabcabc|
```

## REPLACE(value,old,new)

REPLACE는 value에서 old를 new로 바꿔 반환하는 함수

## REVERSE(value)

REVERSE는 value를 반대로 뒤집어 반환하는 함수

```sql
SELECT REVERSE('abc');
```

```
|REVERSE('abc')|
|           cba|
```

## SPACE(value)

SPACE는 value만큼 공백을 생성하여 반환하는 함수
