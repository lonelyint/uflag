###### Add Binary
```java
public class Solution {
    public String addBinary(String a, String b) {
        int i=a.length()-1, j=b.length()-1;
        int carry=0;
        StringBuilder sb = new StringBuilder();
        
        while(i>=0 || j>=0){
            int ca=0, cb=0;
            if(i>=0) ca = a.charAt(i)-'0';
            if(j>=0) cb = b.charAt(j)-'0';
            sb.insert(0, (ca+cb+carry)%2);
            carry = (ca+cb+carry)/2;
        }
        
        if(carry!=0) sb.insert(0, carry);
        return sb.toString();
    }
}
```
