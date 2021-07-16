# flask로 REST API 구현하기

## HTTP(Hypertext Transfer Protocol)
- Server/Client 모델로 Request/Response 사용
  - Client에서 요청(Request)을 보내면, Server에서 응답(Response)을 준다,
  - HTTP는 Connectionless 한 프로토콜임 - 1회성 Request 및 Response
  - TCP/IP socket을 이용해서 연결됨

## REST
- REST(REpresentational State Transfer)
  - 자원(resource)의 표현(representation)에 의한 상태 전달
   - HTTP URI를 통해 자원을 명시하고, HTTP Method를 통해 자원에 대한 CRUD Operation 적용
     - CRUD Operation와 HTTP Method
       - Create:생성(POST)
       - Read:조회(GET)
       - Update:수정(PUT)
       - Delete:삭제(DELETE)

## REST API
- REST 기반으로 서비스 API를 구현한 것
- 마이크로 서비스, OpenAPI(누구나 사용하도록 공개된 API) 등에서 많이 사용됨

## flask로 REST API 구현 방법
- 특정한 URI를 요청하면 JSON 형식으로 데이터를 반환하도록 만들면 됨
- 즉, 웹주소(URI) 요청에 대한 응답(Response)를 JSON 형식으로 작성
- Flask에서는 dict(사전) 데이터를 응답 데이터로 만들고, 이를 jsonify() 메소드를 활용해서 JSON 응답 데이터로 만들 수 

## httpie 사용법
- HTTP메서드를 쓰지 않으면, 디폴트로 GET
  - http GET http://localhost:8080/json_test
- http -v URI
  - 송신 HTTP 프로토콜 데이터도 함께 출력
    - http -v GET http://localhost:8080/json_test
  
## flask jsonify() 함수
 - 리턴 데이터를 JSON 포맷으로 제공

## REST API 구현


      from  flask import Flask, jsonify
      app = Flask(__name__)
      
      # data를 사전 데이터로 만들고, 이를 jsonify() 메서드에 널어서 return 해주면 됨
      
      @app.route('/json_test')
      def hello_json():
          data = {'name' : 'JH', 'region' : 'korea'}
          return jsonify(data)
      
      @app.route('/server_info')
      def server_json():
          data = {'server_name' : '127.0.0.1', 'server_port' : '1234'}
          return jsonify(data)
      
      if __name__ == '__main__':
          app.run(host="127.0.0.1", port="1234")



![dd](https://user-images.githubusercontent.com/76901290/125879265-d6869878-14cf-46c0-b0e8-0aa9e51f6a9f.png)
