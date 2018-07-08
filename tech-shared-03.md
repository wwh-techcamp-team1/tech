## Java 8 Stream API

#### 스트림이란?
   * 데이터 처리 연산을 지원하도록 소스에서 추출된 연속된 요소이다.
   * 다양한 데이터 소스를 표준화된 방법으로 다루기 위한 라이브러리이다.
   * 데이터 소스를 추상화하고, 데이터를 다루는데 자주 사용되는 메서드들을 정의해 놓았다.
   * 데이터소스를 추상화하였다는 것은, 데이터 소스가 무엇이든 같은 방식으로 다룰 수 있게 되었다는 것과 코드의 재사용성이 높아진다는 것을 의미한다.

#### 스트림의 특징
1. 스트림은 데이터 소스를 변경하지 않는다.
    * 스트림은 데이터 소스로 부터 데이터를 읽기만할 뿐, 데이터 소스를 변경하지 않는다.
      필요하다면, 정렬된 결과를 컬렉션이나 배열에 담아서 반환할 수 있다.
2. 스트림은 일회용이다.
    * 스트림 한번 사용하면 닫혀서 다시 사용할 수 없다. 필요하다면 스트림을 다시 생성해야한다.
3. 스트림은 작업 내부를 반복적으로 처리한다.
    * 스트림을 이용한 작업이 간결할 수 있는 비결중의 하나가 바로 '내부 반복'이다.
      내부 반복이라는 것은 반복문을 메서드의 내부에 숨길 수 있다는 것을 의미한다.
4. 지연된 연산
    * 스트림 연산에서는 최종 연산이 수행되기 전까지는 중간 연산이 수행되지 않는다.
      스트림에 대해 `sort()`나 `distinct()`같은 중간 연산을 호출해도 즉각적으로 수행되지 않는다는 것이다.
      중간연산을 호출하는 것은 단지 어떤 작업이 수행되어야하는지를 지정해주는 것일 뿐이다.
      최종연산이 수행되어서야 스트림의 요소들이 중간연산을 거치고 최종연산에 소모된다.
5. 기본형 스트림
    * 오토박싱, 언박싱으로 인한 비효율을 줄이기 위해 데이터 소스의 요소를 기본형으로 다루는 `IntStream`, `LongStream`, `DoubleStream` 이 제공된다.
      일반적으로 `Stream< Integer>` 대신 `IntStream`을 사용하는 것이 더 효율적이고, `IntStream`에는 `int`타입으로 작업하는데 유용한 메서드들이 포함되어있다.
6. 병렬 스트림
    * 스트림은 내부적으로 fork&join framework를 이용해서 연산을 자동적으로 병렬로 수행한다.
      `parallel()` 메서드를 호출하면 병렬로 연산이 수행되고, `sequential()` 메서드를 호출하면 병렬로 처리되지 않게 된다.
      모든 스트림은 기본적으로 병렬 스트림이 아니기 때문에 `sequential()` 메서드는 `parallel()`를 취소할 때만 사용한다.

#### 스트림의 사용법

   `orders.스트림 생성().중간 연산().최종 연산()` 
   1. 스트림을 생성한다.
   2. 초기 스트림을 다른 스트림으로 변환하는 중간 연산(intermediate operation)들을 하나 이상의 단계로 지정한다.
   3. 결과를 산출하기 위해 최종 연산(terminal operation)을 적용한다. 
       이 연산은 앞선 지연 연산(lazy operation)들의 실행을 강제한다. 
       이후로는 해당 스트림을 더는 사용할 수 없다.
       
#### 스트림의 연산
   * 중간 연산
   | 중간 연산 | 설명 |
   | :---------- | :--------- |
   | `Stream < T > distinct()` | 중복 제거 |
   | `Stream < T > filter(Predicate < T > predicate)` | 조건에 안 맞는 요소 제외 |
   | `Stream < T > limit(long maxSize)` | 스트림의 일부 잘라내기 |
   | `Stream < T > skip(ling n)` | 스트림의 일부 건너뛰기 |
   | `Stream < T > peek(Consumer< T > action)` | 스트림의 요소에 작업수행 |
   | `Stream < T > sorted()` | 스트림의 요소 정렬 |

   * 최종 연산
   | 최종 연산 | 설명 |
   | :---------- | :--------- |
   | `void forEach(Consumer <? super T> action)` | 각 요소에 지정된 작업 수행 |
   | `long count()` | 스트림의 요소 개수 |
   | `Optional < T > max (Comparator <? super T> comparator)` | 스트림의 최댓값 |
   | `Optional < T > min (Comparator <? super T> comparator)`	 | 스트림의 최솟 |
   | `Optional < T > findAny()` | 아무거나 하나 |
   | `Optional < T > findFirst()` | 스트림의 첫번째 요소 |
   | `Optional < T > reduce (BinaryOperator < T > accumulator)` | 스트림의 요소를 하나씩 줄여가면서 계산하고 최종결과를 반환 |
   | `boolean allMatch(Pradicate < T > p)` | 모두 만족하는지? |
   | `boolean anyMatch(Pradicate < T > p)` | 하나라도 만족하는지? |
   | `boolean noneMatch(Pradicate < T > p)` | 모두 만족하지 않는지? |
   | `Object[] toArray()` | 스트림의 모든 요소를 배열로 반환 |