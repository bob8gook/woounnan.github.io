# 문제

- debugme라는 경고창이 뜨고

- 소스코드를 보면 난독화된 자바스크립트가 보인다.

  

#  풀이

- 일단 beautify로 예쁘게 정리하고...
- 첫줄부터 흐름이라도 파악해보려고 했는데 도무지..
- 모르는 문법도 많고
  - 분서기 안된다...
- 경고창 뜨는것을 힌트로 해서 비교루틴을 찾았다.
  - if else로 나뉘는데
  - debugme가 뜨는 조건이 아닌 반대의 조건에서 실행되는 코드를 콘솔에서 실행시키면 
  - 끝

# 알게된 것

### 