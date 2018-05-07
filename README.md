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

**6. 메세지 프로퍼티(Message Properties)**
* 계약은 msg 프로퍼티를 사용해 계약을 호출한 사람이 보낸 메세지를 확인한다.
* gas limit : 그 계약에서 한번 호출로 소비할 수 있는 최대의 가스 양

|  정보  |   타입  |                   설명                  |
|:------:|:-------:|:---------------------------------------:|
|  data  |   byte  |               호출 데이터               |
| sender | address |       계약을 호출한 이더리움 주소       |
|  value |   uint  |         계약 주소로 보낸 Ether량        |
|   gas  |   uint  | gas limit에서 함수를 호출하고 남은 가스 |

**7. Transfer 함수**
* transfer 함수를 사용하면 계약이 다른 이에게 이더를 전송한다.

```
<받는 사람의 주소>.transfer(<송금할 금액>);

pragma solidity ^0.4.18;

contract Solution {
    address friend;

    function transfer() payable public {
        friend.transfer(msg.value);
    }

    function set(address _friend) public {
        friend = _friend;
    }
}
```
**8. 계약 수수료(Gas)**
* 작성된 스마트 계약은 EVM(Ethereum Virtual Machine) 타킷으로 컴파일된 후, 이더리움 네트워크에 배포된다.
* 이렇게 배포된 스마트 계약 코드는 Gas라는 수수료를 내야 사용할 수 있다.
* 수수료는 노드들이 데이터를 검증하고, 기록하는 등 다양한 일에 대한 댓가 (스마트 계약에 명령어가 많을수록 Gas를 더 많이 냄)

**9. 계약 기록, 트랜잭션(Transaction)**
* Smart Contract로 생성되는 장부 및 기록
* 트랜잭션들이 모여 하나의 블록을 이룬다. (이더리움 네트워크 상에는 이러한 블록들이 체인처럼 엮은 모습을 하고 있기 때문에 "블록체인" 이라고 부름)

* 트랜잭션이 담은 정보

|                   |                                    |
|-------------------|------------------------------------|
|  transactionHash  |          트랜잭션의 해시값         |
|  transactionIndex |         트랜잭션의 인덱스값        |
|     blockHash     | 이 트랜잭션이 추가된 블록의 해시값 |
|    blockNumber    |  이 트랜잭션이 추가된 블록의 번호  |
|      gasUsed      |  이 트랜잭션 호출에 소비한 가스양  |
| cumulativeGasUsed |       누적으로 사용된 가스량       |
|  contractAddress  |             계약의 주소            |
|        logs       |         event로 로깅된 정보        |

**10. 접근 제어자(Visibility Modifiers)**
* 접근 제어자를 통해 상태변수와 함수에 접근 범위를 제한 할 수 있다.

| 접근 제어자 |              설명             |                                                특징                                               |
|:-----------:|:-----------------------------:|:-------------------------------------------------------------------------------------------------:|
|    public   |       외부에서 호출 가능      | 접근 제어자를 명시하지 않은 함수는 public으로 선언, public으로 선언하면 자동으로 Getter 함수 생성 |
|   internal  | 상속받은 계약에서만 호출 가능 |                      접근 제어자를 명시하지 않은 상태 변수는 internal로 선언                      |
|   private   |   해당 계약에서만 호출 가능   |                   상속받은 계약도 private으로 선언된 상태 변수/함수 호출 불가능                   |
|   external  |       인터페이스의 함수       |              이 상태 변수/함수를 선언한 계약 내부에서는 이 상태 변수/함수 호출 불가능             |

**11. View 함수**
* 수수료(gas)가 들지 않는다.
* 상태를 변경하지 않는다. (ReadOnly)

```
contract Variables {
    uint year = 2018;

    function get() public view returns (uint) {
        return year;
    }
}
```

**12. Pure 함수**
* 수수료(gas)가 들지 않는다.
* 블록체인 네트워크에 기록된 데이터에 아예 접근하지 않는다.
* 파라미터로 주어지지 않은 상태 변수는 읽거나 쓸 수 없다.

|             구분            | 일반 함수 | View 함수 | Pure 함수 |
|:---------------------------:|:---------:|:---------:|:---------:|
| 네트워크에 기록된 상태 읽기 |     O     |     O     |     X     |
| 네트워크에 기록된 상태 쓰기 |     O     |     X     |     X     |
|      호출 시 가스 소모      |     O     |     X     |     X     |

**13. 구조체(Structs)**
* 다양한 자료형의 데이터를 묶어서 관리할 수 있다.

```
struct Person {
    string name;
    uint age;
    address wallet;
}

function birthday() {
    Person p = Person("James", 26, 0x7Df34...);
    p.age += 1;
}
```

**14. 맵핑(Mappings)**
* <Key-Value> 형태의 자료 구조
* 선언 시, Key 타입과 Value 타입을 지정해주어야 하며 Key로 Value를 불러올 수 있다.

예제 (key로 address, value로 uint를 사용하는 예제)
```
mapping (address => uint) public balances;

function open(uint newBalance) public {
    balances[msg.sender] = newBalance;
}
```

**15. Event로 로그 남기기**
* 계약이 수행된 정보는 트랜잭션으로 남는다. (트랜잭션에 기록하고 싶은 사항이 있다면 이벤트를 사용)

예제 (물건이 구입될때마다 구매자의 이더리움 주소와 결제 금액을 기록하는 예제)
```
event BuySomethings(
    address indexed _buyer, // indexed 키워드가 붙은 데이터는 log 검색 시 사용할 수 있음
    uint256 _value
);

function buy() payable public {
    seller.transfer(msg.value);    
    // 이벤트 호출
    BuySomethings(msg.sender, msg.value);
}
```

**16. 오류 처리(Error Handling)**
* require(bool 조건) : 조건을 만족하지 않으면 오류를 발생시킨다.
* 솔리디티에는 catch 매커니즘이 없다. 오류가 발생하면 진행 중인 스마트 계약은 즉시 종료되며, 모든 상태는 호줄되기 전으로 돌아간다. (이때 계약을 호출할 때 소비한 가스는 반환되지 않는다)

```
function half(uint x) {
    require(x % 2 == 0); // 짝수일 때만 허용
}
```

**17. 함수 제어자(Function Modifiers)**
* 함수 제어자는 커스텀 할 수 있는 제어자이다.
* modifier 키워드로 선언한다. 함수를 호출할 때 함수 제어자를 명시해주면 실행 시 오류를 처리 할 수 있다.
* 모든 실행 조건을 만족한 경우를 언더바(_)로 표시한다.

예제 (owner만 가격을 수정하도록 허용하는 함수 제어자 예제)
```
modifier onlyOwner {
    require(msg.sender == owner);
    _;
}

function changePrice(uint _price) public onlyOwner {
    price = _price;
}
```

**크라우드 펀딩 DApp 예제**

```
pragma solidity ^0.4.18;

contract Crowdfunding {
    struct Campaign {
        uint id;
        address creator;
        mapping (address => uint) balanceOf;
        uint fundingGoal;
        uint pledgedFund;
        uint deadline;
        bool closed;
    }

    mapping (uint => Campaign) campaigns;
    uint campaignsId = 0;
    
    modifier campaignOwner(uint _campaignId) { 
        require(msg.sender == campaigns[_campaignId].creator);
        _;
    }

    event GeneratedCampaign(
        uint indexed _id,
        address indexed _creator,
        uint _fundingGoal,
        uint _deadline
    );

    event FundCampaign(
        uint indexed _id,
        address indexed _funder,
        uint _amount,
        uint _pledgedFund
    );

    event FundTransfer(
        uint indexed _id,
        address indexed _creator,
        uint _pledgedFund,
        bool _closed
    );

    function createCampaign(uint _fundingGoal) public {
        campaigns[campaignsId] = Campaign(campaignsId, msg.sender, 
                                          _fundingGoal, 0, 
                                          getDeadline(now), false);

        Campaign memory campaign = campaigns[campaignsId];
        GeneratedCampaign(campaignsId, msg.sender, campaign.fundingGoal, campaign.deadline);
        campaignsId++;
    }

    function fundCampaign(uint _campaignId) payable public {
        require(msg.sender != campaigns[_campaignId].creator);

        campaigns[_campaignId].pledgedFund += msg.value;
        campaigns[_campaignId].balanceOf[msg.sender] += msg.value;

        FundCampaign(_campaignId, msg.sender, msg.value, 
                     campaigns[_campaignId].pledgedFund);
    }

    function checkFundingGoal(uint _campaignId)
    public campaignOwner(_campaignId) {
        Campaign memory c = campaigns[_campaignId];

        if (c.fundingGoal <= c.pledgedFund) {
             campaigns[_campaignId].closed = true;

            msg.sender.transfer(c.pledgedFund);
            FundTransfer(_campaignId, msg.sender, 
                         c.pledgedFund, 
                         campaigns[_campaignId].closed);
        } else {
            revert();
        }
    }

    function getDeadline(uint _now) private pure returns (uint) {
        return _now + (3600 * 24 * 7);
    }
}
```
