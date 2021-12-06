# Cors 란?

> 나는 사실 토이 프로젝트를 하면서 cors 로 인해 막히거나 했던 부분은 없었다. Https를 적용하지 않고 Https로 요청을해 cors를 한번 경험해본것 말고는...

### 과거에는?

옛난에는 브라우저를 통해 누구나 데이터를 요청하고, 응답이 가능했다. 하지만 이는 보안에서 문제점이 생기게 되어 SOP(Same Origin Policy) 가 탄생하는 계기가 되었다. 만약 네이버의 웹페이지에서 다음으로 요청을 보내고 데이터 값을 받아온다 생각해보자.

그렇게 된다면 검색엔진을 사용하여 사용자에게 정보를 제공해주는 두 경쟁사가 서로의 리소스를 훔쳐 사용하게 될수도 있다. **또 해커가 만든 웹사이트에서 원치 않은 요청을 내가 요청한것처럼 이루어지게 될수도 있다. (이를 CRSF라 한다.)**

---

### SOP
이러한 이유때문에 출처를 비교하는 SOP가 생기게 되었다. SOP는 Same Origin Policy 의 약자로 **다음과 같이 출처를 비교하고 출처가 같아야만 데이터를 응답해주는 보안 정책이다.**

**Origin ( 출처 )?**

Origin이 무엇인가에 대해서 부터 알아보자. Origin은 우리가 흔히 아는 URL을 의미한다. 

다음 URL을 분류 해보면
**http://store.company.com/dir/page.html ** 
* Http:// (프로토콜)
* store.company.com (도메인)
* /dir/page.html (경로)

이때 다음 3가지가 같으면 같다고 판단하게 된다. 
* Scheme (프로토콜)
* host (도메인)
* port (포트번호)

다음은 http://store.company.com/dir/page.html 에대해 출처를 비교하는 예시이다.
![](https://images.velog.io/images/donglee99/post/c368a312-8e11-47d8-accf-397f215f60cd/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202021-12-06%20%EC%98%A4%ED%9B%84%208.36.12.png)

즉 SOP로 인해 www.daum.net에서 요청을 해야지만 daum의 리소스를 제공 받을수 있게 되었다. 

---

**그렇다면 다른 웹사이트의 API를 사용할수 있는 방법은..?**

>현재 우리가 사용하는 웹사이트들만 해도 모두 각자의 리소스만 제공 받고 있는게 아니다.그렇다면 앞에서 말한 SOP는 어떻게 되는 것인가? 
출처가 다른데 어떻게 리소스를 가져오지? 따라서 이를 극복하기 위해 생겨나게 된것이 Cors이다.

### Cors

**Cors**란 Cross-Origin Resource Sharing 의 약자로 이를 해석한다면 **교차 출처 리소스 공유** 라고 해석이 가능하다. **간단하게 말하면 다른 출처의 요청이어도 서버에서 허용하고 응답 결과를 보내주는 것이다.** 하지만  단어를 풀어 쓴다고 해서 바로 "아 이거다!" 하고 와 닿지는 않는다. (교차라는 표현보다는 다른이 더 맞는 표현인거 같다.)

만약 자신이 속하지 않은 다른 도메인, 다른 프로토콜, 다른 포트의 리소스등을 요청하게 된다면 Cors를 만나볼수 있을 것이다. 이는 Cors라는 방어막을 작동시켜 **우리가 가져오는 리소스에 대한 안전성을 보장 받게 되는것이다.**

**Cors 요청**

1. Simple Requests (단순 요청)
2. Preflighted Requests (사전 요청)

### Simple Requests

![](https://images.velog.io/images/donglee99/post/90d504f7-3e2a-4558-94c8-e09f37dbdabc/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202021-12-06%20%EC%98%A4%ED%9B%84%209.00.06.png)

이 방법은 **한번에 요청과 응답이 오고 간다.** 보기에는 매우 단순하지만 **이에 따른 제약 조건이 많다. **
* **Method** : Get, Post, Head 만 가능
* **Header** : Accept, Accept-language, Content-language, Content-type
* **Content-type** : application/x-www-form-urlencoded, multipart/form-data, text/plain
* **XMLHttpRequestUpload객체에 EventListener가 없을때**
* **ReadableStream 사용하지 않을때**


### Preflighted Requests
이 방법은 2번의 요청이 오고간다. 먼저 Options 메소드를 이용해 다른 도메인의 리소스로 Http 요청을 보내 실제 요청이 전송하기 안전한지 확인한다. 이를 보고 Preflighted 라고 칭한다.
* 미리 안전한지 검증하고
* 요청 수행
![](https://images.velog.io/images/donglee99/post/ac37fb21-5d17-4fc7-b325-4759d425cb00/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202021-12-06%20%EC%98%A4%ED%9B%84%209.07.59.png)

다음과 같은 요청을 한다 하면
![](https://images.velog.io/images/donglee99/post/228143c6-7357-4609-9b68-bce44f199298/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202021-12-06%20%EC%98%A4%ED%9B%84%209.08.38.png)

![](https://images.velog.io/images/donglee99/post/fb22625d-56b1-4632-b8d6-a2a0751ad0b5/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202021-12-06%20%EC%98%A4%ED%9B%84%209.09.54.png)

위와 같은 결과가 나온다.
* access-control-allow-origin -> 출처 허용
* access-control-allow-method -> 유효 메소드
* access-control-allow-header -> 헤더에 위 값을 담아 요청 가능
* access-control-max-age -> 이때까지 캐싱 가능

![](https://images.velog.io/images/donglee99/post/89fd1451-6605-490e-9b08-4b79395b43b0/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202021-12-06%20%EC%98%A4%ED%9B%84%209.10.16.png)

---

### 해결 방법
**Access-Control-Allow-Origin 세팅하기**
* 출처를 애초부터 허용되지 않게 설정 

**프록시 서버 구축**
* 개발 환경 세팅을 잘해뒀는데 문제가 생긴다면 프록시 설정을 하자



---
## Reference
https://developer.mozilla.org/ko/docs/Web/Security/Same-origin_policy
https://velog.io/@young_pallete/CORS

