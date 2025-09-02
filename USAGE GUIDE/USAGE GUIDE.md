로스트아크 Open API 사용 가이드 번역
사용 가이드
이 가이드는 로스트아크 Open API와 상호 작용하는 방법을 설명합니다. 소프트웨어 개발자를 대상으로 하며 구현 세부 정보 및 샘플 코드를 안내합니다.

기본 HTTP
로스트아크 Open API 서비스에 유효한 HTTP 요청을 만드는 데는 세 가지가 포함됩니다:

올바른 HTTP 동사를 설정합니다.

accept 헤더에 application/json을, 필요한 경우 content-type 헤더도 설정합니다.

authorization 헤더에 베어러 토큰을 설정합니다.

모든 API 요청에는 항상 JWT가 포함되어야 합니다. authorization 헤더에 이를 지정하십시오.

cURL 예시
curl -X 'GET' 'https://developer-lostark.game.onstove.com/exmaple/api' 
-H "accept: application/json" 
-H "authorization: bearer your_JWT"
Javascript 예시
JavaScript

var xmlHttpRequest = new XMLHttpRequest();
xmlHttpRequest.open("GET", "https://developer-lostark.game.onstove.com/exmaple/api", true);
xmlHttpRequest.setRequestHeader('accept', 'application/json');
xmlHttpRequest.setRequestHeader('authorization', 'bearer your_JWT');
xmlHttpRequest.onreadystatechange = () => { };
xmlHttpRequest.send();
로스트아크 Open API GET 요청 예시
GET /guilds/rankings API는 요청 매개변수로 'serverName' 문자열을 받습니다. 이는 쿼리 문자열이므로 다음과 같이 요청 URL을 구성합니다:

쿼리 문자열 매개변수 cURL 예시
curl -X 'GET' 'https://developer-lostark.game.onstove.com/guilds/rankings?serverName=%EB%A3%A8%ED%8E%98%EC%98%A8'
-H 'accept: application/json'
-H 'authorization: bearer your_JWT
쿼리 문자열 매개변수 Javascript 예시
JavaScript

var xmlHttpRequest = new XMLHttpRequest();
xmlHttpRequest.open("GET", "https://developer-lostark.game.onstove.com/guilds/rankings?serverName=%EB%A3%A8%ED%8E%98%EC%98%A8", true);
xmlHttpRequest.setRequestHeader('accept', 'application/json');
xmlHttpRequest.setRequestHeader('authorization', 'bearer your_JWT');
xmlHttpRequest.onreadystatechange = () => { };
xmlHttpRequest.send();
GET /armories/characters/{characterName}/profiles API는 요청 매개변수로 'characterName' 문자열을 받습니다. 이는 경로 문자열이므로 다음과 같이 요청 URL을 구성해야 합니다:

경로 매개변수 cURL 예시
curl -X 'GET' 'https://developer-lostark.game.onstove.com/armories/characters/coolguy/profiles'
-H 'accept: application/json'
-H 'authorization: bearer your_JWT'
경로 매개변수 Javascript 예시
JavaScript

var xmlHttpRequest = new XMLHttpRequest();
xmlHttpRequest.open("GET", "https://developer-lostark.game.onstove.com/armories/characters/coolguy/profiles", true);
xmlHttpRequest.setRequestHeader('accept', 'application/json');
xmlHttpRequest.setRequestHeader('authorization', 'bearer your_JWT');
xmlHttpRequest.onreadystatechange = () => { };
xmlHttpRequest.send();
로스트아크 Open API POST 요청 예시
POST /auction/items API는 POST 바디로 'requestAuctionItems' 객체를 필요로 합니다. cURL에서 -d로 객체 바디를 지정할 수 있습니다.

POST 바디 cURL 예시
curl -X 'POST' 'https://developer-lostark.game.onstove.com/markets/items'
  -H 'accept: application/json'
  -H 'authorization: bearer your_JWT'
  -H 'Content-Type: application/json'
  -d '{
  "CategoryCode": 20000
}'
POST 바디 Javascript 예시
JavaScript

var xmlHttpRequest = new XMLHttpRequest();
xmlHttpRequest.open("POST", "https://developer-lostark.game.onstove.com/markets/items", true);
xmlHttpRequest.setRequestHeader('accept', 'application/json');
xmlHttpRequest.setRequestHeader('authorization', 'bearer your_JWT');
xmlHttpRequest.setRequestHeader('content-Type', 'application/json');
xmlHttpRequest.onreadystatechange = () => { };
xmlHttpRequest.send(JSON.stringify({"CategoryCode" : 20000}));
API 오류
로스트아크 Open API는 기본 HTTP 오류를 반환합니다. 아래 표를 참조하십시오.

코드	설명
200	OK
401	Unauthorized (권한 없음)
403	Forbidden (금지됨)
404	Not Found (찾을 수 없음)
415	Unsupported Media Type (지원되지 않는 미디어 유형)
429	Rate Limit Exceeded (요청 한도 초과)
500	Internal Server Error (내부 서버 오류)
502	Bad Gateway (잘못된 게이트웨이)
503	Service Unavailable (서비스를 사용할 수 없음)
504	Gateway Timeout (게이트웨이 시간 초과)

Sheets로 내보내기
점검 시간 중 API 요청
점검 중에는 웹사이트 전체에 점검 페이지가 표시되며, 요청은 503 Service Unavailable HTTP 상태 코드를 반환합니다. 이 상태 코드를 받으면 추가 요청을 자제하거나 요청 간격을 조정하는 것이 좋습니다.

비효율적인 API 요청
요청 할당량을 효과적으로 활용하세요! 제한(throttling)으로 인해 애플리케이션에 강력한 캐싱 전략을 구현하는 것이 필수적입니다. 게임 내 데이터의 효율적인 관리가 중요합니다. 특정 데이터 세트의 경우 하루에 한 번 또는 일주일에 한 번만 API를 호출하면 될 수 있습니다. 예를 들어:

GET /news/events

GET /auctions/options

GET /markets/options

이러한 API의 응답 데이터는 현재 리소스나 응답 모델을 변경하는 예기치 않은 점검이 없는 한 일반적으로 정적입니다. 지속적으로 요청을 생성하여 할당량을 초과하면 애플리케이션이 제한되어 할당량 새로 고침을 기다려야 합니다. 마이크로초 간격으로 과도한 폴링을 하거나 각 사용자 상호 작용에 의해 중복 요청이 발생하는 것을 자제하는 것이 좋습니다.

API 요청 한도
요청 한도, 남은 요청 수, 다음 할당량 새로 고침 시간을 나타내는 카운터와 타임스탬프를 제공하여 클라이언트 요청 속도 관리를 용이하게 합니다.

참조 응답 헤더
응답 헤더 키	응답 헤더 값	설명
X-RateLimit-Limit	100	분당 요청 수
X-RateLimit-Remaining	15	클라이언트에서 사용 가능한 남은 요청 수
X-RateLimit-Reset	1668659557	할당량의 다음 새로 고침을 위한 UNIX 타임스탬프

Sheets로 내보내기
API 상태
API 상태는 5가지가 있습니다.

⚪ 서버가 상태 메타데이터를 가져오는 중입니다.

🔴 서버가 작동하지 않습니다.

🟡 서버가 부분적으로 작동합니다.

🟢 서버가 완전히 작동합니다.

🔵 서버가 점검 중입니다.

API 버전 관리
자주 **변경 로그(changelog)**를 확인하여 최신 상태를 유지하십시오.

다음과 같은 경우 버전을 증가시킵니다:

새로운 엔드포인트 추가

엔드포인트 제거

호환되지 않는 API 변경

사용 중단 (Deprecation)
API 엔드포인트, 응답 데이터의 속성/필드, 매개변수는 다양한 이유로 사용 중단될 수 있습니다.
