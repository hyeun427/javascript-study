## 30.1 Date 생성자 함수

- Date 생성자 함수로 생성한 Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 갖는다.

### 30.1.1 new Date()

- Date 생성자 함수를 인수 없이 new 연산자와 함께 호출하면 **현재 날짜와 시간을** 가지는 Date 객체를 반환한다.

- new 연산자 없이 호출하면 Date 객체를 반환하지 않고 날짜와 시간 정보를 나타내는 문자열을 반환한다.

### 30.1.2 new Date(milliseconds)

- 숫자 타입의 밀리초를 인수로 전달하여 밀리초만큼 경과한 Date 객체 반환한다.

### 30.1.3 new Date(dateString)

- 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.
	- 전달한 문자열은 ```Date.parse 메서드```로 해석 가능해야 한다.
    
### 30.1.4 new Date(year, month[, day, hour, minute, second, millisecond])

- 연, 월, 일, 시, 분, 초, 밀리초를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 개게를 반환한다.
- 이때 **연, 월**은 반드시 지정해야한다.

----

## 30.2 Date 메서드

### 30.2.1 Date.now

- 1970년 1월 1일 00:00:00(UTC) 기점으로 현재 시간까지 경과한 밀리초 숫자로 반환한다.

### 30.2.2 Date.parse

- 1970년 1월 1일 00:00:00(UTC) 기점으로 인수로 전달된 지정 시간 까지의 밀리초를 숫자로 반환한다.

### 30.2.3 Date.UTC

- 1970년 1월 1일 00:00:00(UTC) 기점으로 인수로 전달된 지정 시간까지 밀리초를 숫자로 반환한다.
- ```new Date(yaer, month, [, day, hour, minute, second, millisecond])```와 같은 형식의 인수를 사용해야 한다.

### 30.2.4 Date.prototype.getFullYear

- Date 객체의 연도를 나타내는 정수 반환한다.

### 30.2.5 Date.prototype.setFullYear

- Date 객체에 연도를 나타내는 정수 설정한다.
- 연도 이외에 옵션으로 월, 일도 설정할 수 있다.

### 30.2.6 Date.prototype.getMonth

- Date 객체의 월을 나타내는 0 ~ 11 정수를 반환한다.
	- Date 객체에서의 월은 0부터 시작한다.(1월은 0, 12월은 11)
    
### 30.2.7 Date.prototype.setMonth

- Date 객체에 월을 설정한다.
- 월 이외에 옵션으로 일도 설정할 수 있다.

### 30.2.8 Date.prototype.getDate 

- Date 객체의 날짜(1~31)를 나타내는 정수를 반환한다.

### 30.2.9 Date.prototype.setDate 

- Date 객체에 날짜(1~31)를 나타내는 정수를 설정한다.

### 30.2.10Date.prototype.getDay

- Date 객체의 요일을 나타내는 0 ~ 6 반환한다.
- ![](https://velog.velcdn.com/images/hyeun427/post/a810a200-07e9-4ae3-853c-0340656d56eb/image.png)


### 30.2.11 Date.prototype.getHours

- Date 객체 시간을 나타내는 정수를 반환한다.

### 30.2.12 Date.prototype.setHours

- Date 객체에 시간을 나타내는 정수를 설정한다.
- 시간 이외에 옵션으로 분, 초, 밀리초도 설정할 수 있다.

### 30.2.13 Date.prototype.getMinutes

- Date 객체의 분(0 ~ 59)을 나타내는 정수를 반환한다.

### 30.2.14 Date.prototype.setMinutes 

- Date 객체에 분(0 ~ 59)을 나타내는 정수를 설정한다.
- 분 이외에 옵션으로 초, 밀리초도 설정할 수 있다.

### 30.2.15 Date.prototype.getSeconds 

- Date 객체의 초(0 ~ 59)를 나타내는 정수를 반환한다.

### 30.2.16 Date.prototype.setSeconds

- Date 객체에 초(0 ~ 59)를 나타내는 정수를 설정한다.
- 초 이외에 옵션으로 밀리초도 설정할 수 있다.

### 30.2.17 Date.prototype.getMilliseconds

- Date 객체의 밀리초(0~999)를 나타내는 정수를 반환한다.

### 30.2.18 Date.prototype.setMilliseconds

- Date 객체에 밀리초(0~999)를 나타내는 정수를 설정한다.

### 30.2.19 Date.prototype.getTime 

- 1970년 1월 1일 00:00:00(UTC) 기점으로 Date 객체의 시간까지 경과된 밀리초 반환한다. 

### 30.2.20 Date.prototype.setTime 

- 1970년 1월 1일 00:00:00(UTC)를 기점으로 경과된 밀리초 설정한다.

### 30.2.21 Dat.prototype.getTimezonOffset

- UTC와 Date 객체에 지정된 로캘(locale) 시간과의 차이를 분 단위로 반환한다.

### 30.2.22 Date.prototype.toDateString

- 사람이 읽을 수 있는 형식으로 Date 객체의 날짜 반환한다.

### 30.2.23 Date.prototype.toTimeString

- 사람이 읽을 수 있는 형식으로 Date 객체의 시간을 표현한 문자열 반환한다.

### 30.2.24 Date.prototype.toISOString 

- ISO 8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열 반환한다.

### 30.2.25 Date.prototype.toLocaleString

- 전달한 로캘을 기준으로 Date 객체의 날짜, 시간을 표현한 문자열 반환한다.
- 인수를 생략할 경우 시스템의 로캘을 적용한다.

### 30.2.26 Date.prototype.toLocaleTimeString

- 인수로 전달한 로캘 기준으로 Date 객체의 시간을 표현한 문자열 반환한다.
- 인수를 생략할 경우 시스템의 로캘을 적용한다.

---
블로그에 작성한 파트 외 정리본은 아래의 깃헙에서 확인 가능합니다!
> https://github.com/hyeun427/javascript-study


