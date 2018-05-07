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
* 솔리디티는 다른 언어와 마찬가지로, 다양한 자료형을 지원한다. (배열,구조체 지원)
* 단, 부동소수점 자료형(float)는 지원하지 않는다.

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
