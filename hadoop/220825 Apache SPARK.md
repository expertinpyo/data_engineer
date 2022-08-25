# Apache Spark



Spark란?

대규모 데이터 처리에 사용되는 신속하고 보편적인 엔진

Java, Scala, Python 등을 사용해 스크립트를 작성할 수 있는 유연성을 제공함

복잡한 데이터를 조작, 변형, 분석할 수 있음



Pig같은 기술과 달리, Spark 위에 또 다른 풍부한 생태계가 존재함

=> 이 것을 활용해 머신러닝, 데이터 마이닝, 그래프 분석, 데이터 스트리밍 같은 복잡한 일을 할 수 있음



Spark 구성도

Driver Program(Spark context) => Cluster Manager(Spark, YARN) => 여러 개의 Executor(Cache, Tasks)



- Driver Program
  - 작업을 어떻게 진행할지 통제하는 스크립트
- Cluster Manager
  - YARN, Sparkf를 통해 활용
  - Hadoop에서 꼭 사용할 필요는 없음
  - 내장된 클러스터를 활용하거나 Mesos라는 클러스터 관리자를 사용할 수도 있음

- Executor
  - 데이터를 병렬로 처리하는 프로세스
  - Cache
    - Spark 성능의 키
  - Tasks

디스크 기반 솔루션 : HDFS와 항상 접촉함

Spark는 이와 달리, 메모리 기반 솔루션(정보를 최대한 RAM에 유지하며 처리함) => 속도가 빠름

방향성 비사이클 그래프(DAG, Directed Acyclic Graph / 개별 요소들이 특정한 방향을 향하고 있으며, 서로 순환하지 않는 구조로 짜여진 그래프)

MapReduce 대비 최대 100배까지 빠름

최종 결과부터 시작해 거꾸로 돌아오며 작업 흐름을 최적화하고 최종 결과에 다다르는 가장 빠른 방법을 계산함



RDD(Resilient Distributed Dataset) : 회복성 분산 데이터. Spark의 기본 데이터 구조

분산 변경 불가능한 객체의 모음. Spark는 새로는 RDD를 반들거나, 기존 RDD를 변형함.



### Components of Spark

Spark Streaming / Spark SQL / MLLib / GraphX

SPARK CORE



- Spark core내의 RDd에 프로그래밍 시 Spark 위에 구축된 라이브러리 패키지가 있어 함께 사용 가능

- Spark Streaming => 데이터를 일괄 처리하는 대신 실시간으로 데이터 입력 가능

- Spark SQL => Spark의 SQL 인터페이스. SQL 작성 or SQL 유사 함수를 통해 Spark의 데이터 세트(RDD)를 변환함

- MLLib => Spark의 데이터 세트에 실행하는 머신 러닝이나 데이터 마이닝 같은 도구의 라이브러리

-  GraphX => 그래프 이론에서 사용되는 그래프

  - ex) 인스타 팔로워, 팔로우 관계를 어떻게 가장 짧게 연결할 수 있는 지등을 볼 수 있음

    

### RDD, Resilient Distributed Dataset

회복성 분산 데이터 세트

Spark 내부에서 일어나는 일들을 추상적으로 표현한 것

작업을 클러스터에 고르게 분산하고 실패가 생겨도 회복할 수 있음

RDD 객체 => key - value or 일반적인 정보를 저장하는 수단이고 클러스터에 알아서 작업함

내부적으로는 회복성이 있고 분산되어 있지만, 사용자가 고려할 것은 크게 없음. 그냥 데이터 세트라고 생각하기



#### SparkContext

드라이버 프로그램이 SparkContext를 만듦

- RDD를 운영할 환경
- RDD를 만들어줌
- 객체를 대화형으로 만드는 Spark shell인 "sc" 존재
- 독립적인 스크립트를 사용해 SparkContext를 스케쥴에 따라 만들 수 있음



=> SparkContext란, Spark 내에서 드라이버 프로그램이 작동하는 환경이며, RDD를 만드는 주체임



#### RDD 만들기

다양한 방법으로 만들 수 있음

- paralleize(list) => list를 가진 RDD 생성 가능
  - 하지만, list안의 담긴 데이터는 너무 작으므로 잘 안쓰는 방법
- RDD를 text 파일에서 불러오기(sc.textFile)=> hdfs:// 를 사용해 hdfs에 있는 대용량 파일도 불러올 수 있음, AWS 사용 시 s3n://
- HiveContext(sc) => Spark 내에서 SQL 쿼리 사용
- 그 외
  - JDBC
  - Cassandra
  - HBase
  - Elastisearch
  - JSON, CSV, sequence files, object files, various compressed formats

결론 : 대부분의 데이터 소스의 빅데이터를 RDD로 만들 수 있다! => 확장성이 크다



#### RDD 이용하기

- map
  - Mapping / 입력 행과 출력 행이 1:1 관계일 때 많이 사용
- flatmap
  - 입력 행과 출력 행 사이에 다양한 관계가 있을 때 사용
- filter
  - RDD에서 데이터를 거를 때 사용
  - 이 때, RDD의 행을 유지할지 말지 결정하는 함수를 사용함
- distinct
  - 고유한 값 찾아냄

- sample
  - 무작위 표본을 만듦
- union, intersection, subtract, cartesian
  - RDD를 결합함



#### RDD Actions

하나의 RDD를 가져와 변형하는 작업



- collect
  - RDD 결과물을 입력받아 드라이브 스크립트로 빨아들임
  - RDD 데이터를 가져와 Python 객체로 반환
- count
  - RDD에 얼마나 많은 행이 있는지 알려줌

- countByValue
  - 각각의 고유값이 얼마나 RDD에 있는지 총계를 냄
- take
- top
- reduce
  - Reducing 작업



#### Lazy evaluation

Spark에서는 코드를 실행하기 전까지 아무런 변화가 없음

=> 실행 시, 종속성을 통해 가장 빠른 경로를 찾아냄

=> 그 이후에 클러스터에서 작업을 시작함

=> Spark 속도의 핵심





### Spark 2.0 and Spark SQL



RDD의 형태는 정확히 표기되어있지 않다

DataFrame이 Spark에서 하는 일 => RDD를 RdataFrame 객체로 확장하는 것



DataFrame

- 행 객체 존재
- 그 행에는 구조화된 데이터 들어있음



RDD를 DataFrame으로 변환한다면

=> 데이터 유형과 이름을 가진 실제 열(column)이 있는 구조화된 정보를 갖게 됨

=> Spark의 DataFrame에 SQL 쿼리를 실행할 수 있게 됨



위 방법의 장점

- DataFrame과 연관된 실제 스키마 존재, 그 안에 실제 유형 정보 담겨 있음
  - 데이터를 더 효율적으로 저장 or 네트워크를 통해 전송 가능
- SQL쿼리 최적화
  - Spark를 더 효율적이고 빠르게 사용 가능
  - JSON / Hive / parquet 같은 구조회 데이터 양식 읽을 수 있다는 뜻
- 데이터를 고수준으로 볼 수 있는 Tableau or JDBC를 사용해 다른 DB와 소통 가능



#### python에서 Spark SQL 사용하기

- from pyspark.sql import SQLContext, Row
- HiveContext(sc) => Hive 데이터베이스와 연결되어 있을 시 그 곳에서 DataFrame 가져올 수 있음
- spark.read.json(dataFile) => JSON 파일과 내제된 구조에서 column 이름과 column 데이터 유형을 가져온 후 DataFrame 생성
- createOrReplaceTempView => 그 이후 DB의 Table처럼 생긴 것을 만들 수 있음
- hiveContext.sql("""쿼리문""") => 그 곳에 sql 쿼리문 실행 가능



#### DataFrame에 직접 접근하기

- df.show()
- df.select("field name") => 해당 열 선택
- df.filter(조건) => 조건을 적용
- df.groupBy => 원하는 field 별로 묶기
- df.rdd().map(함수) => 내제된 RDD를 추출해 RDD 수준의 작업 가능



#### Spark 2.0, DataFrame => DataSet



Spark 2.0부터 DataFrame 개념 대신 DataSet 개념 도입

DataFrame은 DataSet의 열 객체

Dataset이 좀 더 포괄적인 용어

=> DataFrame에서 사용하는 열 객체를 포함해 어떤 형태의 정보를 포함할 수 있음

정적으로 작동해서 컴파일 단계에서 오류를 발견할 수 있음



스크립트 내에 SQL 쿼리를 사용해 추가적인 최적화 도모



#### Shell access

Spark SQL을 사용해 DB 서버를 시작해 연결할 수 있고, 다른 DB처럼 쿼리할 수 있음

Spark에 내제된 thriftserver 활용

입력된 테이블에 SQL 명령어 실행 가능



#### UDF's(User-Defined Functions)

Spark SQL의 확장성이 큼을 표현

SQL에 플러그인하는 사용자 정의 함수(UDF)를 만들어 SQL 쿼리에 사용할 수 있음

