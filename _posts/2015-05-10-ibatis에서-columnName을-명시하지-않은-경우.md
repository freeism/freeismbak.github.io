---
layout: post
title: "ibatis에서 columnName을 명시하지 않은 경우"
author: "freeism"
description: ""
category: "Development" 
tags: ["Java", "Ibatis"]
---
{% include JB/setup %}

신입사원 코드 리뷰를 하는데 이상한 부분이 있어서 찾아본 내용을 적어둡니다.

```sql
<resultMap>
	<result property="goodsId"/>
    ... 
</resultMap>
  
<select id="selectGoods" resultMap="Goods">
    SELECT 
        goodsid, 
        ... 
    FROM 
        goods 
</select>
```

위와 같은 코드에서 resultMap속성에 column명이 명시되어 있지 않은데, 정상적으로 동작한다?  
전체 소스는 생략하고 핵심만 쏙쏙 뽑아서 공유합니다. 
  
ibatis입니다. 

```java
if (columnName == null) {
    value = typeHandler.getResult(rs, columnIndex); 
} else { 
    value = typeHandler.getResult(rs, columnName); 
}
```
columnName이 없는 경우 columnIndex 값을 활용하게 되어 있습니다. 

```java
if (columnIndex != null) {
    mapping.setColumnIndex(columnIndex.intValue()); 
} else { 
    resultMappingIndex++; 
    mapping.setColumnIndex(resultMappingIndex); 
}
```
columnIndex가 없는 경우 ++연산자를 통해 선언된 순서대로 내부에서 index를 정하도록 되어 있습니다.  
그래서 특별히 columnName을 지정하지 않고, columnIndex가 없었지만 서로 매핑될 수 있었던 것 입니다.  
(실제로 result property 순서를 바꿔서 선언했더니 순서가 뒤바뀌어 출력됨을 확인했습니다^^) 

참고로 mybatis에서는 columnIndex라는 기능이 제거되었습니다.  
(아무래도 명시적이지 않고, 버그의 원천이 될 위험이...) 
```java
if (columnIndexProp != null) {
    throw new UnsupportedOperationException(
        "Numerical column indices are not supported.  Use the column name instead."
    ); 
}
```
