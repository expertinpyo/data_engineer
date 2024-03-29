# 220815 hadoop



컴퓨터 클러스터링

- 네트워크에 접속된 다수의 컴퓨터들(PC, 워크스테이션 등)을 통합하여 하나의 거대한 병렬 컴퓨팅 환경을 구축하는 기법





클러스터링을 이용해 구축할 수 있는 컴퓨팅 환경

1. 병렬 처리
   - 다수의 컴퓨터를 이용하여 MPP 혹은 DSM형 병렬처리 시스템 구성 가능
2. 네트워크 RAM
   - 각 노드(PC)의 기억 장치들을 토앟ㅂ하여 거대한 분산 공유-기억장치를 구성할 수 있으며, 이 것은 가상 기억장치와 파일 시스템 성능을 크게 높여줌





Hadoop의 Hive는 데이터를 관계형 데이터베이스인 것 마냥 다룰 수 있음

=> SQL 쿼리 사용 가능함



Hadoop => 다수의 PC를 활용해 빅데이터를 다룸

분산저장 => Hadoop의 기술 중 하나. 한 개의 하드 드라이브에 국한되지 않겠다는 뜻.



클러스터에 컴퓨터를 추가하면, 그 컴퓨터의 하드 드라이브가 데이터 저장소의 일부가 됨

=> Hadoop 의 역할 : 클러스터의 모든 하드 드라이브에 걸쳐 분산돼 있는 모든 데이터를 단일 파일 시스템으로 보여줌



데이터가 소실되도 자동으로 클러스터의 다른 컴퓨터에도 해당 데이터를 보관하고 있으므로 자동 복구 가능 => 데이터 신뢰성 좋음



Hadoop : 분산 처리 => 데이터 클러스터 전체에 저장 + 해당 데이터 처리 시에도 클러스터의 컴퓨터들과 함께 함



데이터를 다른 양식으로 전환 or 다른 시스템으로 전송 or 집계할 때

=> Hadoop은 이 모든 작업을 병렬로 처리하도록 함



구글의 GFS(Google File System)이 Hadoop 분산 저장 개념의 시초가 됨

MapReduce => 분산 처리를 개발하는 데 Hadoop에게 영감을 줌



Hadoop은 수평적으로 확산함 => 병렬 처리, 분산



MapReduce / YARN / HDFS 는 Hadoop 자체임



1. HDFS(Hadoop의 데이터 저장 부분을 담당)(1단계)

   - Hadoop 분산 파일 시스템(Hadoop Distributed File System)
   - HDFS는 Hadoop 버전의 GFS임
   - 빅데이터를 클러스터의 컴퓨터들에 분산 저장하는 시스템
   - 클러스터의 하드 드라이브들을 하나의 거대한 파일 시스템으로 사용함
   - 또한, 데이터들의 복사본을 만들어서 자동으로 손실을 회복함

   

2. YARN(Yet Another Resource Negotiator)(2단계)

   - 또 다른 리소스 교섭자라는 뜻
   - Hadoop의 데이터 처리 부분을 담당
   - 컴퓨터 클러스터의 리소스를 관리하는 시스템
   - 누가 작업을 언제 실행하고, 어떤 노드(컴퓨터)가 추가 작업을 할 수 있고, 없고의 여부 등을 결정함

3. MapReduce(3단계)

   - 데이터를 클러스터 전체에 걸쳐 처리하도록 하는 프로그래밍 메타포 or 모델

   - Mapper와 Reducer로 나뉨. 두 개의 구분된 함수같은 느낌

   - Mapper

     - 클러스터에 분산돼있는 데이터를 효율적으로 동시에 변형시킬 수 있음

   - Reducer

     - 그 데이터를 집계함

     

4. Pig(4단계)

   - SQL 스타일의 스크립팅 언어
   - 고수준의 API
   - SQL과 비슷한 간단한 스크립트를 작성함 => python이나 java 활용안함
   - 작성된 스크립트를 MapReduce가 읽을 수 있도록 번역함
     - => MapReduce는 YARN과 HDFS에게 데이터를 처리하고 원하는 답을 가져오게 함

   

5. Hive(4단계)

   - Pig처럼 MapReduce 위에서 작동함. 하지만 실제 DB처럼 생김
   - 실제 SQL 쿼리를 받고 파일 시스템에 분산된 데이터를 SQL 데이터베이스 처럼 취급함
   - Shell 클라이언트나 ODBC(Open DB Connectivity)를 활용해 DB에 접속 가능
   - Hadoop 클러스터에 저장돼있는 데이터는 RDB가 아님에도 불구하고 SQL로 쿼리함

6. Apache Ambari(5단계)

   - 클러스터 전체를 보여줌
   - 클러스터에서 어떤 시스템을 사용하고 얼마나 많은 리소스를 사용하는지 등 무슨 일이 일어나는 지를 시각화 해서 보여줌
   - Hive or Pig 쿼리 실행 or DB를 불러올 수 있는 화면 존재
   - 클러스터와 동작하는 어플리케이션의 상태를 볼 수 있게끔 해줌

   

7. MESOS(2단계)

   - Hadoop의 일부는 아님
   - YARN의 대안 정도로 볼 수 있음
   - YARN과 같이 "리소스 교섭자" 역할
   - YARN과 다른 방식으로 문제 해결함
   - YARN과 협업 가능

8. Spark(3단계)

   - MESOS, YARN 둘 다를 기반으로 할 수 있음
   - MapReduce와 동등한 선상에 존재
   - Python, Java or Scala의 프로그래밍 필요
   - 클러스터의 데이터를 신속, 효율, 안정적으로 처리하고 싶을 때 사용
   - 머신러닝 데이터 or 실시간 스트리밍 데이터 핸들링 가능

   

9. TEZ(3단계)

   - Spark와 비슷한 기술 많이 사용함
   - 방향성 비사이클 그래프 사용함
     - 이 것 때문에, MapReduce의 일을 할 때 좀 더 유리해짐
     - Why? 쿼리 실행에 좀 더 효율적인 계획을 세우게 되므로
   - 보통 Hive와 같이 사용되어 성능을 가속화함(Hive는 TEZ 위에서도 작동 가능)



10. HBASE

    - 클러스터의 데이터를 트랜잭션 플랫폼으로 노출하는 역할. No SQL DB(단위 시간당 실행되는 트랜잭션의 수가 큰, 아주 빠른 db)

    - 따라서, 데이터를 웹 app or site에 노출시켜 OLTP(Online Transaction Processing) 트랜잭션을 하는데 적합함
    - 클러스터에 저장된 데이터를 노출시킴
      - 이 데이터는 Spark or MapReduce 등에 의해 전환 or 그 결과를 다른 시스템에 노출 시킬 빠른 방법을 제공함

11. Apache STORM
    - 스트리밍 데이터를 처리하는 방식(실시간 처리)
    - 센서, 웹 로그로부터 데이터를 스트리밍 할 때 사용(STORM or Spark Streaming 사용하는 듯)
12. OOZIE
    - 클러스터의 작업을 스케줄링함

13. Zookeeper

    - 클러스터의 모든 것을 조직화하는 기술

    - 어떤 노드가 살아있는지 추적 가능
    - 여러 앱이 사용하는 클러스터의 공유 상태를 안정적으로 확인함
    - 어떤 노드가 다운되더라도 일관성 있고, 안정적인 성능을 온 클러스터에 걸쳐 유지 할 수 있음
    - 많은 앱들이 이 것에 의존함

    

데이터 수집에 특화된 툴

14. Sqoop
    - Hadoop의 DB를 관계형 데이터베이스로 엮어냄
    - ODBC(Open DB Connectivity), JDBC(Java Database Connectivity)로 소통가능한 데이터는 Sqoop을 통해 HDFS 파일로 변형 가능
    - 레거시 데이터베이스와 Hadoop을 잇는 연결장치 역할

15. FLUME
    - 대규모 웹로그를 안정적으로 클러스터에 불러올 수 있음
    - 실시간으로 웹 서버의 웹 로그를 감시 + 클러스터에 게시 => STORM이나 Spark steaming을 사용해 처리함
16. Kafka
    - 좀 더 포괄적인 데이터 수집에 사용
    - PC or 웹 서버 클러스터에서 모든 종류의 데이터를 수집해 Hadoop 클러스터로 내보냄

외부 DB

17. SQL

18. Cassandra, MONGO DB

    - HBase처럼 '기둥형 데이터 스토어'. 웹 app 등에 데이터를 실시간으로 노출하는데 활용할 수 있음

    - 실시간 app과 클러스터 사이에 해당 툴을 사용해 층을 만들어 두는 것을 추천함

쿼리 엔진

19. Apache Drill
    - 다양한 NoSQL Db에 SQL 쿼리를 작성해 사용할 수 있도록 함
    - Hbase, Cassandra or MongoDB DB와 소통 가능
    - 이 후, 소통한 내용을 엮어서 이질적인 데이터 스토어들에 걸쳐 쿼리 작성 + 그 결과를 한데 모아줄 수 있음
20. HUE
    - Hive, HBASE에 잘 작동하는 쿼리를 대화형으로 생성할 수 있음
21. Apache PHOENIX
    - Apache Drill과 비슷한 역할을 함
    - 전체 데이터 스토리지 기술에 걸쳐 SQL 스타일의 쿼리를 할 수 있게 함.
    - 또한, ACID를 보장함(데이터 트랜젝션의 성질)
      - NoSQL이지만 관계형 DB와 되게 비슷해짐
    - OLTP를 제공함(Online Transaction Processing)
22. Presto
    - 전체 클러스터에 쿼리를 실행할 수 있는 방법
23. Zeppelin
    - 클러스터와의 상호작용과 사용자 인터페이스를 노트북 유형으로 접근함



