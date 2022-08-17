# Apache PIG



Hadoop과 MapReduce 위에 구축된 언어

=> Mapper와 Reducer를 만들지 않고도 MapReduce작업 가능



MapReduce의 가장 큰 문제 => 개발 사이클 타임

MapReduce 프로그램을 개발해 실행하고, 원하는 작업을 수행하기 까지 오랜 시간이 걸림



=> Pig는 해당 문제를 해결할 수 있음



Pig Latin은 절차형 언어로서 SQL과 비슷한 역할을 수행함

=> MapReduce 작업을 좀 더 간소화 할 수 있음

커스텀 함수를 활용해 하고자하는 거의 모든 일을 할 수 있음



MapReduce에는 순차적인 순서가 존재함

=> Map으로 데이터를 변형하고, Reduce로 집계함 => 이 과정의 반복이라는 순서 존재



TEZ는 이런 것들의 상호 의존성을 파악하고, 의도한 바를 이루기 위한 가장 효율적인 경로를 계산함

TEZ가 MapReduce보다 약 10배정도 빠른 듯



Pig는 MapReduce와 TEZ 위에 있음

Pig를 독립적으로 사용할 수도 있음



Pig를 사용할 수 있는 방법

- Grunt : 명령줄 해석 프로그램

  - 마스터 노드에서 pig라고 입력하면 Grunt 프롬포트가 나옴 => Pig 스크립트를 한 줄씩 작성하고 실행할 수 있음
  - 작은 단위의 데이터를 만질 때 효율적인 듯

- Script : 스크립트를 파일에 저장하고 명령줄에서 Pig를 사용해 실행할 수 있음

  - Crontab 등에서 주기적으로 실행할 수도 있음

  



ex)

~~~
ratings = LOAD '/user/maria_dev/ml-100k/u.data' AS (userID:int, movieID:int, rating:int, ratingTime:int)

DUMP ratings;
=> DUMP : 전체 관계성의 내용을 버림

# LOAD => 데이터들에 관계성을 부여함
# '/user~/' => HDFS 경로
# AS ~ 로 각 필드에 이름과 type을 부여할 수 있음
~~~

Pig와 RDB의 차이=> 데이터를 불러올 때 스키마가 적용되는지 여부(pig는 적용됨)



Pig 명령어

~~~
LOAD : 데이터를 관계성으로 불러옴(읽기)
STORE : 데이터를 디스크에 기록함(쓰기)
	EX) STORE ratings(관계성 이름) INTO 'outRatings'(파일이름) USING PigStorage(':');
DUMP : 관계성의 내용을 출력함

FILTER : 필터 역할
DISTINCT : 관계성 내의 고유 값
FOREACH/GENERATE : 기존 관계성의 데이터를 한줄 씩 가져와 변형해 새 관계성 만듦
MAPREDUCE : 관계성에 명시적인 MAPPER와 REDUCER를 호출할 수 있음 => 즉, PIG와 MAPREDUCE를 따로 사용하지 않고 혼합해서 사용 가능함
STREAM : 확장성 관련. PIG의 결과물을 어떤 처리 장치로 표준 입출력을 사용해 스트리밍 할지 정함.
	MAPREDUCE의 STREAMING과 비슷
SAMPLE : 관계성 내에 무작위의 표본을 만듦. JOIN과 사용할 시 주의해야 함
JOIN : 관계성끼리 합쳐줌
COGROUP : JOIN의 변형. JOIN에서는 합쳐진 행을 단일 튜플에 넣는다면, COGROUP은 키별로 서로 다른 튜플에 넣어 계층적 구조를 생성함(JOIN보다 좀 더 구체적임) 좀 더 구조화된 데이터를 만듦.
GROUP : 일종의 REDUCING
CROSS : 두 관계성 사이에서 모든 조합을 함.
CUBE : 두 개 이상의 열을 가져와 모든 조합을 만들 수 있음
ORDER : 관계성을 정렬할 때 사용함.
RANK : ORDER와 비슷. 정렬대신 각 행에 등번호를 부여함
LIMIT : 정해진 만큼의 결과만 가져옴
UNION : 두 개의 관계성을 가져와 합침
SPLIT : 하나 이상의 관계성으로 나눔

DESCRIBE : 주어진 관계성의 스키마를 보여줌. PIG가 그 관계성을 어떻게 보는지 내부의 이름과 유형을 보여줌
EXPLAIN : SQL의 EXPLAIN PLAN과 비슷함. PIG가 주어진 쿼리를 어떻게 실행할지에 관한 계획을 볼 수 있음
ILLUSTRATE : 관계성에서 표본을 가져와 각 데이터 조각들을 가지고 무엇을 하는지 보여줌


사용자 정의 함수(UDF) -> JAVA 코딩을 하고 JAR 파일을 만드는 등의 작업 진행
REGISTER : UDF가 포함된 JAR 파일을 가져오는데 사용함
DEFINE : 그 함수에 이름을 부여해 PIG 스크립트 내에서 사용할 수 있게 함
IMPORT : PIG 파일의 매크로를 불러오는데 사용함

AVG : 평균
CONCAT : 문자열 합치기
COUNT : 카운트 
MAX : 최댓값
MIN : 최솟값
SIZE : 크기
SUM : 더하기

PigStorage : 필드 기반의 데이터 사용. 입출력 각 행에 구분 기호 사용
TextLoader : 입출력 데이터의 한 행에 한 줄씩만 싣음
JsonLoader : json
AvroStorage : serializer or unserializer에 특화
ParquetLoader : 열 지향 데이터 양식
OrcStorage : 압축된 양식
HBaseStorage : pig를 SQL DB인 HBase와 결합할 수 있음
~~~



