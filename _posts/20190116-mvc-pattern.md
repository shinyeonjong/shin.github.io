---
layout: post
title: "MVC 패턴"
description: "mvc pattern"
date: 2019-01-16
tags: [mvc, pattern]
comments: false
share: false
---
## MVC 패턴

#### Model View Controller


* Model : 순수한 데이터 원형      
 역할1. Subject 역할 (Controller 에게 notify)
* Controller : 조정자, 매니저     
역할1. 모델을 생성한다.      
역할2. 모델의 옵저버.       
역할3. 뷰 생성       
역할4. 뷰에게 모델을 전달     
역할5. 뷰의 액션을 처리      
* View : 화면     
역할1. 모델에 기반하여 렌더링 처리        
역할2. 입력 처리를 컨트롤러에게 위임       

**Q.** 
```Model```에 기반하여 렌더링 하는것이  ```View```라면 ```Model```의 수신을 ```View``` 가 받아야 하지 않은가?      
즉, Observer는 ```View```가 아닌가?           
**A.** 
보통은 ```Model```의 변화를 ```View```가 수신하여 그릴수 없다.
```View```는 전체 작업 과정 중 일부만 그리게 되어있어 ```Controller```가 전체를 알고 있고 데이터의 후처리 또는 ```Controller```에 상황을 더 넣어 준다든지 이러한 추가 작업을한 ```Model```도 넣어주기 때문에 ```View```가 그냥 수신한다 해도 방법이 없다.


**Q.** 
```Controller```는 누가 만들었을까?     
```Model``` 과 ```View```는 ```Controller```가 만들었을텐데...       
**A.** 
라우터, 앱, 스프링에서는 MVC라는 그 전체 framework






