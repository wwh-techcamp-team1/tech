# Commit Log 작성 규칙

- 개조식(상세는 줄글)으로 작성
- 제목은 영어로, 상세 내용은 한글로 작성한다.
- 구현된 기능에 대해 어떤 파일의 어떤 내용을 수정, 생성했는지 기술한다.
- 커밋 뒤에 해당 기능에 대한 이슈 번호를 붙인다.

- - -

- 예시 1:

 로그인 기능 추가 및 페이지 로딩 오류 수정됨. #14(이슈 번호)
  * 로그인 기능 추가
    - a.java : ...구현
    - b.java : ...구현
  * 페이지 로딩 오류 수정
    - c.java(new) : ... 구현

- - -

- 예시 2:

implement login feature #14(이슈번호)

스프링 스큐리티를 이용해 로그인 기능을 구현했습니다.

스프링 시큐리티를 이용한 로그인 기능 구현

 * 필터를 통해 들어온 요청에 대해 처리
   - AFilter.java : A필터 구현

 * 프로바이더 구현
   - AProvider.java : AProvider 구현 ~~ 를 이용해 했다.

 * SecurityConfig 작성
   - SecurityConfig.java: ProviderManager 등록, session 설정 추가
