##  Session

cookie를 좀 더 안전하고 scalable한 효과를 낼 수 있는 방법.(개선)

* **cookie** 

  * 웹브라우저가 서버에 접속하면, 서버가 응답을 하면서 cookie에 모든 정보를 저장하게 됨.(Save all)
  * 사용자의 컴퓨터와 서버가 통신하는 과정에서 아이디, 비밀번호가 왔다갔다하는 경우, 중간에 누가 가로챌 수 있음.(보안이슈)

* **session**
  * 이러한 cookie가 가지고있는 문제점들을 개선하기 위해서 서버쪽에서 데이터를 저장할 수 있는 공간을 잘 조합해서 만든 것이 **session**
  * cookie방식과는 다르게 사용자의 식별자 id값만 저장한다.(Save only id)
  * 값에 해당되는 정보들은 서버쪽의 database나 파일에 저장.(real data)
  * 사용자가 요청으로 식별자를 전송하면 그에 맞는 실제 데이터를 메모리에서 가져와 보내준다.

  * 웹서버는 웹브라우저대신 구체적인 값을 저장하는 대신에 웹브라우저에 고유한 값(connect.sid)을 보낸다.
  * connect.sid가 같다면 같은 사용자라고 볼 수 있다.
  * cnnect.sid를 이용하여 값을 저장한다. 이 id가 요청을 보내면 이 요청에 대한 값을 웹브라우저에 보내준다.

* cookie와 server의 차이는 사용자의 컴퓨터에 직접 저장하느냐 식별자만 저장하느냐에 따라 다르다.
* 사용자의 컴퓨터 자체에 쿠키값이 저장되지 않기 때문에 훨씬 더 안전하다. 데이터를 서버에 저장하기 때문에.
* 사용자의 식별자를 통해 구분.



### Session 설치
![1549852480197](https://user-images.githubusercontent.com/38032500/52551821-68e2c000-2e21-11e9-9126-7dff6e218dbd.png)

**<실습1>**

`app_session.js`  : connect.sid값을 이용하여 각각 별도의 데이터를 서버에 저장해서 유지할 수 있다.

express-session 은 기본적으로 메모리에 데이터를 저장한다. 재접속할 경우, 다시 정보를 만들어서 count는 1부터 다시 저장. 실제 서비스할 때는 데이터베이스에 session 데이터를 저장해야 함.

### Login

`/auth/login`

### Session Store

메모리가 아닌 영구적인 데이터 시스템에 저장하는 방법. 파일에 sessio id 데이터를 저장.

`express session store file` 키워드로 검색.

```java
$ npm install session-file-store --save
```

```java
var session = require('express-session');
var FileStore = require('session-file-store')(session);
 
app.use(session({
    store: new FileStore(options),
    secret: 'keyboard cat'
}));
```

`store: new FileStore(options)`세션 데이터를 저장하는 디렉토리를 만든다 


![1549860620403](https://user-images.githubusercontent.com/38032500/52551819-684a2980-2e21-11e9-8e62-6a946287adec.png)

=> session 디렉토리가 생김 : `sessions`

![1549860505155](https://user-images.githubusercontent.com/38032500/52551823-68e2c000-2e21-11e9-81ef-331f4b4758ee.png)

* 파일에서 읽어올 수 있다면 언제든지 세션 데이터를 사용 가능.

### Orient DB를 세션 스토어로 사용

* `connect-oriento`

Orient-DB session store for [Connect](https://github.com/senchalabs/connect) and [Express](http://expressjs.com/) based on the binary transfer protocol in `oriento`.

* 설치(--save : 패키지에 포함)

```java
npm install connect-oriento --save
```

```
var OrientoStore = require('connect-oriento')(session);
```



![1549861210774](https://user-images.githubusercontent.com/38032500/52551828-6aac8380-2e21-11e9-8972-d537e4ba9412.png)

