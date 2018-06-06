AJAX 자바스크립트/제이쿼리
====================

1. AJAX : 비동기적 자바스크립트 & XML. 웹 브라우저와 웹 서버거 비동기적으로 데어터 통신을 하는 행위를 의미한다. 현재는 XML보다는 JSON이 더 선호되는 데이터 형식이라고...



2. javascript 이용

```
    function startAjax() {
        var xhr = new XMLHttpRequest();
        xhr.open("GET", "https://dog.ceo/api/breeds/image/random");
        xhr.onreadystatechange = function () {
            if(this.readyState === 4 && this.status === 200) {
                var result = JSON.parse(this.responseText);
                document.querySelector("img[alt='doggy']").setAttribute("src", result.message);
            }
        }
        xhr.send();
    }
    document.querySelector("button").addEventListener("click", startAjax);
```

XMLHttpRequest 객체는 이름과는 달리 xml뿐만 아니라 다른 형식의 데이터도 비동기적으로 처리할 때 사용할수 있는 객체이다.  
onreadystatechange 는 readyState 속성(0: request not initialized, 1: server connection established, 2: request received ,3: processing request ,4: request finished and response is ready) 이 바뀔 때마다 실행되는 콜백 함수를 할당해주면 된다.  
responseText 속성은 서버에서 받아온 데이터를 텍스트 형식으로 리턴해준다.  
마지막에 .send()메소드를 실행 하지 않으면 요청이 서버로 전달되지 않으니 주의!  



3. jquery 이용

```
    $("button").click(function () {
        $.ajax({
            url: "https://dog.ceo/api/breeds/image/random",
            dataType: 'json',
            success: function (data) {
                $("img[alt='doggy']").attr("src", data.message);
            }
        });
    });
```

$.ajax() 메소드는 제이쿼리로 ajax통신을 실행하는 가장 기본이 되는 메소드. 유일한 인자로 객체를 받으며 객체의 키:값 을 통해 여러 옵션들을 설정해줄 수 있다.  
type: 은 해당 요청의 타입을 알려주며 명시하지 않으면 GET방식이 default이다.  
data: 는 위에는 없으나 POST타입으로 서버에 데이터를 전달해 줄 경우 해당 데이터를 할당해주는 속성이다.  
dataType: 은 서버와의 통신의 결과로 받아온 데이터의 타입을 제이쿼리에 알려주는 속성. 위에서 json타입의 데이터라고 알려 줬으므로 우리는 콜백함수에서 data를 json형식으로 변환해 줄 필요가 없다!  
success: 서버와의 통신에 성공했을 경우 호출되는 콜백 함수를 전달하는 속성. 첫번째 인자로 리턴 데이터를 받는다.  
$.ajax() 메소드는 실행될 때 바로 요청을 전달하기 때문에 .send()같은 메소드를 따로 적어줄 필요가 없다.