# [KECE449] Project 1 
이름: 최재은  학번: 2023171409  
[1] 과제 목표  
Node.js랑 Express를 활용해서 AI chatbot을 만드는 것입니다. 사용자가 Google 계정으로 log in 하면 질문을 입력할 수 있고, 그걸 Gemini API로 보내서 응답을 받아서 다시 보여주게 합니다.

[2] 파일별 기능 정리  
1) server.js  
- server 초기 설정을 위한 `setupExpress(app)` 함수에서는 static file 등록, session 설정, passport 초기화를 하고, `configureGoogleStrategy()`는 Google OAuth log in 처리를 합니다.  
  로그인에 성공하면 사용자 정보를 session에 저장하도록 하고, serialize/deserialize 설정도 같이 했습니다.  
- `handleGeminiPrompt(prompt)`는 사용자가 보낸 질문을 Gemini API에 POST 요청으로 보내고, 응답에서 답변 텍스트만 뽑아서 반환해주게 합니다.  
- `setupRoutes(app)`에서는  `/`의 경우 로그인을 했는지에 에 따라서 redirect 되며, `/auth/google` & `/auth/google/callback`로 Google login 처리하고, `/prompt`로 chatbot 화면, `/ask`로 프롬프트를 Gemini API에 전달, `/logout` → session 없애고 log out 합니다.

2) index.html  
- 로그인 화면으로, page는 중앙 정렬로 디자인했고, Google log in 버튼을 누르면 `/auth/google`로 연결이 되어서 인증이 시작됩니다.

3) prompt.html  
- 이는 로그인된 사용자만 접근할 수 있는 chatbot 화면으로, 입력창에 질문을 쓰고 "Send" 버튼을 누르면 `/ask?q=...`으로 요청이 가며 그에 따른 Gemini의 응답은 아래 박스에 표시합니다.  
- 우측 상단에는 log out 버튼이 있고, 클릭 시 `/logout`으로 연결됩니다.  
- JavaScript에서는 예외 처리로 예를 들어 아무 것도 입력하지 않고 전송하면 아무 반응이 없고, 서버 오류가 생기면 "Error: ~" 같이 에러를 빨간 글씨로 표시합니다.

+) 기타  
- CSS에서는 전체적으로 부드러운 pink 계열 색상을 사용했고, log out 버튼은 `position: absolute`로 오른쪽 위에 항상 고정되도록 처리했습니다.
