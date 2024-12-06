## 문제 설명

- 얀에서는 매년 달리기 경주가 열립니다. 해설진들은 선수들이 자기 바로 앞의 선수를 추월할 때 추월한 선수의 이름을 부릅니다.
  예를 들어 1등부터 3등까지 "mumu", "soe", "poe" 선수들이 순서대로 달리고 있을 때, 해설진이 "soe"선수를 불렀다면 2등인 "soe" 선수가 1등인 "mumu" 선수를 추월했다는 것입니다.
  즉 "soe" 선수가 1등, "mumu" 선수가 2등으로 바뀝니다.
  선수들의 이름이 1등부터 현재 등수 순서대로 담긴 문자열 배열 players와 해설진이 부른 이름을 담은 문자열 배열 callings가 매개변수로 주어질 때, 경주가 끝났을 때 선수들의 이름을 1등부터 등수 순서대로 배열에 담아 return 하는 solution 함수를 완성해주세요.

## 제한사항

- 5 ≤ players의 길이 ≤ 50,000
  - players[i]는 i번째 선수의 이름을 의미합니다.
  - players의 원소들은 알파벳 소문자로만 이루어져 있습니다.
  - players에는 중복된 값이 들어가 있지 않습니다.
  - 3 ≤ players[i]의 길이 ≤ 10
- 2 ≤ callings의 길이 ≤ 1,000,000
  - callings는 players의 원소들로만 이루어져 있습니다.
  - 경주 진행중 1등인 선수의 이름은 불리지 않습니다.

## 입출력 예

| players                               | callings                       | result                                |
| ------------------------------------- | ------------------------------ | ------------------------------------- |
| ["mumu", "soe", "poe", "kai", "mine"] | ["kai", "kai", "mine", "mine"] | ["mumu", "kai", "mine", "soe", "poe"] |

## 입출력 예 설명

- 입출력 예 #1

- 4등인 "kai" 선수가 2번 추월하여 2등이 되고 앞서 3등, 2등인 "poe", "soe" 선수는 4등, 3등이 됩니다.
  5등인 "mine" 선수가 2번 추월하여 4등, 3등인 "poe", "soe" 선수가 5등, 4등이 되고 경주가 끝납니다.
  1등부터 배열에 담으면 ["mumu", "kai", "mine", "soe", "poe"]이 됩니다.

## 코드 설명

```jsx
import java.util.*;

class Solution {
    public String[] solution(String[] players, String[] callings) {
        // 선수 이름과 위치를 매핑하는 HashMap 생성
        //예: {"mumu": 0, "soe": 1, "poe": 2, "kai": 3, "mine": 4}
        Map<String, Integer> playerPositions = new HashMap<>();
        for (int i = 0; i < players.length; i++) {
            playerPositions.put(players[i], i);
        }
        // {
        //   "mumu" -> 0,  // mumu는 1등 (배열의 첫 번째)
        //   "soe"  -> 1,  // soe는 2등 (배열의 두 번째)
        //   "poe"  -> 2,  // poe는 3등
        //   "kai"  -> 3,  // kai는 4등
        //   "mine" -> 4   // mine은 5등
        // }

        // callings를 처리하며 위치를 업데이트
        for (String called : callings) {
            int currentPosition = playerPositions.get(called); // 호출된 선수의 현재 위치 [예 3]
            if (currentPosition > 0) {
                int newPosition = currentPosition - 1; // 추월한 위치 [3-1] = 2

                // players 배열 스왑
                String swappedPlayer = players[newPosition];
                // 새로운 변수에 players의 2(poe)를 담아준다
                players[newPosition] = called;
                // plauers[2] 자리에 called 호출된 선수의 kai 이름을 가져와 담는다
                players[currentPosition] = swappedPlayer;
                // players[3] 자리에 poe를 담아준다.

                // HashMap 업데이트
                playerPositions.put(called, newPosition);
                // 여기서 새로운 등수의 플레이어를 담아준다.
                playerPositions.put(swappedPlayer, currentPosition);
                // 추월당한 플에이어를 새로운 곳에 다시 담아준다.
            }
        }

        return players;
    }
}
```

## 알고리즘 내용

- HashMap을 통해 O(1) 시간 복잡도로 위치를 관리하므로, callings 배열이 매우 길더라도 효율적으로 처리할 수 있습니다.
- 입력 데이터 크기가 크므로, 반복문과 스왑 작업이 누적될 때의 성능을 고려해 최적화를 유도합니다.
