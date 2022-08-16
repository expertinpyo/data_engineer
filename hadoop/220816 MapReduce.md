# MapReduce

HDFS, YARN과 더불어 Hadoop의 핵심 기술임



MapReduce란

Hadoop에 내제된 기능

클러스터에 데이터의 처리를 분배함

데이터를 파티션으로 나눠 클러스터에 걸쳐 병렬처리 되도록 함

=> 이 과정을 관리하고 문제가 생겼을 때 처리하는 역할



데이터를 Mapping(변형) 하고 Reducing(집계)함

데이터가 한 줄씩 들어오면 Mapper는 그 데이터를 변형시킴

들어오는 데이터에서 필요한 정보를 추출하고 이해할 수 있는 구조로 정리함

모든 input line(입력)마다 Mapper는 중요한 데이터를 추출하고 구조화 해 한 줄을 출력함



Reducer는 데이터를 집계함

데이터 Mapping시, 특정한 key-value값과 연관시킴





Mapper의 주된 목적 : 필요한 정보를 추출하고, 적절하게 구조화하는 것



## 작동 원리

[1] 입력 데이터 형식 변환 , mapping

Key - Value

이 단계에서는 같은 키가 여러번 나타나도 괜찮음



[2] Shuffle and Sort

그 이후 자동으로 각 고유키에게 주어진 값들이 집계됨

한 키에 여러 벨류가 묶이는 형태로 변하게 됨 => 그 이후 키에 대한 정렬 발생



MapReduce : 클러스터 전체에 걸친 모든 값을 집계하고, 한 자리에 가져와 필요에 따라 처리하는 것에 있음



[3] Reducing

집계와 정렬이 된 데이터들을 원하는대로 처리할 수 있음





## MapReduce의 분산 처리 방법

Mapping 단계에서 각 Node는 정해진 할당량만큼 일함

=> 따라서, 각 Node의 작업이 독립적임



Shuffle and Sort 과정에서는 여러 Node에서 같은 key에 대한 데이터들이 나올 수 있음

=> 이러면, 같은 key들끼리 모아서 reducer로 보내야 함

이 때, MapReduce는 Merge Sort, 합병 정렬을 진행함



그 이후, Reducing 단계에서 각 Node는 주어진 Key set을 reducing함



최종 결과는 Client Node에게 전달됨



정리하자면, Mapping 단계에서는 입력 데이터를 덩어리로 쪼개 여러 Node(컴퓨터)로 보냄

=> Shuffle and Sort 단계에서는 여러 컴퓨터가 각각의 key - Value 세트를 정렬하고,

Reducing 단계에서는 각 Node들이 할당된 key set에 대한 Reducing을 진행함



### Hadoop 내부 처리  과정

1. Client Node가 MapReduce 작업 지시

2. Client는 YARN Resource Manager(어떤 Node에서 무엇을 실행할지 관리하는 역할)와 통신함

3. 이 때, HDFS에서는 필요한 데이터를 복사함 => Why? 이 작업을 수행할 Node들이 데이터에 접근할 수 있게끔 하기 위해!

4. Node Manmager 아래에 있는 MapReduce Application Master가 작동

   => Application Manager는 각각의 Mapping, Reducing 작업을 관찰 + YARN과 협업해 작업을 클러스터에 걸쳐 배분함

   

정리하자면, 클라이언트 노드에서 작업을 개시하고, 리소스 관리자는 다른 컴퓨터들의 가용 여부 등을 추적함

애플리케이션 마스터는 작업 진행을 관리하고, 노드 매니저는 개별 PC를 관리함

이 때, 모든 노드들은 HDFS 클러스터의 데이터를 사용함

리소스 관리자는 매핑이나 리듀싱 작업을 최대한 데이터와 가까운 곳에서 실행함

=> HDFS에서는 데이터 복사가 일어나는데, 리소스 관리자는 작업이 최대한 원본이 담겨져 있는 곳에서 일어나게끔 유도하고, 그게 불가능하다면 네트워크에 최대한 가까이 두려고 노력함 => 불필요한 네트워크 전송을 줄일 수 있음



### Map Reduce 함수

Map, Reduce는 java로 작성됨(Hadoop 자체가 java로 작성됨)

Streaming을 통해 Python을 활용해 해당 작업을 진행할 수 있게 됨



###  Failure Handling 하기

거대한 클러스터는 범용 하드웨어가 종종 다운된다는 단점이 있음

=> 이 때 어플리케이션 관리자가 재실행 등의 작업을 진행



모든 노드(어플리케이션 관리자 포함)가 다운되면?

=> YARN 리소스 관리자가 재시작할 수 있음



리소스 관리자가 다운되면?

=> MapReduce는 Zookeeper와 이야기해 어떤 다른 리소스 관리자를 사용할지 알아냄

=> Zookeeper는 리소스 관리자가 다운될 시, 자동으로 백업 리소스 관리자를 가리키는 역할 수행

 

### 그 외 MapReduce 기능

Counter : 클러스터 전반에 걸쳐 공유된 총 수를 유지하는 기능

Combiner : Mapper 노드를 줄여 최적화함 => 비용 절약 가능



python에서는 return 대신 yield로 generator를 만들어 관리함

=> 성능과 메모리 차원에서 높은 이득을 가짐

from mrjob.job import MRJob
from mrjob.step import MRStep

class RatingsBreakdown(MRJob):
        def steps(self):
                return [
                        MRStep(mapper=self.mapper_get_ratings,
                                reducer=self.reducer_count_ratings)
                        ]
        def mapper_get_ratings(self, _, line):
                (user_ID, movie_ID, rating, timestamp) = line.split('\t')
                yield rating, 1
        def reducer_count_ratings(self, key, values):
                yield key, sum(values)

if __name__ == '__main__':
        RatingsBreakdown.run()