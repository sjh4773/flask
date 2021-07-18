# Rest API 요청시 파라미터/파라미터값 넣기
- HTTP 의 요청 방식 중, 가장 많이 사용되는 방식이 GET 방식
  - GET 방식에서는 URI 상에 파라미터와 파라미터 값을 넣을 수 있음
    - 규칙 : URL?파라미터1=파라미터1값^&파라미터2=파라미터2값
    - URL 이후 첫 파라미터 이름 전에 ? 를 표시하고, 추가 파라미터가 있을 시에는 ^& 표시를 해야 함


            # jsonify로 리턴, request로 url 뒤에 파라미터 값을 flask 안에서 사용할 수 있도록 해줌
            from flask import Flask, jsonify, request
            app = Flask(__name__)
            
            
            # get 방식으로 가져온 파라미터 값을 가져오려면 request.args.get으로 한 다음에
            #  여기에 파라미터 변수 이름을 적으면 파라미터 값이 다른 변수에 저장이 될 수 있음
            @app.route('/login')
            def login():
                username = request.args.get('user_name') 
                passwd = request.args.get('pw')
                email = request.args.get('email_address')
                
                if username == 'hun':                   
                    return_data = {'auth':'success'}
                else:
                    return_data = {'auth':'failed'}
                return jsonify(return_data)           # 사전 데이터를 jsonify에 호출을 해서 리턴해주면 프론트엔드 쪽에서 해당 데이터를 받을 수 있다.

            # 웹서버 실행
            if __name__ == '__main__':
                app.run(host="127.0.0.1",port="1234")



![Cap 2021-07-18 22-52-04-851](https://user-images.githubusercontent.com/76901290/126069872-1af04479-ff45-426d-88b0-a48118e87e44.png)
