## 문자열 계산기에서 lambda와 인터페이스, Map을 활용해 if문 제거

```java
public interface CalculateStrategy {
    int calculate(int a, int b);
}

Map<String, CalculateStrategy> operateMap = new HashMap();

public Calculator(String inputText) {
    operateMap.put("+", (a, b) -> a + b);
    operateMap.put("-", (a, b) -> a - b);
    operateMap.put("*", (a, b) -> a * b);
    operateMap.put("/", (a, b) -> a / b);
    stringParser.setInput(inputText);
}

operateMap.get(operator).calculate(a, b);
```

CalculateStrategy는 calculate라는 1개의 메소드를 가지는 함수형 인터페이스이다.

계산기에서 아래와 같은 부분을 제거하고 싶으면?

```java
if(operator.equals("+")) {
	return add(a, b);
}
if(operator.equals("-")) {
	return minus(a, b);
}
```

+, -의 문자값을 키로 가지는 맵에 CalculateStrategy의 구현체를 넣고 calculate 메소드를 통해 해당 로직을 실행하도록 해서 리팩토링을 할 수 있다.

```java
operateMap.put("+", (a, b) -> a + b);
operateMap.put("-", (a, b) -> a - b);
operateMap.put("*", (a, b) -> a * b);
operateMap.put("/", (a, b) -> a / b);

operateMap.get(operator).calculate(a, b);
```

위의 람다식은 아래와 같이 풀어쓸 수 있다.

```java
(a, b) -> a + b

public class AddStrategy implements CalculateStrategy {
	@Override
    public int calculate(int a, int b) {
    	reutnr a + b;
    }
}
```

a, b 파라미터를 넘겨서 a + b를 리턴함을 람다식을 통해 표현할 수 있다. 원래라면 AddStrategy를 직접 구현해서

```java
public class AddStrategy implements CalculateStrategy {
	@Override
    public int calculate(int a, int b) {
    	reutnr a + b;
    }
}

operateMap.put("+", AddStrategy);
```

위와같이 구현해야 하지만 함수형 인터페이스(메소드를 1개를 가진 인터페이스)의 경우 람다식을 통해 간단히 구현할 수 있다.



## 자동차 경주 게임에 Collection, Stream, lambda 적용해보기

승자를 구하는 로직을 만들 때, 가장 많은 거리를 이용한 자동차를 가져오려면?

우선 먼저 가장 큰 거리를 이동한 자동차를 찾아서 최대값을 구해야하는데.. 기존의 방법으로 구하려면 반복문을 통해 리스트를 순회해서 최대값을 찾아야 한다.

이를 Collections.max()를 이용해 구해보자.

- 가장 많이 이동한 자동차의 이동한 거리값 구하기

max 메소드는 숫자의 경우에는 따로 Comparator(비교 기준 설정)를 구현해주거나 compareTo 메소드를 구현하지 않아도 되는데 우리는 Car라는 클래스를 어떤 기준으로 비교할지가 정해지지 않았기 때문에 이를 정해주어야 하고 이때문에 Comparator를 max 메소드의 두번째 파라미터로 넘겨야 한다.

```java
public static int getMaxDist(List<Car> cars){
        return Collections.max(cars, (car1, car2) -> car1.getPosition() - car2.getPosition()).getPosition();
}
```

위처럼 구현할 수 있는데 `(car1, car2) -> car1.getPosition() - car2.getPosition())` 은 아래와 같이 풀어 쓸 수 있다.

Comparator는 FunctionalInteface로 아래의 하나의 메소드를 가지는데, lambda를 통해 아래와 같이 구현해 줄 수 있다.

```java
(car1, car2) -> car1.getPosition() - car2.getPosition()

//위와 아래의 내용은 같음
public int compareTo(Car car1, Car car2) {
    return car1.getPosition() - cat2.getPosition();
}
```

위처럼 Comparator를 구현해 넘기면 position을 기준으로 max값을 구해 리턴해준다.

- 구한 최대 이동값을 통해 우승자 구하기

이제 우승자를 구하려면 List에서 위의 최대 이동 거리값과 같은 거리를 이동한 자동차 리스트를 받아오면 되는데, 이는 자바8의 Stream을 이용해 간단하게 구할 수 있다.

`cars.stream()`을 통해 stream을 받아오고, `filter()` 를 통해 max값과 같은 car만 가져오도록 필터링을 할 수 있다. `filter()` 는 이름 그대로 필터링을 하는 역할을 한다. 그리고 필터링 된 리스트 스트림에서 `collect(Collectors.toList())` 최종 연산을 통해 winners 리스트를 받아올 수 있다.

```java
public static List<Car> getWinners(List<Car> cars, int max) {
    return cars.stream().filter((car) -> car.isEqualPosition(max)).collect(Collectors.toList());
}
```

