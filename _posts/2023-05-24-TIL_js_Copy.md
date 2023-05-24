---
layout: single
title:  "얕은 복사? 깊은 복사?"
---

# 문제

가장 아래의 코드가 실행 되었을 때, “Passed ~” 가 출력되도록 getAge 함수를 채워주세요  
  
    var user = {
    name: "john",
    age: 20,
}

var getAged = function (user, passedTime) {
  // !!!!!!!!여기를 입력!!!!!!!
}


var agedUser = getAged(user, 6);

var agedUserMustBeDifferentFromUser = function (user1, user2) {
    if (!user2) {
		    console.log("Failed! user2 doesn't exist!");
	} else if (user1 !== user2) { 
        console.log("Passed! If you become older, you will be different from you in the past!");
    } else {
        console.log("Failed! User same with past one");
    }
}

    agedUserMustBeDifferentFromUser(user, agedUser);
  
# 문제점  
  
우선 가장 먼저 호출되는 함수를 찾아 흐름을 파악했다.
getAged(user, 6)이 실행되고, agedUserMustBeDifferentFromUser에 파라미터로 agedUser와 agedUser가 들어간다.
그리고 agedUserMustBeDifferentFromUser 함수를 분석하면 user1과 user2가 존재하고 두 객체가 다르면 “Passed ~”가 나오게 구현돼있다.
문제의 조금의 헛 점이 있어보이지만 출제자의 의도는 복제를 하지만 같은 값을 복사해서 age에 passedTime을 더해주어 agedUserMustBeDifferentFromUser 함수안에서 name과 age를 비교하려고 한 것 같다.  
우선 문제의 방향을 잡고 문제를 풀어보았다.


# 문제해결  

우선 객체의 값을 복사해오는 방법은 두 가지를 떠올렸다. 얕은 복사와 깊은 복사이다.  
이 둘 중 더 완벽한 복사는 깊은 복사이다. 그 이유는 얕은 복사는 객체안에 객체가 존재한다면 그 객체를 저장하는 주소값이 같기 때문에 완벽히 복사했다고 볼 수 없기 때문이다.
하지만 깊은 복사는 복사하는 함수안에서 본인을 한번 더 부르게 하여 객체까지도 한 번 더 들어가 완벽하게 복사를 할 수 있다. 이거를 재귀적 수행이라고 한다.  
  
나는 이 둘 중 얕은 복사를 활용해서 풀었다. 문제의 user 객체를 보면 안에 새로운 객체가 없기 때문에 깊은 복사를 활용하지 않아도 문제를 풀 수 있어 보였기 때문이다.

얕은 복사  
    let copyObject = function (target){
    var result = {};


    for(var prop in target){
        result[prop] = target[prop];
    }
    return result;
    }
  
깊은 복사  
    let copyObject2 = function (target){
    var result = {};

    if(typeof target === 'object' && target !== null){
        for(var prop in target){
            result[prop] = copyObject2(target[prop]);
        }
    }else{
        result = target;
    }
    
    return result;
    }
  
# 풀이  
  
그리고 문제 해결 방법을 적용한 코드가 아래이다.  
  
    var getAged = function (user, passedTime) {
    var result = {};

    for(var prop in user){
            result[prop] = user[prop];
    }
    
    result.age += passedTime
    return result;
}

var agedUser = getAged(user, 6);

var agedUserMustBeDifferentFromUser = function (user1, user2) {
    if (!user2) {
		    console.log("Failed! user2 doesn't exist!");
	} else if (user1 !== user2) { 
        console.log("Passed! If you become older, you will be different from you in the past!");
    } else {
        console.log("Failed! User same with past one");
    }
}

    agedUserMustBeDifferentFromUser(user, agedUser);
  
결과는 잘나오지만 문제 설정이 조금은 오류가 있어 그런지 조금은 찜찜해서 문제 코드를 아래처럼 조금 수정했다.  
    var agedUserMustBeDifferentFromUser = function (user1, user2) {
    if (!user2) {
		    console.log("Failed! user2 doesn't exist!");
	} else if (user1 !== user2 && user1.name === user2.name && user1.age !== user2.age) { 
        console.log("Passed! If you become older, you will be different from you in the past!");
    } else {
        console.log("Failed! User same with past one");
    }
    }
  
else if 부분에 이름과 age를 비교하게 바꿔주어 좀 더 원하는 게 명확해질 수 있도록 바꿔주었다.
  
# 느낀점  
  
얕은 복사와 깊은 복사를 공부하고 문제를 풀어보면서 더욱 코드를 읽고 따라가는 게 빨라진 느낌을 받았다. 코드를 읽고 뭔가 이상하다고 느껴 고치면서 풀어보면서 내가 제대로 잘 이해하고 있는 것 같은 느낌이 들어 뿌듯했다.
오늘도 많은 것을 배울 수 있었고 문제를 풀면서 성취감도 느껴 내일 더 열심히 공부를 해 보며 프로그래머스에서도 문제를 자주 풀어서 배운 내용들을 내 것으로 만들 것이고, 까먹지 않게 이 TIL도 다시 훑어보면서 하루를 마무리 하도록 해볼 예정이다.
  
