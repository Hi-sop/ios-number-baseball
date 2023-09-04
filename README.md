

## 숫자 야구
- 컴퓨터는 1~9 범위내의 랜덤한 숫자를 3개 생성한다. 
- 사용자가 수를 입력하면 그 결과를 판별해 strike혹은 ball의 개수를 알려준다.
- 사용자는 위 결과를 받아 숫자를 맞추어 게임을 이기는 것이 목표.

## 팀원
|Hisop| Kiseok|
|---|---|
|[Github - Hisop](https://github.com/Hi-sop)| [Github - Kiseok](https://github.com/carti1108)

## 목차
[타임라인](#타임라인)  
[플로우 차트](#플로우-차트)  
[실행 화면](#실행-화면)  
[트러블 슈팅](#트러블-슈팅)  
[참고 자료](#참고-자료)

## 타임라인
- 08.28 (월)
  - 숫자야구 룰 이해
  - 플로우 차트 작성
  - 랜덤 숫자 생성, 볼 / 스트라이크 판별 함수 설계
- 08.29 (화)
  - 출력 결과 자연스럽게 조정
- 08.30 (수)
  - PR 작성 / 네이밍 및 고차함수 사용부분 변경
- 08.31 (목)
  - 플로우차트 전체 기능 넣도록 수정
  - 사용자 입력 검증함수 설계
  - PR 작성 / 네이밍 수정, 함수 기능 분리 등 리팩토링
- 09.01 (금)
  - READ ME 작성

## 플로우 차트
![number_baseball](https://github.com/Hi-sop/ios-number-baseball/assets/69287436/093df975-45fc-464c-84a3-dab6322447ca)


## 실행 화면
|잘못된 메뉴 입력|
|---|
|![잘못된 메뉴입력](https://github.com/Hi-sop/ios-number-baseball/assets/69287436/c7651622-9ec7-4f2b-9c3e-cd0b676b5e73)|

|잘못된 숫자가 입력된 경우|
|---|
|![잘못된 숫자입력](https://github.com/Hi-sop/ios-number-baseball/assets/69287436/1920d46c-69ea-4cdc-9b3b-8f459395d3f2)|

|정상적인 게임 결과|
|---|
|![정상적인 게임결과](https://github.com/Hi-sop/ios-number-baseball/assets/69287436/bc40f5bb-7a63-40d2-99cc-dd1bc70a964e)|

## 트러블 슈팅

1. **사용자 입력 검증부 예외처리**  
   사용자 입력에 대한 검증이 필요했다. 통과조건은 스페이스를 기준으로 구분된 1 ~ 9 사이의 숫자 3개. ```ex) 1 2 3```
    
    -> Readline으로 받아온 ```String```을 ```[Int]```로 변환하며 입력된 String이 숫자로 변환가능한지 확인.
     이 때 map으로 시도하다가 실패하여 for문을 사용했다. Int()를 통해 변환된 값은 옵셔널을 리턴했기 때문.  
    -> 옵셔널을 제외하고 반환해주는 ```compactmap```를 이용해 최종 변경했다.
    -> 추가 수정을 통해 guard문을 최대한 줄일 수 있도록 변경해주었다.
    <br>

    ```swift
    let userNumbers = userInput.split(separator: " ").compactMap { Int($0) }
    
    guard Set(userNumbers).count == 3 && userNumbers.filter({ 1...9 ~= $0 }).count == 3 else {
        return nil
    }
    ```
    
    20줄 가량의 코드가 3줄로 요약되었다.  <br>
     
2. **스트라이크, 볼 계산 함수 구현**  
  처음엔 스트라이크를 판별하는 방법으로 값이 중복되지 않는 타입인 Set을 생각했다.  
  겹치는 숫자에 대해서는 쉽게 판별할 수 있었으나 이 게임에서 스트라이크는 '자리수'가 맞아야해서 다른 방법으로 변경했다.  
    
    -> 현재 index의 유저넘버와 컴퓨터 넘버가 같다면 스트라이크로 판별해주었고 아니라면 contains를 통해 볼을 판별하도록 변경.  
  
    ```swift
    for (index, number) in userNumbers.enumerated() {
        if number == randomNumbers[index] {
            strike += 1
        } else if randomNumbers.contains(number) {
            ball += 1
        }
    }
    ```
<br>

3. **에러 핸들링**  
  사용자 입력을 검증하는 코드가 메인 반복문을 실행하는 함수에 상태를 전달하는 방법을 고민했음.  
   -> 가공한 배열도 같이 전달받아야 했으므로 ```(Bool, [Int])``` 형태의 튜플을 리턴받는것으로 구현  
   -> ```[Int]?``` 옵셔널을 사용하여 유효하지 않다면 nil을 리턴하는 방식으로 변경함.  


## 참고 자료

[Apple Developer Documentation - split](https://developer.apple.com/documentation/swift/string/split(separator:maxsplits:omittingemptysubsequences:))

[Apple Developer Documentation - compactmap](https://developer.apple.com/documentation/swift/sequence/compactmap(_:))

[Apple Developer Documentation - optional](https://developer.apple.com/documentation/swift/optional)

[Apple Developer Documentation - control flow](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/controlflow/)


---

<details>
<summary>팀 회고</summary>
<div markdown="1">
  
- **우리팀이 잘한 점**  
    성실하게 시간을 많이 투자했다.  
    개인이 부족한 부분을 놓치지 않고 설명해주며 채워주웠다.  
    
- **서로에게 좋았던 점 피드백**  
    Hisop - 제 흐름에만 맞춰 달린 것 같은데 제 속도에 맞춰주신거 같아 좋았습니다.  
    Kiseok - 실력차이가 나서 답답한 부분이 있었을텐데 모르는 부분을 설명까지 해주며 이끌어주셨다.  
    
- **서로에게 아쉬웠던 점 피드백**  
    Hisop - 제 코드에 태클을 걸어주십쇼...

</div>
</details>


    
