# 19.09.30

- Preview.vue 기본 컴포넌트 생성
- msg 저장(mongoose update)
- getChat 하다맘(테스트해서 확인 후 조치할 것)



# 19.10.01

- msg 저장 09.30꺼는 에러남(다시 함)

- 채팅 백업(getChat), 파일 저장 구현(db_utils.js::save)

- 내일 할일

  - BasicChat.vue에서 Work.vue 띄워야함

  - import로 선언 안하고 Vuex로 전달하는것 시도해봤으나 실패

    - import Work ... 로 선언하자




# 19.10.02

- modal로 창 띄움
- db/work 구현
  - 단순히 파일저장용도
  - sender인 Work.vue에서 파일과, works(title, contents, date 등)를 설정하여 db/work로 요청
  - db/work는 파일을 저장한 뒤, saveName과 realName을 works에 포함시켜서 응답
  - Work.vue는 받은 works를 bus emit(work)로 BasicChat.vue에 전달 
  - BasicChat.vue는 bus on(work)로 works를 수신하여 일반적인 msg와 동일하게 처리
    - 주의할 점은, receiver가 배열(여러명 가능)이므로, 각 수신자에게 동일한 메시지 처리를 해야한다.
- 큰일남.. multer 오류남.
  - 다음에 할 때, multer 기본 파일 수신 코드로 바꾼뒤 테스트해야함 하..



# 19.10.04

- multer 오류 해결

  - dictionary 전달이 안됨, 각 요소를 따로 append 해줘야함

    ```js
    fd.append('title', this.v_work.title)
    fd.append('selects', this.v_work.selects)
    fd.append('contents', this.v_work.contents)
    fd.append('startDate', this.v_work.startDate)
    fd.append('endDate', this.v_work.endDate)
    fd.append('notice', this.v_work.notice)
    ```

- WorkCard.vue 구현

  - 기본 틀 구성
  - 

- `state_work`, `state_notice` 추가

  - 알림은 "미확인", "확인", "확인-대기" 으로 구성

    - 받은 사람이 확인했을 경우 "확인"으로 변경가능하며, 이 때 코멘트를 남길 수 있음

    - 여기에 보낸 사람이 응답할시 "확인-대기"로 변경되며, "확인-대기"는 버튼 색깔이

      주황색(미정)으로 표시됨(미확인한게 있다는 의미로)

  - 작업은 "미제출", "승인대기", "승인거절", "승인완료"로 구성

    - "미제출" - 빨강, 응답 대기상태
    - "승인대기" - 주황색, 응답했지만 송신자가 확인안한 상태
    - "승인거절" - 연빨강(?), 응답한 자료가 거절되어 다시 제출해야 함
      - 송신자의 거절사유를 확인할 수 있음
    - "승인완료" - 초록색, 응답한 자료가 승인됨.  
      - 시간이 만료되거나, 수신자가 삭제할 경우 처리할일에서 지나간 일로 등록됨
        - 지나간 일을 구분하기 위한 상태를 추가  - 하자

- multiple receiver 처리

  - receiver 배열요소 각각에게 모두 메시지 전송

    - 발생문제

      - file 전송시 배열은 문자열로 치환된다... (['계획과장',''군수과장'] -> '계획과장,군수과장')
        - split해서 배열로 만들어줘야함..
      - 배열은 forEach시 value가 인자로 들어가네??
        - 지난번엔 idx가 들어가더니 하
        - 별로 중요한거 아님
      - 이상한 에러가 발생한다. 백엔드 서버에서.. 해결해야 됨




# 10.05

- multiple receiver 에러 해결

  - `notice` 자료형이 boolean인데 work등록시 true/false 전달하는게 문자열로 변환되서

    에러가 난것이었다.

    - 다시 boolean으로 변경해주어 해결

      ```js
      notice: data.notice === 'true',
      ```

- 여러명에게 보낼 때, 보낸 메시지가 모두 지금 채팅방에 뜨고, 채팅방 다시열어야 사라지던거 해결

  - setInput처리에서 보낸 메시지 모두 `feed`에 push하도록 만들어서 채팅방에 모든 메시지가

    다 입력된 것이었다.

    - 지금 열린 채팅방 대상일 때만 채팅방에 msg가 push되도록 변경

      ```js
            if(to === this.title)
              this.onNewOwnMessage(msg)
          },
      ```

- 상태 추가
  - state_notice : '미확인', '확인', '확인-대기'
  - state_work : '미제출', '승인대기', '승인거절', '승인완료'
  
- 채팅/등록창에서 작업/알림 구분 표시

- 부서에게 전송

  - 확인해봐야함

- 부서, 사람 따로 검색하던거 합쳐서 검색하게 만듬

- 파일 다운로드 링크 해야함!