# Flask(플라스크) 특징
- 마이크로 프레임워크 기방
- 웹 개발 최소 기능 제공, RESTful 요청 처리, 유니코드 기반, 필요한 부분은 추가해서 확장 가능

## flask 기본 사용법

### flask 모듈 임포트
- pip install flask


            from flask import Flask
            
            # Flask 객체를 app에 할당
            # 웹서비스를 만들기 위해서는 웹서비스에 해당하는 객체를 만들어야한다
            # app이라는 객체를 만들고 이 객체에 웹서비스에 필요한 기능을 추가하는 형태로 웹서비스 구현
            
            app = Flask(__name__)
            
            input:
            __name__
            
            output:
            '__main__'


### __name__이란?
- __name__이라는 변수는 모듈의 이름이 저장됨
- 실행하는 코드에서는 __main__이 들어감


### flask 객체 생성
- Flask(__name__)으로 설정하여, 현재 위치를 flask 객체에 알려줘야 함
   - 이름을 변경해도 정상 실행되지만, 일부 확장 기능 사용시에는 해당 이름을 정확히 알려주지 않을 경우, 정상 동작되지 않음



            from flask import Flask
            app = Flask(__name__)


            input:
            app

            output:
            <Flask '__main__'>

### 라우팅 경로를 설정
#### URL이란
- Uniform Resource Locator
- 인터넷 상의 자원 위치 표기를 위한 규약
- WWW 주요 요소 중 하나 : HTML, URL, HTTP
- URL vs URI
    - URI(Uniform Resource Identifier) : 통합 자원 식별자
    - URI의 하위 개념이 URL
    


### 라우팅(route)이란?
- 적절한 목적지를 찾아주는 기능
- URL을 해당 URL에 맞는 기능과 연결해 줌
- @으로 시작하는 코드는 데코레이터라고 함



            @app.route("/hello")
            def hello():
                return "<h1>Hello World!</h1>"


### 메인 모듈로 실행될 때 flask 웹 서버 구동
- 서버로 구동한 IP와 포트를 옵션으로 넣어줄 수 있음
- app.run() 함수로 서버 구동 가능
  - host, port, debug를 주로 사용함
    - host: 웹주소
    - port: 포트
    - debug: True or False

            
             run(host = None, port=None, debug=True)



### 기본 개발 프로세스
- 자신의 PC에서 웹서비스 구현
   - localhost, 127.0.0.1, 또는 0.0.0.0 으로 host 설정
   - app.run() 함수로 자체 웹서버 구현 가능



            # 자기 pc를 나타내는 ip주소 0.0.0.0 또는 127.0.0.1 
            host_addr = "127.0.0.1"
            # 서비스마다 번호를 가지고 있고 그것을 port라고 부른다.
            # 보통 8080 port는 test용도
            port_num = "1234"
            
            if __name__ == "__main__":
                app.run(host=host_addr, port=port_num)


### 전체 기본 코드
- jupyter notebook 사용



            from flask import Flask
            
            app = Flask(__name__)
            @app.route("/hello")
            def hello():
                return "<h1>Hello Flask!</h1>"
            
            if __name__ == "__main__":
                app.run(host="127.0.0.1", port="1234")