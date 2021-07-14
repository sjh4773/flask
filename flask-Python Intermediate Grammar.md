# 파이썬 중급 : 데코레이터의 이해

## flask 깊은 이해를 위한 파이썬 중급 문법 : nested function

- 데코레이터는 단지 파이썬 flask 뿐만 아니라, 다양한 언어 전반에 걸쳐서 많이 사용됨
- 파이썬 flask에서 나오는 데코레이터를 쓰기 전에, 언어 전번에 걸친 데코레이터 관련 기술을 이해
- 한번 이해해놓으면, 다양한 언어 전반에서 데코레이터를 만났을 때마다 꾸준이 도움이 됨

### 중첩 함수 (Nested function)
- 함수 내부에 정의된 또 다른 함수
- 중첩함수는 해당 함수가 정의된 함수 내에서 호출 및 반환 가능
- 함수 안에 선언된 변수는 함수 안에서만 사용 가능한 원리와 동일(로컬 변수)


      input:
      def outer_func():
          print('call outer_func function')
          # 중첩 함수의 정의
          def inner_func():
              return 'call inner_func function'
          # 중첩 함수 호출
          print(inner_func())
      
      outer_func()
      
      output:
      call outer_func function
      call inner_func function

      # 중첩함수는 함수 밖에서는 호출 불가 (outer_func 함수 안에서 선언되었으니, outer_func 함수 안에서만 호출 가능)
      inner_func() ---> X

      # 중첨함수를 함수 밖에서도 호출할 수 있는 방법이 있다. 이 방법을 이해하기 위해 First-class function, closure에 대해 알아야한다.
      
      #input:
      def outer_func(num):
          # 중첩 함수에서 외부 함수의 변수에 접근 가능
          def inner_func():
              print(num)
              return 'complex'
          return inner_func
      
      fn = outer_func(10) # <--- First-class function
      print(fn())         # <-- Closure 호출

      output:
      10
      complex



## First-class function

### First-class 함수
- 다음과 같이 다룰 수 있는 함수를 First-class 함수라고 부름
   - 함수 자체를 변수에 저장 가능
   - 함수의 인자에 다른 함수를 인수로 전달 가능
   - 함수의 반환 값(return 값)으로 함수를 전달 가능

### 파이썬과 First-class 함수
- 사실 파이썬에서는 모든 것이 객체
- 파이썬 함수도 객체로 되어 있어서, 기본 함수 기능 이외 객체와 같은 활용이 가능
  - 즉, 파이썬 함수들은 First-class 함수로 사용 가능
  


        # 다른 변수에 함수 할당 가능
        input:    
        def calc_square(digit):
            return digit * digit
        
        # func1이라는 변수에 함수를 할당 가능
        # func1이라는 변수는 calc_square 함수를 가리키고, calc_square와 마찬가지로 인자도 넣어서 결과도 얻을 수 있음(완전 calc_square과 동일)
        func1 = calc_square
        
        print(func1)
        func1(2)
        
        output:
        <function calc_square at 0x0000016E07B4CE50>
        4
  
  
        # 함수를 다른 함수의 인자로 넣을 수도 있음
        input:
        def calc_square(digit):
            return digit * digit
        
        def calc_plus(digit):
            return digit + digit
        
        def calc_quad(digit):
            return digit * digit * digit * digit
  
        def list_square(function, digit_list):
            result = list()
            for digit in digit_list:
                result.append(function(digit))
            print(result)
  
        num_list = [1, 2, 3, 4, 5]
  
        list_square(calc_square, num_list)
        list_square(calc_plus, num_list)
        list_square(calc_quad, num_list)
      
        output:
        [1, 4, 9, 16, 25]
        [2, 4, 6, 8, 10]
        [1, 16, 81, 256, 625]
  
        
        # 함수의 결과값으로 함수를 리턴할 수도 있음
        input:
        def logger(msg):
            message = msg
            def msg_creator(): # <-- 함수 안에 함수를 만들 수도 있음
                print('[HIGH LEVEL]:', message)
            return msg_creator
  
        log1 = logger('Hun Log-in')
  
        log1()
          
        output:
        [HIGH LEVEL]: Hun Log-in
  
        # logger 함수를 삭제해도 log1() 함수는 logger 함수 안에 있는 msg_creator 함수와 msg 값을 유지
        input:
        del logger
  
        log1()
        
        output:
        [HIGH LEVEL]: Hun Log-in
  
        
        # First-class 함수 활용
        input:
        def html_creator(tag):
            def text_wrapper(msg):
                print('<{0}>{1}</{0}>'.format(tag, msg))
            return text_wrapper
  
        h1_html_creator = html_creator('h1')
  
        h1_html_creator('H1 태그는 타이틀을 표시하는 태그입니다.')
  
        output:
        <h1>H1 태그는 타이틀을 표시하는 태그입니다.</h1>
  
        input:
        p_html_creator = html_creator('p')
        p_html_creator('P 태그는 문단을 표시하는 태그입니다.')
  
        output:
        <p>P 태그는 문단을 표시하는 태그입니다.</p>
  
        input:
        def index_creator(index):
            def table_of_contents(content_list):
                for content in content_list:
                    print('{0} {1}'.format(index, content))
            return table_of_contents
  
        data_list_minus = index_creator('-')
        data_list_minus(['등운동', '가슴운동','다리운동'])
        
        data_list_mul = index_creator('*')
        data_list_mul(['팔운동', '어깨운동', '전완근 운동'])
  
        output:
        - 등운동
        - 가슴운동
        - 다리운동
        * 팔운동
        * 어깨운동
        * 전완근 운동

## closure function
- 함수와 해당 함수가 가지고 있는 데이터를 함께 복사, 저장해서 별도 함수로 활용하는 기법으로 First-class 함수와 동일
- 외부 함수가 소멸되더라도, 외부 함수 안에 있는 로컬 변수 값과 중첩함수(내부함수)를 사용할 수 있는 기법
- closure_func가 바로 closure임
- closure_func = outer_func(10)에서 outer_func 함수는 호출 종료
- closure_func()은 결국 inner_func 함수를 호출
- outer_func(10) 호출 종료시 num 값은 없어졌으나, closure_func()에서 inner_func이 호출되면서 이전의 num값(10)을 사용함.


      input:
      def outer_func(num):
          # 중첩 함수에서 외부 함수의 변수에 접근 가능
          def inner_func():
              print(num)
              return '안녕'
          
          return inner_func    # 중첩 함수 이름을 리턴

      closure_func = outer_func(10)   # <--- First-class function
      closure_func()                  # <--- Closure 호출

      output:
      10
      '안녕'
      

      # 심지어 outer_func 함수를 아예 삭제해버려도 fn(), 즉 inner_func()와 num값(10)은 살아있음
      input:
      del outer_func

      closure_func()

      output:
      10
      '안녕'

### 언제 closure를 사용할까?
- closure은 객체와 유사
- 일반적으로 제공해야할 기능(method)이 적은 경우,closure를 사용하기도 함
- 제공해야할 기능(method)가 많은 경우등은 class를 사용하여 구현

      

      input:
      def calc_power(n):
          def power(digit):
              return digit ** n
          return power
      
      power2 = calc_power(2)
      power3 = calc_power(3)
      power4 = calc_power(4)
      
      print(power2(2))
      print(power3(3))
      print(power4(4))
      
      output:
      4
      27
      256

      input:
      list_data = list()
      for num in range(1,6):
          list_data.append(calc_power(num))
      
      for func in list_data:
          print(func(2))

      output:
      2
      4
      8
      16
      32

## 데코레이터(Decorator)
- 함수 앞뒤에 기능을 추가해서 손쉽게 함수를 활용할 수 있는 기법
- Closure function을 활용


      input:
      import datetime
      
      def logger_login():
          print(datetime.datetime.now())
          print("Hun login")
          print(datetime.datetime.now())
          
      logger_login()

      output:
      2021-07-14 12:10:20.862073
      Hun login
      2021-07-14 12:10:20.862073


### 데코레이터 작성법

      
      # 데코레이터 작성하기
      def datetime_decorator(func):                          # <--- datetime decorator는 데코레이터 이름, func가 이 함수 안에 넣을 함수가 됨
          def wrapper():                                     # <--- 호출할 함수를 감싸는 함수
              print('time ' + str(datetime.datetime.now()))  # <--- 함수 앞에서 실행할 내용
              func()                                         # <--- 함수
              print(datetime.datetime.now())                 # <--- 함수 뒤에서 실행할 내용
          return wrapper                                    # <--- closure 함수로 만든다.

      input:
      @datetime_decorator   # @데코레이터
      def logger_login_kevin():
          print("kevin login")
          
      logger_login_kevin()

      output:
      time 2021-07-14 12:15:52.783625
      Hun login
      2021-07-14 12:15:52.783625

      input:
      @datetime_decorator   # @데코레이터
      def logger_login_kevin():
          print("kevin login")
          
      logger_login_kevin()

      output:
      time 2021-07-14 12:17:11.242653
      kevin login
      2021-07-14 12:17:11.242653


## Nested function, Closure function과 함께 데코레이터를 풀어서 작성

      
      # decorator 함수 정의
      
      def outer_func(function):
          def inner_func():
              print('decoration added')
              function()
          return inner_func
      
      # decorating할 함수
      def log_func():
          print('logging')

      # 본래 함수
      input:
      log_func()

      output:
      logging


      # log_func 함수에 inner_func 함수의 기능을 추가한 decorated_func 함수
      input:
      decorated_func = outer_func(log_func)
      decorated_func() # <--- 결과는 데코레이터를 사용할 때와 동일함

      output:
      decoration added
      logging



      # 이것을 한번에 데코레이터로 작성하면
      # log_func 함수는 outer_func함수의 인자로 들어가서 해당 함수가 출력된다.
      input:
      @outer_func
      def log_func():
          print('logging')
          
      log_func()
      
      output:
      decoration added
      logging

### 파라미터가 있는 함수에 Decorator 적용하기
- 중첩함수에 꾸미고자 하는 함수와 동일하게 파라미터를 가져가면 됨


      # 데코레이터
      input:
      def outer_func(function):
          def inner_func(digit1, digit2):
              if digit2 == 0:                          # <-- 유효성 검사의 예
                  print('cannot be divided with zero')
                  return
              return function(digit1, digit2)
          return inner_func
      
      # 실제 함수
      def divide(digit1, digit2):
          return digit1 / digit2
      
      # 데코레이터 사용하기(유효성 검사)
      @outer_func
      def divide(digit1, digit2):
          return digit1 / digit2
      
      divide(4, 2)
      
      output:
      2.0
      
      input:
      divide(9, 0)
      
      output:
      cannot be divided with zero
      
      
      input:
      def type_checker(function):
          def inner_func(digit1, digit2):
              if(type(digit1) != int) or (type(digit2) != int):
                  print('only integer support')
                  return
              return function(digit1, digit2)
          return inner_func
      
      @type_checker
      def multiplexer(digit1, digit2):@ㅎ
          return digit1 * digit2
      
      multiplexer(1.1, 2)

      output:
      only integer support


### 파라미터와 관계없이 모든 함수에 적용 가능한 Decorator 만들기
- 파라미터는 어떤 형태이든 결국 (*args, **kwargs)로 표현 가능
- 데코레이터의 내부함수 파라미터를 (*args, **kwargs)로 작성하면 어떤 함수이든 데코레이터 적용 가능

      
      # 데코레이터 작성하기
      def general_decorator(function):
          def wrapper(*args, **kwargs):
              print('function is decorated')
              return function(*args, **kwargs)
          return wrapper
      
      # 데코레이터 적용하기
      @general_decorator
      def calc_square(digit):
          return digit * digit
      
      @general_decorator
      def calc_plus(digit1, digit2):
          return digit1 + digit2
      
      @general_decorator
      def calc_quad(digit1, digit2, digit3, digit4):
          return digit1 * digit2 * digit3 * digit4
      
      # 여러 데코레이터를 함수에 한번에 적용하기
      
      @decorator1
      @decorator2
      def hello():
          print('hello')
          
      hello()
      
      output:
      decorator1
      decorator2
      hello

      input:
      def mark_bold(function):
          def wrapper(*args, **kwargs):
              return '<b>' + function(*args, **kwargs) + '</b>'
          return wrapper
      
      def mark_italic(function):
          def wrapper(*args, **kwargs):
              return '<i>' + function(*args, **kwargs) + '</i>'
          return wrapper
      
      @mark_bold
      @mark_italic
      def add_html(string):
          return string
      
      print(add_html('안녕하세요'))
      
      output:
      <b><i>안녕하세요</i></b>


## Method Decorator
- 클래스의 method에도 데코레이터 적용 가능
  - 클래스 method는 첫 파라미터가 sslf이므로 이 부분을 데코레이터 작성시에 포함시켜야 함
  
            
            # 데코레이터 작성하기 (for method)
            def h1_tag(function):
                def func_wrapper(self, *args, **kwargs):                          # <--- self를 무조건 첫 파라미터로 넣어야 메서드 적용 가능 
                    return "<h1>{0}</h1>".format(function(self, *args, **kwargs)) # <--- function 함수에도 self를 넣어야 함
                return func_wrapper
            
            # 데코레이터 적용 확인해보기
            kevinseo = Person('Seo', 'Kevin')
            print(kevinseo.get_name())
            
            output:
            <h1>Seo Kevin</h1>


### 파라미터가 있는 Decorator 만들기 (심화)
- decorator에 파라미터를 추가 가능

            
            # 중첨 함수의 하나 더 깊게 두어 생성
            def decorator1(num):
                def outer_wrapper(function):
                    def inner_wrapper(*args, **kwargs):
                        print('decorator1 {}'.format(num))
                        return function(*args, **kwargs)
                    return inner_wrapper
                return outer_wrapper
            
            def print_hello():
                print('hello')
            
            # 위와 같이 작성하면, 다음과 같이 호출할 수 있다.
            
            print_hello = decorator1(1)(print_hello)
            print_hello()
            
            output:
            decorator1 1
            hello
            
            # 이를 데코레이터로 표현하면 다음과 같다.
            @decorator1(1)
            def print_hello():
                print('hello')
                
            print_hello()
            
            output:
            decorator1 1
            hello


#### 연습

            def mark_bold(function):
                def wrapper(*args, **kwargs):
                    return '<b>' + function(*args, **kwargs) + '</b>'
                return wrapper
            # 이를 데코레이터로 표현하면 다음과 같다.
            @decorator1(1)
            def print_hello():
                print('hello')
                
            print_hello()
            def mark_italic(function):
                def wrapper(*args, **kwargs):
                    return '<i>' + function(*args, **kwargs) + '</i>'
                return wrapper
            
            def mark_h1(function):
                def wrapper(*args, **kwargs):
                    return '<h1>' + function(*args, **kwargs) + '</h1>'
                return wrapper
            
            def mark_h2(function):
                def wrapper(*args, **kwargs):
                    return '<h2>' + function(*args, **kwargs) + '</h2>'
                return wrapper
            
            def mark_h3(function):
                def wrapper(*args, **kwargs):
                    return '<h3>' + function(*args, **kwargs) + '</h3>'
                return wrapper
            
            def mark_h4(function):
                def wrapper(*args, **kwargs):
                    return '<h4>' + function(*args, **kwargs) + '</h4>'
                return wrapper
            
            def mark_h5(function):
                def wrapper(*args, **kwargs):
                    return '<h5>' + function(*args, **kwargs) + '</h5>'
                return wrapper
            
            def mark_h6(function):
                def wrapper(*args, **kwargs):
                    return '<h6>' + function(*args, **kwargs) + '</h6>'
                return wrapper
            
            def mark_center(function):
                def wrapper(*args, **kwargs):
                    return '<center>' + function(*args, **kwargs) + '</center>'
                return wrapper
            
            
            @mark_bold
            @mark_italic
            @mark_h1
            @mark_h2
            @mark_h3
            @mark_h4
            @mark_h5
            @mark_h6
            @mark_center
            
            def add_html(string):
                return string
            
            add_html('안녕하세요')
            
            output:
            '<b><i><h1><h2><h3><h4><h5><h6><center>안녕하세요</center></h6></h5></h4></h3></h2></h1></i></b>'
            
            def mark_html(tag):
                def outer_wrapper(function):
                    def inner_wrapper(*args, **kwargs):
                        return '<' + tag + '>' + function(*args, **kwargs) + '</' + tag + '>'
                    return inner_wrapper
                return outer_wrapper
            
            @mark_html('b')
            @mark_html('i')
            @mark_html('h1')
            @mark_html('h2')
            @mark_html('h3')
            @mark_html('h4')
            @mark_html('h5')
            @mark_html('h6')
            @mark_html('center')
            def add_html(string):
                return string
            
            add_html('안녕하세요')
            
            output:
            '<b><i><h1><h2><h3><h4><h5><h6><center>안녕하세요</center></h6></h5></h4></h3></h2></h1></i></b>'