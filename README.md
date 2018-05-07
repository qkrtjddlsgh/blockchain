**1. 솔리디티(Solidity) 언어란?**
* 솔리디티는 이더리움 블록체인 플랫폼에서 스마트 계약(Smart Contract)를 정의하는 언어이다.
* 솔리디티 문법은 자바스크립트 문법과 유사하지만, 정적 타입 언어라 자료형을 명시해주어야 한다.

**2. DApp이란?**
* 스마트 계약을 구현한 블록체인 기반의 탈중앙화 어플리케이션

예제 (Greeting.sol)
```
pragma solidity ^0.4.18; // 솔리디티 버전을 나타내는 코드

contract Greeting {
    string message = "Hello Solidity";
    string name = "name";

    function getMessage() public view returns(string) {
        return message;
    }

    function getName() public view returns(string) {
        return name;
    }
}
```

**3. 솔리디티 자료형**
* 솔리디티는 다른 언어와 마찬가지로, 다양한 자료형을 지원한다. (배열, 구조체 지원)
* 단, 부동소수점 자료형(float)는 지원하지 않는다. (부동소수점 타입으로는 수를 정확하게 표현하지 못하므로 Ether를 다룰 수 없다)
* 솔리디티의 정수 자료형은 그 크기를 명시할 수 있다. (uint8, uint256, int8, int256)

|  자료형 |         설명        |
|:-------:|:-------------------:|
|   uint  |  부호가 없는 정수형 |
|   int   |        정수형       |
|   bool  |     논리 자료형     |
|  string | UTF-8 인코딩 문자열 |
|  bytes  |        바이트       |
| address |   이더리움 주소 값  |


예제 (Variables.sol)
```
pragma solidity ^0.4.18;

contract Variables {
    string public name = "James";
    uint128 public birthday = 20180328;
    address public addr = 0x72ba7d8e73fe8eb666ea66babc8116a41bfb10e2;
    uint[] setOfYear = [2018, 2019, 2020];

    uint year = setOfYear[0];
    bool isHappy = true;

    function setYear() public {
        year = 2018;
    }
    
    function getYear() public view returns (uint) {
        return year;
    }
    
    function getHappy() public view returns (bool){
        return isHappy;
    }
}
```

**4. 연산자와 제어문**
* C언어나 Java에서 사용하는 연산자와 제어문과 대부분 일치한다.
* switch/case나 goto문은 지원하지 않는다.
* 솔리디티는 자동 형 변환을 지원하지 않는다. 예를 들어 if(true)는 허용하지만 if(1)은 허용하지 않는다.

**5. Payable 키워드**
* 스마트 계약도 내부적으로 이더리움 계정을 가진다. (A->B로 송금시, 계약 계정(Contarct Accounts)을 거쳐서 송금)
* payable 키워드는 계약 계정이 외부에서 이더를 송금 받을 수 있도록 한다. (즉, 계약이 A에게 송금을 받으려면 A가 호출하는 함수에 payable 키워드가 필요)

예제 (Payable.sol)
```
function 함수이름() payable public {
    //함수 내용
}
```

```
pragma solidity ^0.4.18;

contract Solution {
    uint256 balance = 0;

    function sending() payable public {
        balance = msg.value;
        Sended(msg.value, balance);
    }

    event Sended(
        uint256 _value,
        uint256 _balance
    );
}
```

**6. 메세지 프로퍼티**
* 계약은 msg 프로퍼티를 사용해 계약을 호출한 사람이 보낸 메세지를 확인한다.
* gas limit : 그 계약에서 한번 호출로 소비할 수 있는 최대의 가스 양

|:------:|:-------:|:---------------------------------------:|
|  data  |   byte  |               호출 데이터               |
| sender | address |       계약을 호출한 이더리움 주소       |
|  value |   uint  |        계약 주소로 보낸 Ether 량        |
|   gas  |   uint  | gas limit에서 함수를 호출하고 남은 가스 |
