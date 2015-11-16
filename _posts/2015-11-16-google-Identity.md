###Google Identity

- 사건의 발단은 https://apis.google.com 을 사용하려다가, 아래 에러 발생 
- C:\01.NodeJs\11.ChromeEx\00.note\RefusedLoadScript.png
- Loading Google API Javascript Client Library into Chrome Extension
  http://stackoverflow.com/questions/18681803/loading-google-api-javascript-client-library-into-chrome-extension

- 위의 해결방법대로 manifest.json 에 "content_security_policy" 를 추가했음
- 근데.. 이 중간단계가 기억나지않는데, oauth2 가 생각대로 움직이지 않아! 어쩌지! 하다가 Identity 를 찾음.
  (위의 내용이 제대로 해결책이 되지 않았다는 얘기.)

1. manifest.json 에 아래와 같이 추가

{% highlight javascript %}
"permissions" : [ 
	... 생략 ... 
	"identity"  
], 
"oauth2": {
	"client_id": "", // 추가한 사용자인증정보 - OAuth 2.0 클라이언트ID (by Google Developers Console)
	"scopes": [      // 인증을 통해 사용하고자 하는 부분
		  "https://www.googleapis.com/auth/script.storage" 
		, "https://www.googleapis.com/auth/spreadsheets"
	]
}
{% endhighlight %}

2. popup.js 에 아래 작성
{% highlight javascript %}
// 호출부
chrome.identity.getAuthToken({ 'interactive': true }, checkAuthorization);
{% endhighlight %}	


{% highlight javascript %}
function checkAuthorization(token) {
	... 생략 ...

	// request 제대로 작성하지 않으면 Invalid JSON 에러 발생
	var request = {};
	request['function']   = "handleResponse";
	request['parameters'] = [saveData];
	request['devMode']    = true;

	// scriptId 는 script.google.com 의 파일-프로젝트 속성에서 확인
	var root = 'https://script.googleapis.com/v1/scripts/' + scriptId + ':run';

	$.ajax({
		url  : root,
		type : 'POST',
		dataType    : 'json',     
		contentType : "application/json; charset=utf-8",
		data: JSON.stringify(request),
		beforeSend: function (xhr) {
			// 이 부분을 작성해야 oauth2가 정상적으로 적용됩니다.
		    xhr.setRequestHeader('Authorization', 'Bearer ' + token);
		},
		success: function (obj) {
			alert('success');
		},
		error: function (error) { 
			alert('error' + error);
		},
	});
}
{% endhighlight %}	

- 이 방법을 통해서 정상적으로 success 까지 받음
- 단지 중간에 계속 error 는 나는데, 이유를 알 수 없고 계속 반복되는게 짜증나서 
  캐쉬 다 지우고 클라이언트ID 도 새로 받았더니 어느 순간 길이 터졌다고 하는....의미없는 뻘짓.

- 하다가 중간에 좀 신기했던건, 저 Identity 로 얻은 token 값은 gapi.client 을 실행할 떄 
  전혀 의미없는 값이 되어버린다. 
- gapi.auth.setToken(token); 란 게 있어서, 어떻게 연결되지않을까 했는데..
  둘 중 하나의 방법을 이용해야한다고, 아래에 나와있음. 
http://stackoverflow.com/questions/19075016/chrome-identity-and-youtube-v3-api

- 2015.11.17 추가
지금은 gapi.client 로 oauth2 가 정상작동 되고있습니다. 
지난주에 한 저 위의 모든 작업은 답없는 뻘짓으로.....아하하.

### Google API

- 정상작동되니까 이쪽도 정리해보지요... 

Chrome Extensionでpolymerを使う
http://qiita.com/asanoboy/items/a1b5a8dd83d5d8e47f75
 
1. Content Security Policy(CSP) 는 무엇?
 - Chrome Extension 에서는 CSP 제약이 다른 웹페이지보다 강하다 
 - eval() / new Function() 의 금지 
 - <script> 태그 내의 자바스크립트, <buttion onclick="..."> 등의 inline script 금지
 - Extension 외부의 스크립트 읽어들이기 금지
