## Enum 활용 로또 추첨 결과 구하기

3개 맞은 경우, 4개 맞은 경우, 5개 맞은 경우 모든 경우에 수에 대해서 `if` 를 통해 분기처리를 해서 결과값을 생성해 줄 수도 있지만 아래처럼 java의 enumeration을 활용해 구현할 수 있다.

```java
public enum LottoMatch {
    MATCH_NO(0, 0),
    MATCH_THREE(3, 5000),
    MATCH_FOUR(4, 50000),
    MATCH_FIVE(5, 1500000),
    MATCH_SIX(6, 200000000);
    private int matchCount;
    private int price;

    public int getPrice() {
        return price;
    }

    LottoMatch(int matchCount, int price) {
        this.matchCount = matchCount;
        this.price = price;
    }

    public static LottoMatch getLottoMatch(int count) {
        return Arrays.stream(LottoMatch.values()).filter(lottoMatch -> lottoMatch.matchCount == count).findFirst().orElse(LottoMatch.MATCH_NO);
    }

}
```

위의 enumeration은 하나도 맞지 않은 경우, 3개 맞은 경우, 4개 맞은 경우, 5개 맞은 경우, 6개 맞은 경우에 대해 구현한 상태이다. 각각의 enum은 LottoMatch라는 타입을 가지게 되며 matchCount를 통해 맞은 갯수, 해당 등수의 당첨금을 저장하게 된다.

```java
public static LottoMatch getLottoMatch(int count) {
    return Arrays.stream(LottoMatch.values()).filter(lottoMatch -> lottoMatch.matchCount == count).findFirst().orElse(LottoMatch.MATCH_NO);
}
```

위의 getLottoMatch는 3을 넘겨주는 경우 `LottoMatch.MATCH_THREE`를 리턴하고, 4를 넘길 경우 4개가 맞은 LottoMatch 타입인 `LottoMatch.MATCH_FOUR` 타입을 리턴해준다. 그리고 0, 1, 2개가 맞는 경우에는 아무 해당사항도 없으므로 `LottoMatch.MATCH_NO`를 리턴해주게 된다.



## Enum을 활용해 LottoGameManager가 가진 lotto 리스트에서 당첨 결과 구하기

구조는 다 다르겠지만, 위의 Enum을 이용해 아래처럼 로또 결과를 가지는 `LottoResult` 클래스를 생성할 수 있다. 이는 `List<LottoMatch>` 를 인스턴스 변수로 가지는 클래스이다.

```java
public LottoResult createLottoResult() {
    List<LottoMatch> lottoMatches = new ArrayList<>();
    for (Lotto lotto : lottos) {
        lottoMatches.add(LottoMatch.getLottoMatch(lotto.getMatchCount(winningLotto)));
    }
    return new LottoResult(lottoMatches);
}
```

위처럼 구현할 경우 1개가 맞은 경우, 2개가 맞은 경우, 4개가 맞은 경우가 있다고 한다면

```java
new LottoResult(Arrays.asList(LottoMatch.MATCH_NO, LottoMatch.MATCH_NO, LottoMatch.MATCH_FOUR));
```

위처럼 당첨 결과가 없는 `LottoMatch.MATCH_NO` 두개, 4개가 맞은 `LottoMatch.MATCH_FOUR` 3개가 저장되게 되고

`LottoResult`에 구현된 아래의 메소드를 통해 해당 갯수가 몇개 맞았는지를 구할 수 있다.

```java
public int getMatchCount(LottoMatch lottoMatch) {
    return (int) lottoMatches.stream().filter(m -> lottoMatch == m).count();
}
```

파라미터로 넘긴 lottoMatch와 일치하는 경우의 count를 리턴한다.

```java
public class ResultView {
    private LottoResult lottoResult;

    public ResultView(LottoResult lottoResult) {
        this.lottoResult = lottoResult;
    }

    public static void printStatResult(LottoResult lottoResult) {
        System.out.println("당첨 통계");
        System.out.println("----------");
        System.out.println("3개 일치 (5000원) - " + lottoResult.getMatchCount(LottoMatch.MATCH_THREE) + "개");
        System.out.println("4개 일치 (50000원) - "  + lottoResult.getMatchCount(LottoMatch.MATCH_FOUR) + "개");
        System.out.println("5개 일치 (1500000원) - "  + lottoResult.getMatchCount(LottoMatch.MATCH_FIVE) + "개");
        System.out.println("6개 일치 (2000000000원) - "  + lottoResult.getMatchCount(LottoMatch.MATCH_SIX) + "개");
        System.out.println("총 수익률은 " + lottoResult.getRate() + "%입니다.");
    }
}

```

위처럼 getMatchCount 메소드를 통해 해당 개수만큼 맞은 경우의 수가 몇개인지를 체크할 수 있음.
