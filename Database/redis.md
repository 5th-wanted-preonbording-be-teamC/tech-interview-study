# Redis란?
Redis는 Remote Dictionary Server의 약자이다. key-value 방식으로 데이터를 저장하고 관리하기 위한 비 관계형 데이터베이스로 cache 방식으로 빠르게 원하는 데이터를 가져올 수 있는 메모리 기반의 DB이다. 데이터 타입은 문자열, 리스트, 해시, 셋, 정렬된 셋 등 다양한 방식을 지원한다. 

> Memcached와의 차이
Redis와 비슷하지만 대부분의 기능은 Redis로도 커버가 가능하다.<br>
Redis와 Memcached의 차이점은 아래와 같다. 
> * 다양한 데이터 구조 지원
> * 데이터 저장, 복제
> * 트랜잭션 지원
> * Replica를 통해 master, slave 구조를 구현 가능

# Redis 백업 방식
대표적인 두 가지 백업 방식이 있다.
* RDB(Snapshotting) 방식
  * 순간적으로 메모리에 있는 내용을 DISK에 전체를 옮겨 담는 방식이다.
  * AOF 파일보다 사이즈가 작다. 따라서 로딩 속도가 AOF 보다 빠르다.
* AOF(Append On File) 방식
  * Redis의 모든 write/update 연산 자체를 모두 log 파일에 기록하는 형태이다.
  * 계속 추가되면서 기록된다. 하지만 특정 시점에 데이터 전체를 다시 쓰는 기능이 있다.
  * 기록이 누적되면서 사이즈 제한이 걸려 기록이 중단될 수 있고, 레디스 서버 시작 시 로드가 지연될 수 있다.
  * rewrite를 통해 파일 데이터를 다시 써서 사이즈를 줄일 수 있다.
  



### 참고(세미나 강력 추천)
* [우아한테크 세미나 Redis](https://youtu.be/mPB2CZiAkKM)
* https://luran.me/category/Development/Hadoop%2C%20NoSQL%2C%20BigData