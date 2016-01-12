###### Flatten multi-layer linked list
###### Subset II
###### First Good/Bad Version
###### Valid Palindrom
###### Minimum-window-substring with set
###### 
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
```java
public void printTopDown(TreeNode root){
	TreeMap<Integer, List<TreeNode>> hm = new TreeMap<>();
	helper(root, hm, 0);
	for(Map.Entry<Integer, List<TreeNode>> e : hm.entrySet()){
		System.out.println(e.getValue());
	}
}

private void helper(TreeNode root, TreeMap<Integer, List<TreeNode>> hm, int col){
	if(root == null) return;
	if(hm.containsKey(col)) {
		List<TreeNode> lt = hm.get(col);
		lt.add(root);
	} else {
		List<TreeNode> lt = new ArrayList<>();
		lt.add(root);
		hm.put(col, lt);
	}
	
	helper(root.left, hm, col-1);
	helper(root.right, hm, col+1);
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
###### Convert Binary Tree to double linked list (how about single linked list?)
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
###### Buy and Sell Stock 1234
###### Using Stack to implement a Queue (vise versa)
###### 3-sum (Hashtable)
```java
  // Complexity - O(n^2), Space Complexity - O(n^2)
    private int[] findTriple_3(int[] A) {
        Map<Integer, int[]> map = new HashMap<Integer, int[]>();
        for (int i = 0, l = A.length; i < l; i++) {
            map.clear();
            for (int j = i + 1; j < l; j++) {
                if (map.containsKey(A[j])) {
                    int[] pair = map.get(A[j]);
                    return new int[]{pair[0], pair[1], A[j]};
                } else
                    map.put(-A[i] - A[j], new int[]{A[i], A[j]});
            }
        }
        return null;
    }
```
###### 给a list of segment，求出与其他segment相交最多的segment
###### mplement ／ using ＋
###### Trie and w**d
###### climbing stairs
###### 两个字符串，计算长度至少为k的公共子字符串个数
###### Linked List sort
###### 判断二叉树是否为合法的二叉排序树
```java
//two solutions: min/max, prev 
```
###### 输入一个数组，其中有n个非负整数  
求将它们重新排列顺序之后，连在一起所能组成的最大数字
```
//The idea is to use any comparison based sorting algorithm. 
//write a comparison function myCompare() and use it to sort numbers. 
//Given two numbers X and Y, how should myCompare() decide which number to put first – 
//we compare two numbers XY (Y appended at the end of X) and YX (X appended at the end of Y). 
//If XY is larger, then X should come before Y in output, else Y should come before. 
//For example, let X and Y be 542 and 60. To compare X and Y, we compare 54260 and 60542. 
//Since 60542 is greater than 54260, we put Y first.
```
###### 9,8,3,8,8,5,2,9
找出这串数字中长度为‘k’的subsequence（不是subarray, 我专门问了，就是subsequence，不一定挨着的元素序列），使得这串subsequence的和最大
###### Edit Distance
```java
public class Solution {
    public int minDistance(String word1, String word2) {
        int n1 = word1.length();
        int n2 = word2.length();
        int[][] wk = new int[n1+1][n2+1];
        
        for(int i=0;i<=n1;i++) wk[i][0] = i;
        for(int j=0;j<=n2;j++) wk[0][j] = j;
        
        for(int i=1;i<=n1;i++){
            for(int j=1;j<=n2;j++){
                if(word1.charAt(i-1) == word2.charAt(j-1)){
                    wk[i][j] = wk[i-1][j-1];
                } else{
                    wk[i][j] = Math.min(wk[i-1][j-1], Math.min(wk[i][j-1], wk[i-1][j]))+1;
                }
            }
        }
        
        return wk[n1][n2];
    }
}
```
###### find k closest points to a given point in 2D plane
