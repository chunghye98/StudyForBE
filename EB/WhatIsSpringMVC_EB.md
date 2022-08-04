# What is Spring MVC

> 목차
> * What is MVC
> * What is Spring MVC


## 1. What is MVC

MVC는 Model-View-Controller의 약자이다.

개발 할 때, 위의 3가지 형태로 역할을 나누어 개발하는 방법론이다.

이렇게 역할을 나누었을 때, 비즈니스 처리 로직과 사용자 인터페이스 요소들을 분리시켜 서로 영향 없이 개발할 수 있다는 점이 장점이다.

* Model

어플리케이션이 **무엇**을 할 것인지 정의한 것이다. 즉, 내부 비즈니스 로직을 처리하기 위한 역할이다.

예를 들면, 로직을 위한 알고리즘, DB 와의 상호작용 ( CRUD ), 데이터 등이 있다.

* Controller

모델이 **어떻게** 처리할 지를 알려주는 역할을 한다. 웹 혹은 앱에서 화면의 로직 처리 부분이다. 화면에서 사용자의 요청을 받아서 처리되는 부분을 구현하게 되며, 요청 내용을 분석해서 모델과 뷰에 업데이트 요청을 하게 된다.

쉽게 말해, 사용자로부터 입력을 받고 모델 또는 뷰 사이의 중개 역할을 한다.

* View

화면에 **무엇**인가를 **보여주기 위한 역할**을 한다.

쉽게 말해, 최종 사용자에게 무엇을 화면으로 보여준다.

컨트롤러는 모델과 뷰가 각각 무엇을 해야 할지를 알고 있고, 통제한다. 비즈니스 로직을 처리하는 모델과 완전히 UI에 의존적인 뷰가 서로 직접 연관을 갖지 않게 한다.

### MVC의 한계

MVC에서 뷰는 컨트롤러에 연결되어 화면을 구성하는 단위요소이다. 따라서 다수의 뷰를 가질 수 있다. 

그리고 모델은 컨트롤러를 통해서 뷰와 연결되지만, 컨트롤러를 통해서 하나의 뷰에 연결될 수 있는 모델도 여러 개가 될 수 있다.

즉, 뷰와 모델이 서로 의존성을 띄게 된다.

대규모의 MVC 어플리케이션의 경우, 컨트롤러는 뷰와 라이프 사이클이 강하게 연결되어 있어서 코드 분석/수정과 테스트가 힘들어진다. 

이를 보완하기 위해 다양한 패턴이 생겼는데, 이에 대해서는 추후 정리하겠다.

[mvc 정리 글](https://medium.com/@jang.wangsu/%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4-mvc-%ED%8C%A8%ED%84%B4%EC%9D%B4%EB%9E%80-1d74fac6e256)

## 2. What is Spring MVC

spring MVC는 MVC2 패턴에서 발전한 것이다.

[Client] < Request/ Response > [Front Controller] 
< 
delegate Request / 
delegate Response about Rendering > [Controller]
< Return Control / 
delegate Response about Rendering > [ View template ]

* FLOW

1. 클라이어니트가 URL 요청을 한다. 
2. 웹 브라우제서 spring으로 request를 보낸다.
3. 디스패처 서블릿이 request를 받는다.
4. 디스패처 서블릿이 핸들러 매핑을 통해 해당 url을 담당하는 컨트롤러를 찾는다.
> 개별 컨트롤러 클래스를 핸들러라고도 한다.

5. 디스패처 서블릿이 컨트롤러로 request를 보내주고, ( 위임하고 ) 이에 필요한 모델을 구성한다.
6. 모델에서는 페이지 처리에 필요한 정보들을 DB에 접근하여 쿼리문을 통해 가져온다.
7. 데이터를 통해 얻은 정보를 컨트롤러에게 response해준다.
8. 컨트롤러는 이를 받아 모델을 완성시켜 디스패처 서블릿에게 전달한다.
9. 디스패처 서블릿은 view resolver를 통해 request에 해당하는 뷰 파일을 탐색 후 받아낸다.
10. 받아낸 뷰 페이지 파일에 모델을 보낸다.
11. 뷰로부터 클라이언트에게 보낼 페이지를 완성시켜 받아낸다.
12. 완성된 뷰 파일을 클라이언트에게 response하여 화면에 출력한다.

### 구성요소

* 디스패처 서블릿

모든 request를 처리하는 중심 컨트롤러이다.

서블릿 컨테이너에서 HTTP 프로토콜을 통해 들어오는 모든 request에 대해 제일 앞단에서 중앙 집중식으로 처리해주는 역할을 한다. ( 그래서 프론트 컨트롤러라고도 한다. )

기존에는 web.xml에 모두 등록해줘야 했지만, 디스패처 서블릿이 모든 request를 핸들링하면서 작업을 편리하게 할 수 있다.

* 핸들러 매핑

클라이언트의 request url을 어떤 컨트롤러가 처리해야 할 지 디스패처 서블릿에게 전달해주는 역할을 담당한다.

> 컨트롤러 클래스 상에서 url을 매핑하기 위해 @ReuqestMapping 어노테이션을 사용하는데, 이걸 통해 매핑한다.


* 컨트롤러

실질적인 request를 처리하는 곳이다.

디스패처 서블릿이 프론트 컨트롤러이면, 얘는 백 컨트롤러라고 볼 수 있다.

모델의 처리 결과를 담아 디스패처 서블릿에게 반환해준다.

* view resolver

컨트롤러의 처리 결과를 만들 뷰를 결정해주는 역할을 담당한다.

[사진과 그림이 깔끔한 spring mvc](https://velog.io/@miscaminos/Spring-MVC-framework)