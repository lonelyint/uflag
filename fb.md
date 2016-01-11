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
###### Parse a formula String
```c++
//leetcode 224 (basic calculator)
//http://www.fgdsb.com/2015/01/08/parse-formula/
double evaluate(const string& f, double x_val) {
    double sum_y_left = 0, sum_y_right = 0;
    double sum_num_left = 0, sum_num_right = 0;
    double *cur_sum_y = &sum_y_left, *cur_sum_num = &sum_num_left;
    int last_op = 1, bracket_op = 1;
    stack<int> brackets;
   
    for(int i = 0; i <= f.size(); ++i) {
        char c = f[i];
        if(f[i] >= '0' && f[i] <= '9') {
            int over = i + 1;
            while(f[over] >= '0' && f[over] <= '9') ++over;
            double number = atoi(f.substr(i, over-i).c_str()) * last_op * bracket_op;
           
            if(f[over] == 'x') {
                *cur_sum_num += number * x_val;
                i = over;
            } else if(f[over] == 'y') {
                *cur_sum_y += number;
                i = over;
            } else {
                *cur_sum_num += number;
                i = over - 1;
            }
        } else if( c == 'x' ) {
            *cur_sum_num += last_op * bracket_op * x_val;
        } else if( c == 'y' ){
            *cur_sum_y += last_op * bracket_op;
        } else if( c == '(' ) {
            brackets.push(last_op);
            bracket_op *= last_op;
            last_op = 1;
        } else if( c == ')' ) {
            bracket_op /= brackets.top();
            brackets.pop();
        } else if( c == '+' || c == '-' ) {
            last_op = c == '+' ? 1 : -1;
        } else if( c == '=' || c == '\0' ) {
            cur_sum_num = &sum_num_right;
            cur_sum_y = &sum_y_right;
            last_op = 1;
            brackets = stack<int>();
        }
    }
    return (sum_num_right - sum_num_left) / (sum_y_left - sum_y_right);
}
```
###### Convert Binary Tree to double linked list
```java
public void convert(TreeNode root, TreeNode prev, TreeNode head){
	if(root==null) return;
	convert(root.left, prev, head);
	root.left = prev;
	if(prev!=null)prev.right = root;
	else head = root;
	TreeNode right = root.right;
	head.left = root;
	root.right = head;
	prev = root;
	convert(right, prev, head);
}
```
###### Find Peak
###### All root to leaf path
###### Weighted random selection
###### BT Level Traverse
