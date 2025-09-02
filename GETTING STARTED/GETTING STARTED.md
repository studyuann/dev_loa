You asked for a translation of the provided text about the Lost Ark Open API. Here is the translated content.

시작하기
로스트아크 오픈 API를 사용하려면, 아래의 기본 절차들을 따라 API의 작동 방식을 이해할 수 있습니다.

로그인
로스트아크 오픈 API를 사용하기 전에, Stove.com 계정에 로그인해야 합니다. 아직 계정이 없다면, 여기에서 가입하세요. 로스트아크 오픈 API 웹사이트에 가입하기 위한 추가 단계는 필요 없으며, 스토브 가입만으로 충분합니다. 로그인하면 즉시 새로운 클라이언트를 만들 수 있습니다.

클라이언트 생성
클라이언트를 생성하려면 다음 단계를 따르세요.

'CREATE A NEW CLIENT' 버튼 또는 이 링크를 클릭합니다.

Client Name 필드에 클라이언트 목록에서 식별할 이름을 입력합니다. 이 이름은 사용자 계정에만 연결됩니다.

Client URL 필드에 이 클라이언트를 사용할 서비스의 URL을 입력합니다.

Client Description 필드에 애플리케이션이 무엇을 하는지 설명합니다.

이용 약관 및 개인정보 처리방침을 읽고 동의합니다. (이 단계는 필수가 아닙니다.)

클라이언트 생성 후, MY CLIENTS 페이지 또는 오른쪽 상단 모서리의 빨간색 신원 버튼을 클릭하여 나타나는 팝업에서 세부 정보를 확인할 수 있습니다.

JWT 키
클라이언트를 생성하면, JWT(JSON Web Token)가 즉시 발급됩니다. OAuth 보안 계층이 있지만, 만료된 토큰을 새로고침하거나 새로운 액세스 토큰을 요청하는 번거로운 작업에 대해 걱정할 필요가 없습니다.

발급된 보안 토큰은 키가 안전하지 않다고 판단되거나 사용자가 클라이언트를 삭제하지 않는 한 영구적으로 유효합니다. JWT 토큰은 신뢰할 수 있는 서버 측 애플리케이션과 같이 안전한 장소에 저장하는 것이 좋습니다.

예를 들어, "abcdefghijklmnopqrstuvwxyz"라는 토큰을 받았다면, 권한 부여 헤더는 다음과 같이 설정해야 합니다.

"Authorization: bearer abcdefghijklmnopqrstuvwxyz" (O) => 유효

"Authorization: abcdefghijklmnopqrstuvwxyz" (X) => 유효하지 않음 ( "bearer" 누락)

"Authorization: bearer {abcdefghijklmnopqrstuvwxyz}" (X) => 유효하지 않음

"Authorization: bearer <abcdefghijklmnopqrstuvwxyz>" (X) => 유효하지 않음

"Authorization: bearer{abcdefghijklmnopqrstuvwxyz}" (X) => 유효하지 않음

"Authorization: bearerabcdefghijklmnopqrstuvwxyz" (X) => 유효하지 않음

API 문서
로스트아크 오픈 API 문서는 실제 구현 로직 없이도 API 리소스와 상호 작용할 수 있게 해줍니다. 제공되는 내용을 탐색하고 JWT를 사용하여 실제 요청을 만들 수 있습니다. 항상 "Try it out" 버튼을 클릭하여 "Execute" 버튼을 활성화해야 요청 설정이 완료되고, API를 호출하며 결과를 받을 수 있습니다.

로스트아크 오픈 API를 구현하기 전에 요청 매개변수를 탐색하고 응답을 확인하면 상당한 시간과 노력을 절약할 수 있습니다.

Swagger 인증
로스트아크 오픈 API 문서에서 API를 인증하고 호출하려면 다음과 같이 JWT를 전달해야 합니다.

AUTHORIZE 버튼을 클릭합니다.

VALUE 필드에 JWT를 입력합니다.

토큰의 성공적인 유효성 검사를 위해 위에 제시된 잘못된 예시들을 피해야 합니다.

가능한 인증
apiKeyAuth (apiKey)
대화형 문서를 사용하기 위해 API JWT를 여기에 입력하세요.
예: bearer JWT

유효한 토큰인데도 인증에 실패하면, 잘못된 예시들을 다시 검토하세요.

잘못된 사례들을 피해야 합니다.

이름: authorization

위치: header

값:

스로틀링 (요청 제한)
클라이언트는 분당 100회 요청으로 제한됩니다. 이 할당량을 초과하면 할당량이 재설정될 때까지 429 응답을 받게 됩니다. 할당량은 매분 자동으로 갱신되므로, 한도에 도달한 후 1분이 지나면 애플리케이션이 다시 작동해야 합니다.

응답 헤더의 스로틀링 메타데이터
429 응답은 여러분에게 무의미하고 좌절감을 줄 수 있습니다. 클라이언트 애플리케이션을 제어하세요! 아래의 메타데이터가 여러분에게 유용할 것입니다.

X-RateLimit-Limit: 분당 요청 수 (정수)

X-RateLimit-Remaining: 클라이언트에 사용 가능한 남은 요청 수 (정수)

X-RateLimit-Reset: 다음 할당량 갱신까지의 에포크 시간 (정수)
