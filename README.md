# iOS Calculator Application Project
### 10진 & 2진 계산기 앱 프로젝트
[Ground Rule](https://github.com/GREENOVER/ios-calculator-app/blob/main/GroundRule.md)
***
#### What I learned✍️
- Stack
- mutating
- Generic
- CaseIterable
- Protocol
- associatedtype
- typealias

#### What have I done🧑🏻‍💻
- Stack 자료구조를 학습하고 스택을 활용하여 입력과 계산값을 저장하였다.
  - pop / push / peek
- 구조체에서 내부 메서드가 구조체 내부에서 데이터를 수정할때 필요한 mutating 키워드를 선언하여 구현하였다.
- 제네릭을 통해 스택에 2진과 10진 어느 값이든 받을 수 있도록 구현하였다.
- Enum 타입에 CaseIterable 프로토콜을 채택하여 해당 값들을 컬렉션하게 사용하였다.
- 10진과 2진 계산기의 기본이 되는 기본 계산기를 프로토콜로 정의하였다.
- associatedtype을 정의하여 기본 계산기가 타입이 되도록 구현하였다.
- typealias를 각 10/2진 계산기에서 다르게 선언하여 해당하는 자료형 타입을 주도록 구현하였다.
- 10진과 2진 계산을 하도록 구현하였다.

#### Trouble Shooting👨‍🔧
- 문제점 (1)
  - 입력된 값이 순차적으로 계산이 되는 문제
- 원인
  - 연산자 우선순위가 고려되지 않아 순서대로 값이 계산되어 발생하는 문제점
- 해결방안
  - verifyPriorityOperator() 메서드를 구현하여 연산자 우선순위에 따라 연산 혹은 스택에 연산자가 저장되도록 구현
  ```swift
  func verifyPriorityOperator() {
        if operatorSet.peek() == .multiplication || operatorSet.peek() == .division || operatorSet.peek() == .sum {
            while operatorSet.count() != 0 {
                presentOperator = operatorSet.pop()
                operate(presentOperator!)
            }
        }
        else {
            operatorSet.push(element: presentOperator!)
        }
    }
  ```
  10진수 사칙연산에 대해 후위표기법으로 변환하지 않아도 중위표현이 된 수식에 대해 연산자 우선순위에 따라 구현될 수 있도록 적용


#### Thinking Point🤔
- 고민점 (1)
  - "10진과 2진 계산기 모두 공통으로 쓰일 수 있는 스택 자료구조를 정의할 수 있을까?"
- 원인 및 대책
  - 10진은 Int 타입으로 2진은 Double 타입으로 계산하고 반환한다. 각 계산기마다 스택을 별도로 구현한다면 낭비이다. 그래서 아래와 같이 제네릭 타입으로 스택을 구현하였다.
  ```swift
  struct Stack<T> {
    var index = [T]()
    
    mutating func pop() -> T? {
        return self.index.popLast()
    }
    
    mutating func push(element: T) {
        self.index.append(element)
    }
    
    func peek() -> T? {
        return self.index.last
    }
    
    func isEmpty() -> Bool {
        return self.index.isEmpty
    }
    
    func count() -> Int {
        return self.index.count
    }
  }
  ```
- 고민점 (2)
  - "10진과 2진 계산기의 중복되는 기능들을 하나로 통합할 수 있을까?"
- 원인 및 대책
  - 10진과 2진의 공통된 기능을 위해 Calculator라는 프로토콜을 정의하고 각 계산기에서 해당 프로토콜 메서드를 각각 정의하여 사용하도록 구현하고 타 계산기에 없는 특별한 기능들은 해당 계산기에서 별도 구현하도록 하였다.
  ```swift
  protocol Calculator {
    associatedtype CalculatorType
    
    func getOperatorButton() -> CalculatorType
    func result() -> CalculatorType
    func clear()
    func stackPush()
    func verifyPriorityOperator()
    func operate(_ present: Operator)
  }
  ```
- 고민점 (3)
  - "프로퍼티들이 외부에 노출되어야 하나요?"
  ```swift
      var decimalNumber = Stack<Double>()
    var operatorSet = Stack<Operator>()
    var inputNumber: Double?
    var resultNumber: Double?
    var inputOperator: Operator?
    var presentOperator: Operator?
  ```
- 원인 및 대책
  - 각 계산기에 정의된 프로퍼티들은 외부에 노출될 필요가 없음으로 private 접근제어자를 두어 리팩토링
  ```swift
  private var decimalNumber = Stack<Double>()
  private var operatorSet = Stack<Operator>()
  private var inputNumber: Double?
  private var resultNumber: Double?
  private var inputOperator: Operator?
  private var presentOperator: Operator?
  ```
- 고민점 (4)
  - 옵"ㅇ
    private var presentOperator: Operat
