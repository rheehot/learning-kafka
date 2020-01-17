
# 카프카 설치

<hr>


### 주키퍼 설치 및 실행
* 카프카와 주키퍼는 `java` 기반으로 동작하기 때문에 `java` 설치가 선행되어야합니다.
* 주키퍼 설치
  * 설치 경로 접근
  ```bash
  $ cd /usr/local
  ```

  * 주키퍼 다운로드(3.4.14 버전으로 설치함)
  ```bash
  $ wget http://apache.mirror.cdnetworks.com/zookeeper/zookeeper-3.4.14/zookeeper-3.4.14.tar.gz
  ```

  * 압축 해제
  ```bash
  $ tar zxf zookeeper-3.4.14.tar.gz
  ```

  * 심볼릭 링크를 생성하여 버전에 의존성없는 디렉토리 생성
  ```bash
  $ ln -s zookeeper-3.4.14 zookeeper
  ```

  * 스냅샷과 로그를 저장할 디렉토리 생성
  ```bash
  $ mkdir -p /data{ID}
  ```

  * 주키퍼 노드 구분을 위한 ID 생성
  ```bash
  $ echo {ID} > /data/myid
  ```

* 주키퍼 설정
  * zoo.cfg 파일 접근하기
  ```bash
  $ vi /usr/local/zookeeper/conf/zoo.cfg
  ```

  * 주키퍼 환경 설정 - 기본 설정만 간략하게 변경
  ```yml
  # 측정 단위 시간
  tickTime=2000
  # 팔로워가 리더와 초기에 연결하는 시간에 대한 타임 아웃 tick의 횟수
  initLimit=10
  # 팔로워가 리더와 동기화 하는 시간에 대한 타임 아웃 tick의 횟수
  syncLimit=5
  # 로그와 스냅샷이 저장되는 저장 경로
  dataDir=/data
  # 주키퍼 사용 TCP 포트
  clientPort=2181
  # 앙상블을 구성하기 위한 주키퍼 node ID(FQDN or 공인IP)
  server.1=alan-zk-test-1.ay1.krane.9rum.cc:2888:3888
  server.2=alan-zk-test-2.ay1.krane.9rum.cc:2888:3888
  server.3=alan-zk-test-3.ay1.krane.9rum.cc:2888:3888
  ```

* 주키퍼 실행 및 중지  
  * 주키퍼 실행
  ```bash
  /usr/local/zookeeper/conf/zkServer.sh start
  ```

  * 주키퍼 실행 중지 
  ```bash
  /usr/local/zookeeper/conf/zkServer.sh stop
  ```  
  cf) 주키퍼용 서비스 파일을 만들어서 `systemd`에 등록하고 `systemctl` 명령으로 제어할 수 있음


### 카프카 설치 및 실행
* 카프카 설치
  * 설치 경로 접근
  ```bash
  $ cd /usr/local
  ```

  * 카프카 다운로드(2.3.1 버전으로 설치함)
  ```bash
  $ wget http://apache.mirror.cdnetworks.com/kafka/2.3.1/kafka_2.12-2.3.1.tgz
  ```

  * 압축 해제
  ```bash
  $ tar zxf kafka_2.12-2.3.1.tgz
  ```

  * 심볼릭 링크를 생성하여 버전에 의존성없는 디렉토리 생성
  ```bash
  $ ln -s kafka_2.12-2.3.1 kafka 
  ```

  * 로그 데이터를 저장할 디렉토리 생성
  ```bash
  $ mkdir -p /data{ID}
  ```

* 카프카 설정
  * 카프카 환경 설정 파일 접근하기
  ```bash
  $ vi /usr/local/kafka/config/server.properties
  ```
  
  * 카프카 환경 설정 - 기본 설정만 간략하게 변경
  ```yml
  # 호스트ID와 매핑되는 브로커ID
  broker.id=broker.id=3
  # 로그 데이터를 저장할 디렉토리
  log.dirs=/data1
  # 주키퍼 접속 정보
  zookeeper.connect=alan-zk-test-1.ay1.krane.9rum.cc:2181, alan-zk-test-2.ay1.krane.9rum.cc:2181,alan-zk-test-3.ay1.krane.9rum.cc:2181
  ```

* 카프카 실행 및 중지
  * 카프카 실행
  ```bash
  $ /usr/local/kafka/bin/kafka-server-start.sh /usr/local/kafka/config/server.properties
  ```

  * 카프카 실행 중지
  ```bash
  $ /usr/local/kafka/bin/kafka-server-stop.sh 
  ```
  cf) 카프카용 서비스 파일을 만들어서 `systemd`에 등록하고 `systemctl` 명령으로 제어할 수 있음

### 카프카 상태 확인
* TCP 포트 확인
  * 주키퍼의 기본 TCP 포트는 2181이므로 2181 포트가 LISTEN 중인지 체크 
  ```bash
  netstat -ntlp | grep 2181
  ```
  * 카프카의 기본 TCP 포트는 9092이므로 9092 포트가 LISTEN 중인지 체크
  ```bash
  netstat -ntlp | grep 9092
  ```
