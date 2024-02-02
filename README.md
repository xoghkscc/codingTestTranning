# codingTest LV1 트레이닝

## 24.02.02

### 1번 문제
![image](https://github.com/xoghkscc/codingTestLv1Tranning/assets/82793713/9285a9e7-d6db-4c8a-91e2-6aabec01bb25)

```java
import java.util.*;

class Solution {
    public int[] solution(String today, String[] terms, String[] privacies) {
        
        // terms를 map으로 변환 ex) A 6 => {A=6}
        HashMap<String, Integer> map = new HashMap<>();
        
        for(int i = 0; i < terms.length; i++){
            String[] term = terms[i].split(" ");
            map.put(term[0], Integer.parseInt(term[1]));
        }

        //오늘 날짜 .빼고 숫자로 바꾸기
        int todayInt = Integer.parseInt(today.replace(".", ""));
        
        //폐기해야할 날짜를 찾는다면 개인정보 번호 넣기
        ArrayList<Integer> list = new ArrayList<>();
        
        for(int i = 0; i < privacies.length; i++){
            String[] privacy = privacies[i].split(" ");
            String yyyymmdd = privacy[0].replace(".", "");
            
            int year = Integer.parseInt(yyyymmdd.substring(0, 4));
            int month = Integer.parseInt(yyyymmdd.substring(4, 6));
            int day = Integer.parseInt(yyyymmdd.substring(6, 8));
            
            
            int plusMonth = map.get(privacy[1]);
            
            month = month + plusMonth;
            
            //달이 12를 넘어가면 1을 빼주고 년 1 추가
            while(month > 12){
                year++;
                month = month - 12;
            }
            
            //day가 1이라면 day를 28로 하고 month를 1 감소
            if(day == 1) {
                day = 28;
                month--;
                if(month == 0){ //감소한 month가 0이라면 년 1 감소 month 12
                    year--;
                    month = 12;
                }
            } else {
                day--;
            }
            
            StringBuilder sb = new StringBuilder();
            sb.append(year+"");
            
            //month 및 day가 한자리 수라면 0 붙이기 ex) day:9 -> 09
            if(month < 10){
                sb.append("0" + month);
            } else {
                sb.append(month);
            }
            
            if(day < 10){
                sb.append("0" + day);
            } else {
                sb.append(day);
            }
            
            if(todayInt > Integer.parseInt(sb.toString())) list.add(i+1);
            
        }
    
        int[] answer = new int[list.size()];
        
        for(int i = 0; i < list.size(); i++){
            answer[i] = list.get(i);
        }

        return answer;
    }
}
```

### 2번 문제
![image](https://github.com/xoghkscc/codingTestLv1Tranning/assets/82793713/a0e73a7d-21b4-457b-a7b2-97fdb3d21f91)

```java
class Solution {
    public int[] solution(String[] park, String[] routes) {
        
        int x_point = 0;
        int y_point = 0;
        
        //S의 좌표 찾기
        for(int y = 0; y < park.length; y++){
            String parkStr = park[y];
            for(int x = 0; x < parkStr.length(); x++){
                if(parkStr.charAt(x) == 'S'){
                    x_point = x;
                    y_point = y;
                    
                    y = park.length;
                    break;
                }
            }
        }
        
        System.out.println("초기 x좌표 : " +x_point);
        System.out.println("초기 y좌표 : " +y_point);
        
        for(String route : routes){
            //벗어나거나 X를 만날경우 되돌릴 수 있도록 백업
            int backup_x_point = x_point;
            int backup_y_point = y_point;
            
            String direction = route.split(" ")[0];
            int distance = Integer.parseInt(route.split(" ")[1]);
            
            for(int k = 0; k < distance; k++){
                if(direction.equals("N")) y_point--;
                if(direction.equals("S")) y_point++;
                if(direction.equals("W")) x_point--;
                if(direction.equals("E")) x_point++;
                
                //1. 벗어난 경우 backup 좌표로 돌린 뒤 다음 route로 이동
                if(x_point < 0 || y_point < 0 || x_point >= park[0].length() || y_point >= park.length) {
                    x_point = backup_x_point;
                    y_point = backup_y_point;
                    break;
                //2. X를 만난 경우 backup 좌표로 돌린 뒤 다음 route로 이동
                } else if(park[y_point].charAt(x_point) == 'X'){
                    x_point = backup_x_point;
                    y_point = backup_y_point;
                    break;
                }
            }
            System.out.println("이동한 좌표 : [" + y_point +", " + x_point +"]");
        }
        
        int[] answer = new int[2];
        answer[0] = y_point;
        answer[1] = x_point;
        return answer;
    }
}
```

### 3번 문제
![image](https://github.com/xoghkscc/codingTestLv1Tranning/assets/82793713/3fbe5167-eaa1-4061-ab80-29ef1494678e)

```java
-- 코드를 입력하세요
SELECT
    UGB.BOARD_ID
    , UGB.WRITER_ID
    , UGB.TITLE
    , UGB.PRICE
    , CASE WHEN UGB.STATUS = 'SALE' THEN '판매중'
           WHEN UGB.STATUS = 'RESERVED' THEN '예약중'
           ELSE '거래완료' END AS STATUS
FROM
    USED_GOODS_BOARD UGB
WHERE
    TO_CHAR(UGB.CREATED_DATE, 'YYYYMMDD') = '20221005'
ORDER BY
    UGB.BOARD_ID DESC;
```

## 24.01.28

### 1번 문제
![image](https://github.com/xoghkscc/codingTestLv1Tranning/assets/82793713/4a09cc13-034b-4173-88e6-013bfbbc770d)

```java
import java.util.*;

class Solution {
    public String solution(String new_id) {
        //1단계 소문자로 치환
        new_id = new_id.toLowerCase(); 
        
        //2단계
        HashSet<String> set = new HashSet<>();
        
        set.add("-");
        set.add("_");
        set.add(".");
        
        StringBuilder sb = new StringBuilder();
        
        for(int i = 0; i < new_id.length(); i++){
            //알파벳, 숫자, 빼기, 밑줄, 마침표인 단어에 경우 sb에 append함
            if((new_id.charAt(i)>= 'a' && new_id.charAt(i)<= 'z') || (new_id.charAt(i)>= '0' && new_id.charAt(i)<= '9') || set.contains(new_id.charAt(i) + "")){
                sb.append(new_id.charAt(i));
            }
        }
        
        new_id = sb.toString();
        
        sb = new StringBuilder();
        
        //3단계
        for(int i = 0; i < new_id.length(); i++){
            //문자 중 .을 만난다면 그 다음 문자또한 .이라면 sb에 append하지 않고 넘어감 단 i가 마지막 번째 문자열임과 동시에 .이라면 무조건 삭제(4단계 일부)
            if(!((new_id.charAt(i) == '.') && (new_id.charAt(Math.min(i+1, new_id.length()-1)) == '.'))){
                sb.append(new_id.charAt(i));
            }
        }

        //4단계
        //.이 첫째자리라면 첫째자리 자름
        if(sb.indexOf(".") == 0){
            sb.delete(0, 1);
        }
        
        new_id = sb.toString();
        
        //5단계
        if(new_id.isEmpty()) new_id = "a";
        
        //6단계
        if(new_id.length() >= 16){
            new_id = new_id.substring(0, 15);
            //이때 마지막 문자가 .일 경우 반복 제거하는 코드
            while(true){
                if(new_id.charAt(new_id.length()-1) == '.'){
                    new_id = new_id.substring(0, new_id.length() -1);
                } else {
                    break;
                }
            }
            
        }

        //7단계
        while(true){
            if(new_id.length() <= 2){
                new_id += new_id.charAt(new_id.length()-1);
            } else {
                break;
            }
        }
    
        return new_id;
    }
}
```

### 2번 문제
![image](https://github.com/xoghkscc/codingTestLv1Tranning/assets/82793713/1ae2621a-5e91-423e-a810-90f765cfdb26)

```sql
--oracle

SELECT
    UGB.TITLE
    , UGB.BOARD_ID
    , UGR.REPLY_ID
    , UGR.WRITER_ID
    , UGR.CONTENTS
    , TO_CHAR(UGR.CREATED_DATE, 'yyyy-mm-dd') CREATED_DATE
FROM
    USED_GOODS_BOARD UGB
    , USED_GOODS_REPLY UGR
WHERE 1=1
    AND UGB.BOARD_ID = UGR.BOARD_ID --ANSI 쿼리를 적극 활용하자.
    AND TO_CHAR(UGB.CREATED_DATE, 'mm') = '10' --TO_CHAR를 하기 되면 INDEX가 적용이 안됨
ORDER BY
    UGR.CREATED_DATE
    , UGB.TITLE;
```
