

## 1. 제목
- 숫자 야구

## 2. 소개

- 사용자가 랜덤한 수를 입력하여 그 결과를 판별해 strike혹은 ball의 개수를 알려줍니다
사용자는 이 결과를 받아 랜덤한 3가지의 숫자를 맞추어 게임을 이기는 것이 목표입니다.

### 게임 규칙

- 입력할 수 있는 수의 범위는 1부터 9까지입니다.
- 우선 컴퓨터는 중복되지 않은 임의의 수 3개를 생성합니다.
- 임의의 수 3개는 게임 도중 변경되지 않습니다.
- 새로운 게임이 시작되면 임의의 수 3개는 변경합니다.
- 게임이 끝날 때 까지 사용자에게 중복된 수 없이 3개의 정수를 입력받습니다.
- 사용자는 9번의 기회 안에 3 스트라이크를 얻어내면 승리합니다.
- 9번의 기회 안에 3 스트라이크를 얻어내지 못하면 컴퓨터가 승리합니다.

## 3. 팀원
- Hisop, Kiseok

## 4. 타임라인: 시간 순으로 프로젝트의 주요 진행 척도를 표시
- 08.28 (월)
  - 프로젝트 룰 이해
  - STEP1 플로우 차트 작성
  - STEP1 기능 설계
- 08.29 (화)
  - STEP1 실행 예시와 출력이 동일하도록 구현
- 08.30 (수)
  - STEP1 PR 작성 / 코멘트받은 내용 기반으로 코드 수정
- 08.31 (목)
  - STEP2 플로우 차트 작성
  - STEP2 기능 설계
  - STEP2 PR 작성 / 코멘트받은 내용 기반으로 코드 수정
- 09.01 (금)
  - READ ME 작성

## 5. 플로우 차트
![number_baseball](https://github.com/Hi-sop/ios-number-baseball/assets/69287436/2575ffe1-0146-40a5-b097-d0a6cd0d5f86)

## 6. 실행 화면(기능 설명)
- 잘못된 메뉴 입력  
    ![잘못된 메뉴입력](https://github.com/Hi-sop/ios-number-baseball/assets/69287436/c7651622-9ec7-4f2b-9c3e-cd0b676b5e73)

- 잘못된 숫자가 입력된 경우  
    ![잘못된 숫자입력](https://github.com/Hi-sop/ios-number-baseball/assets/69287436/1920d46c-69ea-4cdc-9b3b-8f459395d3f2)

- 정상적인 게임 결과  
    ![정상적인 게임결과](https://github.com/Hi-sop/ios-number-baseball/assets/69287436/bc40f5bb-7a63-40d2-99cc-dd1bc70a964e)

## 7. 트러블 슈팅

- **사용자 입력 검증부 예외처리**  
    사용자 입력에 대한 검증이 필요했다. 통과조건은 스페이스를 기준으로 구분된 1 ~ 9 사이의 숫자 3개. ```ex) 1 2 3```
    - 입력된 값이 숫자인지 확인
    -> Readline으로 받아온 ```String```을 ```[Int]```로 변환하며 확인.
    이 때 map으로 시도하다가 실패하여 for문 사용. Int()를 통해 변환된 값은 옵셔널을 리턴했기 때문.
    - 입력된 값이 중복되는지 확인
    -> for문으로 ```[Int]```를 순회하며 filter로 자신과 같은 값이 있는지 확인
    - 입력된 값이 3개인지 확인
    -> ```[Int].count == 3```인지 확인
    - 입력된 값이 1 ~ 9사이인지 확인
    -> ```1 <= $0 && $0 <= 9``` 조건을 통해 확인
    
    첫 구현에는 위 조건을 사용하여 값을 걸러내주었으나 더 간단하게 확인하고, guard문을 최대한 줄일 수 있도록 변경해주었다.
    - 입력된 값이 숫자인지 확인
    -> for문으로 구현했던 부분을 compactmap 메서드를 사용하도록 변경
    - 입력된 값이 중복되는지, 3개인지 확인
    -> ```Set<Int>([Int]).count == 3``` ```[Int]```를 Set으로 변경하여 중복되지 않음을 보장함과 동시에 count를 확인하도록 변경
    - 입력한 값이 1 ~ 9사이인지 확인
    -> 변경사항 없음  
    <br>

    ```swift
    let userNumbers = userInput.split(separator: " ").compactMap { Int($0) }
    
    guard Set(userNumbers).count == 3 && userNumbers.filter({ 1...9 ~= $0 }).count == 3 else {
        return nil
    }
    ```
    
  20줄 가량의 코드가 3줄로 요약되었다.
     
- **스트라이크, 볼 계산 함수 구현**
  - enumerated()함수를 사용하여 usernumber의 인덱스와 computerrandomnumber의 인덱스 비교
  - 스트라이크가 아닐때만 볼을 검사

    ```swift
    for (index, number) in userNumbers.enumerated() {
        if number == randomNumbers[index] {
            strike += 1
        } else if randomNumbers.contains(number) {
            ball += 1
        }
    }
    ```

- **에러 핸들링**
  - 잘못된 입력을 사용자에게 알리는 문구가 같았기 때문에 처음엔 Error enum을 만들지 않으려고 했음.
  - 사용자 입력을 검증하는 코드 디버깅 과정에서 정확히 어떤 에러를 발생하는지 알면 유용할 것이라 판단하여 enum을 선언.
  - 구현을 마치고 과제의 제약사항을 살펴보는데 '사용자 정의 타입을 구현하지 않고, 함수와 변수, 상수만으로 구현합니다.' 라는 문구를 발견하고 enum을 삭제.
  
  -> 코드 디버깅 과정에서도 분기별로 print를 다르게 출력하여 구분해주면 enum을 추가하지 않더라도 처리가 가능했을거란 생각이 듬
  
  - 메인 함수에서 이 사용자 입력이 유효하다는것을 판단하는 방법을 고민
    -> 가공한 숫자도 주고받아야 했으므로 ```(Bool, [Int])``` 형태의 튜플을 리턴받는것으로 구현
    -> ```[Int]?``` 옵셔널을 사용하여 유효하지 않다면 nil을 리턴하는 방식으로 변경
  
- **들여쓰기 제한**
  - 이중 for문이 사용되는 부분을 함수로 기능분리하여 호출

## 8. 참고 링크

[Apple Developer Documentation - split](https://developer.apple.com/documentation/swift/string/split(separator:maxsplits:omittingemptysubsequences:))

[Apple Developer Documentation - compactmap](https://developer.apple.com/documentation/swift/sequence/compactmap(_:))

[Apple Developer Documentation - optional](https://developer.apple.com/documentation/swift/optional)

[Apple Developer Documentation - control flow](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/controlflow/)


---
### 팀 회고

- **우리팀이 잘한 점**  
    성실하게 시간을 많이 투자했다.  
    개인이 부족한 부분을 놓치지 않고 설명해주며 채워주웠다.  
    
- **서로에게 좋았던 점 피드백**  
    Hisop - 제 흐름에만 맞춰 달린 것 같은데 제 속도에 맞춰주신거 같아 좋았습니다.  
    Kiseok - 실력차이가 나서 답답한 부분이 있었을텐데 모르는 부분을 설명까지 해주며 이끌어주셨다.  
    
- **서로에게 아쉬웠던 점 피드백**  
    Hisop - 제 코드에 태클을 걸어주십쇼...
    
