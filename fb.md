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
###### Integer to English Word
```java
public class Solution {
    private final String[] lessThan20 = {"", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine", "Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"};
    private final String[] tens = {"", "Ten", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};
    private final String[] thousands = {"", "Thousand", "Million", "Billion"};
    
    public String numberToWords(int num) {
        if (num == 0) return "Zero";
        int i = 0;
        String words = "";
    
        while (num > 0) {
            if (num % 1000 != 0) words = helper(num % 1000) + thousands[i] + " " + words;
            num /= 1000;
            i++;
        }
        return words.trim();
    }
    
    private String helper(int num) {
        if (num == 0) return "";
        else if (num < 20) return lessThan20[num] + " ";
        else if (num < 100) return tens[num / 10] + " " + helper(num % 10);
        else return lessThan20[num / 100] + " Hundred " + helper(num % 100);
    }
}
```
###### Randomly return the index of max elements in an array  
Shuffle cards in place  
http://www.1point3acres.com/bbs/thread-123655-1-1.html  
http://www.1point3acres.com/bbs/thread-120864-3-1.html
```java
//reservoir sampling
public int reservSampling(int[] nums){
	int max=nums[0];
	int cnt=1;
	int rt = 0;
	Random rand = new Random();
	for(int i=1;i<nums.length;i++){
		if(nums[i]==max){
			cnt++;
			rt = (rand.nextInt(cnt)+1==cnt) ? i : rt;
		} else if(nums[i] > max){
			max=nums[i];
			cnt=1;
			rt = i;
		}
	}
	return rt;
}
```
###### Binary Tree Vertical Traversal
```java
class ZipNode{
	List<Integer> val;
	ZipNode prev, next;
	public ZipNode(){
		val = new ArrayList<>();
	}
}

public class Solution {
	public void verticalPrint(TreeNode root){
		ZipNode z = new ZipNode();
		helper(root, z);
		ZipNode curr = z;
		while(curr.prev !=null){
			curr = curr.prev;
		}
		while(curr != null){
			System.out.println(curr.val);
			curr = curr.next;
		}
	}
    
	private void helper(TreeNode root, ZipNode z){
		z.val.add(root.val);
		if(root.left != null) {
			if(z.prev == null){
				ZipNode p = new ZipNode();
				z.prev = p;
				p.next = z;
			}
			helper(root.left, z.prev);
		}
		if(root.right != null) {
			if(z.next == null){
				ZipNode r = new ZipNode();
				z.next = r;
				r.prev = z;
			}
			helper(root.right, z.next);
		}
	}
}
```
###### Divide two integers without multiplication, division and mod operator
```java
```
