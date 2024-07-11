---
title: 담당자 배정 로직의 전환
author: leo
categories: [Blogging]
tags: [redis, queue]
render_with_liquid: false
typora-root-url: ../
---

**Written by. Leo**

RDS에서 Redis 로의 전환

기존의 담당자 배정 로직은 RDS를 사용하여 관리되었습니다.

RDS는 데이터의 영속성과 복잡한 쿼리 기능을 제공하지만, 담당자 배정과 같은 빈번한 읽기/쓰기 작업을 처리하는 데는 적합하지 않다고 생각했습니다.

## RDS로 관리했을 때의 문제점

1. 성능 문제
    - 담당자 배정 시스템은 빈번한 읽기 / 쓰기 작업이 발생하는데, 트랜잭션 처리와 잠금 메커니즘으로 인해 높은 동시성 트랜잭션에서 성능 저하가 발생할 수 있습니다.
2. 복잡성
    - 담당자 배정의 큐 형태로 FIFO 특성을 유지하기 위해 큐의 상태를 정확히 추척하여 처리하기 위한 조회 및 업데이트 로직이 필요합니다.
3. 확장성 
    - RDS는 수평적 확장이 어렵습니다. 분산 환경에서 일관성을 유지하기 어렵고 여러 데이터베이스 인스턴스를 사용하면 관리자 복잡해질 수 있습니다.

## Redis로 전환한 이유

Redis는 인메모리 데이터 구조 스토어로, 높은 성능과 담당자 배정 로직 구현 케이스에서 RDS 대비 비교적 간단한 사용법으로 배정 로직을 구현할 수 있습니다.

1. 높은 성능 
    - Redis는 모든 데이터를 메모리에 저장하므로, 매우 빠른 읽기/쓰기 성능을 제공합니다. 이는 빈번한 담당자 배정 작업 처리에 유리합니다.
2. 간단한 설정 및 사용
    - Redis는 간단한 API와 명령어를 제공하여 쉽게 사용이 가능합니다. 이번 스테프앱 토이 프로젝트에서 기존 RDS 시스템을 빠르게 대체 할 수 있었습니다.
3. 다양한 데이터 구조 지원
    - Redis는 List, Set, Hash 등 다양한 데이터 구조를 지원합니다. 담당자 배정 큐를 구현할 때 리스트 자료구조를 사용하여 효율적으로 관리 할 수 있었습니다.
4. 확장성
    - Redis는 클러스터링을 통해 수평적으로 확장할 수 있어, 높은 트래픽을 처리할 수 있습니다. 이는 RDS 보다 유연한 확장성을 제공합니다.

## RDS와 Redis의 차이점

| 특징 | RDS | Redis |
| --- | --- | --- |
| 데이터 저장 방식 | 디스크 기반 저장 | 인메모리 저장 |
| 성능 | 느린 읽기/쓰기 성능 | 매우 빠른 읽기//쓰기 성능 |
| 복잡성 | 복잡한 트랜잭션 및 쿼리 | 간단한 명령어와 API |
| 확장성 | 수평적 확장 어려움 | 클러스터링을 통한 수평적 확장 용이 |
| 데이터 영속성 | 높은 영속성 | 영속성 옵션 존재 (RDB, AOF) |

## Redis 기술 소개

Redis는 “Remote Dictionary Server”의 약자로, 인메모리 데이터 구조 스토어 입니다. Redis는 다양한 데이터 구조를 제공하여 캐싱, 메시지 브로커, 세션 저장소 등 다양한 용도로 사용될 수 있습니다.

### 주요 특징

1. 다양한 데이터 구조
    - List : 큐와 스택 구현에 사용
        - 담당자 배정시 사용했던 데이터 구조
    - Set : 중복되지 않는 요소들의 집합
    - Hash : 필드와 값의 쌍으로 구성된 컬렉션
    - Sorted sets : Sets 자료구조에 `score`라는 값을 추가로 두어 해당 값을 기준으로 순서를 유지
    - 비트맵, 하이퍼로그 등
2. 고성능
    - Redis는 인메모리 데이터베이스로, 낮은 지연 시간과 높은 처리량을 제공합니다. 이는 실시간 데이터 처리가 필요한 앱에 적합니다.
3. 복제 및 영속성
    - Redis는 데이터 복제를 지원하여 고가용성을 제공하며, RDB와 AOF를 통해 데이터 영속성을 보장할 수 있습니다.
4. Pub/Sub 메커니즘
    - Redis는 발행 / 구독 모델을 지원하여, 실시간 메시징 및 알림 시스템을 쉽게 구현할 수 있습니다.
5. 클러스터링
    - Redis 클러스팅을 사용하여 데이터 분산 및 확장을 지원합니다. 이는 대규모 트래픽을 처리하는데 유리합니다.

## Redis로 전환한 담당자 배정 로직

Redis를 사용하여 담당자 배정 로직을 구현한 방법은 다음과 같습니다.

1. 상담 대기 큐 추가
    1. 배정할 수 있는 담당자가 없을때, 요청을 상담대기 큐로 넣음
    2. 상담 카테고리 아이디 - Key
    3. Task 아이디 - Value
2. 상담 작업 할당
    1. 상담 종료 API 로직내에 존재하며, 상담 종료시 상담 카테고리 아이디로 레디스 조회
    2. 카테고리 키로 조회된 레디스 값 리스트에 Task 아이디가 존재하면 추출
    3. 추출된 값을 리스트에서 제거
3. 추출한 Task 아이디로 상담자 배정

## 구현 예제

아래는 Redis를 사용하여 기존 Common - CacheUtil에서 리스트로 상담사 배정 큐를 처리라는 메서드를 추가 스태프 앱 전용 Redis Util입니다.

```java
@Component
public class StaffRedisUtil {

    private static RedisTemplate<String, String> redisTemplate;

    public StaffRedisUtil(RedisTemplate<String, String> redisTemplate) {
        StaffRedisUtil.redisTemplate = redisTemplate;
    }

    public static void enqueueTask(Long taskCategoryId, Long taskId) {
        String key = "staff:assignTask:" + taskCategoryId;
        redisTemplate.opsForList().rightPush(key, String.valueOf(taskId));
    }

    public static Long dequeueTask(Long taskCategoryId) {
        String redisKey = "staff:assignTask:" + taskCategoryId;
        String taskId = redisTemplate.opsForList().leftPop(redisKey);
        return taskId != null ? Long.parseLong(taskId) : null;
    }

    public static boolean exists(String key) {
        return Boolean.TRUE.equals(redisTemplate.hasKey(key));
    }
}
```

1. **enqueueTask 메서드**: 상담 대기 큐에 작업을 추가할 때 사용합니다. 카테고리 ID를 키로 하여, 해당 리스트의 가장 오른쪽에 Task ID를 추가합니다. 이를 통해 배정 우선순위를 설정합니다.
2. **exists 메서드**: 상담 종료 로직에서 상담 대기 큐가 존재하는지 확인하는 데 사용합니다. 큐가 존재하면 `true`, 존재하지 않으면 `false`를 반환합니다.
3. **dequeueTask 메서드**: 큐에서 작업을 가져올 때 사용합니다. 카테고리 ID를 키로 하여, 해당 리스트의 가장 왼쪽에 있는 Task ID를 추출하고, 이를 리스트에서 제거합니다. 추출된 Task ID는 상담사에게 배정됩니다.

참고 블로그글 

[RDS VS Redis](https://f-lab.kr/insight/redis-vs-rdb)

[Redis 클러스터링](https://kjw1313.tistory.com/118)

[Redis 영속성 (RDB / AOF)](https://inpa.tistory.com/entry/REDIS-%F0%9F%93%9A-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%98%81%EA%B5%AC-%EC%A0%80%EC%9E%A5%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%9D%98-%EC%98%81%EC%86%8D%EC%84%B1)

[Redis 데이터 구조](https://sabarada.tistory.com/134)

[Redis 관련 디테일 정보](https://velog.io/@dodozee/Redis)