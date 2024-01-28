# codingTest LV1 트레이닝

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
    AND UGB.BOARD_ID = UGR.BOARD_ID
    AND TO_CHAR(UGB.CREATED_DATE, 'mm') = '10'
ORDER BY
    UGR.CREATED_DATE
    , UGB.TITLE;
```
