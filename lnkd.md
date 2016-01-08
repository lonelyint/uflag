###### 逆波兰表达式 互换 中序表达式
```java
//5 + ((1 + 2) * 4) − 3
//5 1 2 + 4 * + 3 −
```

###### Valid Number (LC65)
```java
// So basically the number should match this regular expression:
// [-+]?[0-9]*(.[0-9]+)?(e[-+]?[0-9]+)?
public boolean isNumber(String s) {
    s = s.trim();

    boolean pointSeen = false;
    boolean eSeen = false;
    boolean numberSeen = false;
    boolean numberAfterE = true;
    for(int i=0; i<s.length(); i++) {
        if('0' <= s.charAt(i) && s.charAt(i) <= '9') {
            numberSeen = true;
            numberAfterE = true;
        } else if(s.charAt(i) == '.') {
            if(eSeen || pointSeen) {
                return false;
            }
            pointSeen = true;
        } else if(s.charAt(i) == 'e') {
            if(eSeen || !numberSeen) {
                return false;
            }
            numberAfterE = false;
            eSeen = true;
        } else if(s.charAt(i) == '-' || s.charAt(i) == '+') {
            if(i != 0 && s.charAt(i-1) != 'e') {
                return false;
            }
        } else {
            return false;
        }
    }
    return numberSeen && numberAfterE;
}
```
###### Deep Iterator
```java
public class DeepIterator<T> implements Iterator<T> {
	// A reference to the item which will be returned during
	// the next call to next().
	private T nextItem;
	private final Stack<Iterator<?>> stack = new Stack<Iterator<?>>();

	public DeepIterator(Collection<?> collection) {
		if (collection == null) {
			throw new NullPointerException("Can't iterate over a null collection.");
		}
		stack.push(collection.iterator());
	}

	@Override
	@SuppressWarnings("unchecked")
	public boolean hasNext() {
		if (nextItem != null) {
			return true;
		}

		while (!stack.isEmpty()) {
			Iterator<?> iter = stack.peek();
			if (iter.hasNext()) {
				Object item = iter.next();
				if (item instanceof Collection<?>) {
					stack.push(((Collection<?>) item).iterator());
				} else {
					nextItem = (T) item;
					return true;
				}
			} else {
				stack.pop();
			}
		}

		return false;
	}

	@Override
	public T next() {
		if (hasNext()) {
			T toReturn = nextItem;
			nextItem = null;
			return toReturn;
		}

		throw new NoSuchElementException();
	}

	@Override
	public void remove() {
		throw new UnsupportedOperationException();
	}
}
```
###### (LC - Majority element) Find an element occurs more than n/2 or n/3 times in array
###### (LC) Search a 2D sorted array
```java
public class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length;
        int n = matrix[0].length;
        int s = 0, e = m-1;
        while(s < e){
            //this line is very import to break the infinite loop
            //use (s+e+1)/2 instead of (s+e)/2
            int mid = (s+e+1)/2; 
            if(matrix[mid][0] > target) e = mid-1;
            else s = mid;
        }
        
        if(s>e) return false;
        
        int ss = 0, ee = n-1;
        while(ss <= ee){
            int mid = (ss+ee)/2;
            if(matrix[s][mid] < target) ss = mid+1;
            else if(matrix[s][mid] > target) ee = mid-1;
            else return true;
        }
        
        return false;
    }
}
```
```java
public class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int row = 0;
        int col = matrix[0].length-1;
        
        while(row < matrix.length && col >=0){
            int val = matrix[row][col];
            if(val < target) row++;
            else if(val > target) col--;
            else return true;
        }
        
        return false;
    }
}
```
###### (LC) Search for the range
###### (LC) Roman To Integer / Integer to Roman
```java
//roman to integer
public int romanToInt(String s) {
    int res = 0;
    for (int i = s.length() - 1; i >= 0; i--) {
        char c = s.charAt(i);
        switch (c) {
        case 'I':
            res += (res >= 5 ? -1 : 1);
            break;
        case 'V':
            res += 5;
            break;
        case 'X':
            res += 10 * (res >= 50 ? -1 : 1);
            break;
        case 'L':
            res += 50;
            break;
        case 'C':
            res += 100 * (res >= 500 ? -1 : 1);
            break;
        case 'D':
            res += 500;
            break;
        case 'M':
            res += 1000;
            break;
        }
    }
    return res;  
}
```
```java
//integer to roman
public String intToRoman(int num) {
    String M[] = {"", "M", "MM", "MMM"};
    String C[] = {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};
    String X[] = {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};
    String I[] = {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"};
    return M[num/1000] + C[(num%1000)/100] + X[(num%100)/10] + I[num%10];
}
```
###### (LC) Text Justification
```java
public class Solution {
    public List<String> fullJustify(String[] words, int L) {
        List<String> list = new LinkedList<String>();

        for (int i = 0, w; i < words.length; i = w) {
            int len = -1;
            for (w = i; w < words.length && len + words[w].length() + 1 <= L; w++) {
                len += words[w].length() + 1;
            }

            StringBuilder strBuilder = new StringBuilder(words[i]);
            int space = 1, extra = 0;
            if (w != i + 1 && w != words.length) { // not 1 char, not last line
                space = (L - len) / (w - i - 1) + 1;
                extra = (L - len) % (w - i - 1);
            }
            for (int j = i + 1; j < w; j++) {
                for (int s = space; s > 0; s--) strBuilder.append(' ');
                if (extra-- > 0) strBuilder.append(' ');
                strBuilder.append(words[j]);
            }
            int strLen = L - strBuilder.length();
            while (strLen-- > 0) strBuilder.append(' ');
            list.add(strBuilder.toString());
        }

        return list;
    }
}
```
###### (LC) String to integer
```java
public int myAtoi(String str) {
    int result = 0, i = 0, sign = 1;
    if(str == null || str.length() == 0)
        return result;
    char[] arr = str.toCharArray();
    while(arr[i]==' ' && i< arr.length)
        i++;
    if(i==arr.length)
        return result;
    if(arr[i] == '-' || arr[i] == '+'){
        sign = arr[i] == '-'? -1: 1;
        i++;
    }
    int j = i;
    for(; j < str.length(); j++){
        if(arr[j] >'9' || arr[j] <'0')
            break;
        if((j > i) && 
            (Integer.parseInt(str.substring(i, j)) > Integer.MAX_VALUE/10 ||
            (Integer.parseInt(str.substring(i, j)) == Integer.MAX_VALUE/10 &&
              (sign == 1 && arr[j]>='7' || sign == -1&& arr[j] >= '8'))))
            return sign == 1? Integer.MAX_VALUE: Integer.MIN_VALUE;
    }
    result = i==j? 0: Integer.parseInt(str.substring(i, j));
    return result*sign;
}
```
###### (LC) Insert interval
###### (LC) Celebrity
```java
int candidate=0;
for i in [1..n-1] 
    - if(c[i] knows candicate) continue
    - else candidate = c[i]
Another pass to check if candicates knows anyone
```
###### (LC) Build BST from its inorder and post-order
```java
public class Solution {
    private TreeNode helper(int[] postorder, int[] inorder, int ps, int pe, int is, int ie){
        if(ps == pe) return new TreeNode(postorder[ps]);
        
        int pivot = postorder[pe];
        TreeNode tn = new TreeNode(pivot);
        
        int i;
        for(i = is; i <= ie; i++){
            if(inorder[i] == pivot) break;
        }
        
        if(i > is) tn.left = helper(postorder, inorder, ps, ps+(i-is)-1, is, i-1);
        if(i < ie) tn.right = helper(postorder, inorder, ps+(i-is), pe-1, i+1, ie);
        
        return tn;
    }
    
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if(inorder.length == 0) return null;
        return helper(postorder, inorder, 0, postorder.length-1, 0, inorder.length-1);
    }
}
```
```java
//inorder and pre order
public class Solution {
    private TreeNode helper(int[] preorder, int[] inorder, int ps, int pe, int is, int ie){
        if(ps == pe) return new TreeNode(preorder[ps]);
        
        int pivot = preorder[ps];
        
        TreeNode tn = new TreeNode(pivot);
        int i;
        for(i = is; i <= ie; i++){
            if(inorder[i] == pivot) break;
        }
        
        if(i > is) tn.left = helper(preorder, inorder, ps+1, ps+(i-is), is, i-1);
        if(i < ie) tn.right = helper(preorder, inorder, ps+(i-is)+1, pe, i+1, ie);
        
        return tn;
    }
    
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if(preorder.length == 0) return null;
        
        return helper(preorder, inorder, 0, preorder.length - 1, 0, inorder.length - 1);
    }
}
```
###### (LC) First Common Ancestor with parent pointer
###### (LC) Kth element in an array
```java
public class Solution {
    public int findKthLargest(int[] nums, int k) {
        return helper(nums, k, 0, nums.length-1);
    }
    
    private int helper(int[] nums, int k, int s, int e){
        int j=s, v=nums[s];
        swap(nums, s, e);
        for(int i=s;i<e;i++){
            if(nums[i]>v) {
                swap(nums, j, i);
                j++;
            }
        }

        if(j-s+1==k) return nums[e]; 
        else if(j-s+1>k) return helper(nums, k, s, j-1);
        else return helper(nums, k-(j-s+1), j, e-1);
    }
    
    private void swap(int[] nums, int i, int j){
        int tmp = nums[i];
        nums[i]=nums[j];
        nums[j]=tmp;
    }
}
```
###### (LC) Maximum Product Subarray
```java
    //max subarray
    public int maxSubArray(int[] nums) {
        int rlt=nums[0], max_so_far=nums[0];
        for(int i=1;i<nums.length;i++){
            max_so_far = max_so_far < 0 ? nums[i] : max_so_far+nums[i];
            rlt = Math.max(max_so_far, rlt);       
        }    
        return rlt;
    }
    
    //max product subarray
    public int maxProduct(int[] nums) {
        int min_so_far = nums[0];
        int max_so_far = nums[0];
        int rlt = nums[0];
        
        for(int i = 1;i<nums.length;i++){
            int tmp = nums[i];
            
            //t1, t2 here is to save the previous value
            //since min_so_far and max_so_far will be updated during computation
            int t1 = min_so_far;
            int t2 = max_so_far;
            min_so_far = Math.min(Math.min(t2 * tmp, t1 * tmp), tmp);
            max_so_far = Math.max(Math.max(t2 * tmp, t1 * tmp), tmp);
            rlt = rlt > max_so_far ? rlt : max_so_far;
        }
        return rlt;
    }
```
###### Insert/Delete a node in BST (*)
###### Intersection and Union of two arrays 
```c
int printUnion(int arr1[], int arr2[], int m, int n)
{
  int i = 0, j = 0;
  while (i < m && j < n)
  {
    if (arr1[i] < arr2[j])
      printf(" %d ", arr1[i++]);
    else if (arr2[j] < arr1[i])
      printf(" %d ", arr2[j++]);
    else
    {
      printf(" %d ", arr2[j++]);
      i++;
    }
  }
 
  /* Print remaining elements of the larger array */
  while(i < m)
   printf(" %d ", arr1[i++]);
  while(j < n)
   printf(" %d ", arr2[j++]);
}
```
###### Stream one char
每次call read（），返回一个char，如果到头了就是返回-
```java
//输入是个stream
class input_stream
{
    // Character or -1
    int read();
}
```
###### (LC) Paint House I/II
###### factorial digits sum
比如input为10，因为10！= 3628800，就返回sum的值 = 3+6+2+8 ... 用了java的BigInteger class 过了。
###### Shuffle Card in place
###### (LC) Word Ladder
###### (LC) Word Distance
```java
//method without using hashing map
public int shortestWordDistance(String[] words, String word1, String word2) {
        int p1 = -1, p2 = -1, distance = words.length;
        
        for(int i = 0; i<words.length; i++){
            if(words[i].equals(word1)){
                p1 = i;
                if(p1 != -1 && p2 != -1){
                    distance = (p1!=p2) ? Math.min(distance, Math.abs(p1-p2)): distance;
                }
            }
            if(words[i].equals(word2)){
                p2 = i;
                if(p1 != -1 && p2 != -1){
                    distance = (p1!=p2) ? Math.min(distance, Math.abs(p1-p2)): distance;
                }
            }
        }
        return distance;
    }
```
###### Flatten double linked list with parent/child pointers
一个双向链表，带头尾指针，每个节点可能都有父节点和子节点，每个父子节点又是
一个链表。要求把它拍扁，顺序随意。
一开始说了一个类似DFS的算法，他说我的空间复杂度是O(N)，我说递归的方法如果堆
栈空间也算的话确实是O(N)，但他咬定我临时放节点的地方也是O(N)，楞说我存节点需
要分配额外空间，我就很纳闷，这节点都已经是双向链表了，里面有next/prev，为毛
还需要分配O(N)的空间来存这些节点？坚持跟他讨论半天，把节点定义什么都给出来，
一点一点说明白，才证明是他理解有问题，幸好还算坚持，不然就被他带沟里去了。
当然这个算法有更好的解，既然不要求顺序，而且有头尾指针，每次把父子链表接到尾
巴后面就可以了。连递归都省了。
```java
public class Solution {
    public void flattenList(ListNode head) {
        if (head == null) {
            return;
        }
         
        // Step 1: get the tail of the list
        ListNode tail = head;
        while (tail.next != null) {
            tail = tail.next;
        }
         
        // Step 2: iterate from the first layer. If a node has a child, put it to
        // the end of hte list, and update the tail pointer
        ListNode p = head;
         
        while (p != tail) {
            if (p.child != null) {
                ListNode childHead = p.child;
                tail.next = chiidHead;
                 
                // Updat the tail
                while (tail.next != null) {
                    tail = tail.next;
                }
                 
                p.child = null;
            }
             
            p = p.next;
        }
    }
}
```
###### All factors of a number
###### (LC) DNA Sequence
###### (LC) Search in 2D matrix I/II
###### Design a data structure supporting O(1) add, remove, removeRandom
###### (LC) max points on a line
```c++
class Solution {
public:
    int maxPoints(vector<Point> &points) {

        if(points.size()<2) return points.size();

        int result=0;

        for(int i=0; i<points.size(); i++) {

            map<pair<int, int>, int> lines;
            int localmax=0, overlap=0, vertical=0;

            for(int j=i+1; j<points.size(); j++) {

                if(points[j].x==points[i].x && points[j].y==points[i].y) {

                    overlap++;
                    continue;
                }
                else if(points[j].x==points[i].x) vertical++;
                else {

                    int a=points[j].x-points[i].x, b=points[j].y-points[i].y;
                    int gcd=GCD(a, b);

                    a/=gcd;
                    b/=gcd;

                    lines[make_pair(a, b)]++;
                    localmax=max(lines[make_pair(a, b)], localmax);
                }

                localmax=max(vertical, localmax);
            }

            result=max(result, localmax+overlap+1);
        }

        return result;
    }

private:
    int GCD(int a, int b) {

        if(b==0) return a;
        else return GCD(b, a%b);
    }
};
```
###### (LC) 给你一个BST的pre-order traverse的结果，让你返回in-order traverse的结果
###### (LC) Tree level order traversal
```c++
vector<vector<int>> ret;

void buildVector(TreeNode *root, int depth)
{
    if(root == NULL) return;
    if(ret.size() == depth)
        ret.push_back(vector<int>());

    ret[depth].push_back(root->val);
    buildVector(root->left, depth + 1);
    buildVector(root->right, depth + 1);
}

vector<vector<int> > levelOrder(TreeNode *root) {
    buildVector(root, 0);
    return ret;
}
```
```java
public class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        List<List<Integer>> wrapList = new LinkedList<List<Integer>>();

        if(root == null) return wrapList;

        queue.offer(root);
        while(!queue.isEmpty()){
            int levelNum = queue.size();
            List<Integer> subList = new LinkedList<Integer>();
            for(int i=0; i<levelNum; i++) {
                if(queue.peek().left != null) queue.offer(queue.peek().left);
                if(queue.peek().right != null) queue.offer(queue.peek().right);
                subList.add(queue.poll().val);
            }
            wrapList.add(subList);
        }
        return wrapList;
    }
}
```
###### (LC) Edit distance
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
###### Find triangle in an array
Given an array of integers, find out a triple of integers such that
they form a triangle. i.e. given a,b,c from the array, a +b >c, b +c >a, a 
+c >b, 返回任何三个就可以了。 
```java
// Java program to count number of triangles that can be
// formed from given array
import java.io.*;
import java.util.*;
 
class CountTriangles
{
    // Function to count all possible triangles with arr[]
    // elements
    static int findNumberOfTriangles(int arr[])
    {
        int n = arr.length;
        // Sort the array elements in non-decreasing order
        Arrays.sort(arr);
 
        // Initialize count of triangles
        int count = 0;
 
        // Fix the first element.  We need to run till n-3 as
        // the other two elements are selected from arr[i+1...n-1]
        for (int i = 0; i < n-2; ++i)
        {
            // Initialize index of the rightmost third element
            int k = i + 2;
 
            // Fix the second element
            for (int j = i+1; j < n; ++j)
            {
                /* Find the rightmost element which is smaller
                   than the sum of two fixed elements
                   The important thing to note here is, we use
                   the previous value of k. If value of arr[i] +
                   arr[j-1] was greater than arr[k], then arr[i] +
                   arr[j] must be greater than k, because the
                   array is sorted. */
                while (k < n && arr[i] + arr[j] > arr[k])
                    ++k;
 
               /* Total number of possible triangles that can be
                  formed with the two fixed elements is k - j - 1.
                  The two fixed elements are arr[i] and arr[j].  All
                  elements between arr[j+1] to arr[k-1] can form a
                  triangle with arr[i] and arr[j]. One is subtracted
                  from k because k is incremented one extra in above
                  while loop. k will always be greater than j. If j
                  becomes equal to k, then above loop will increment
                  k, because arr[k] + arr[i] is always/ greater than
                  arr[k] */
                count += k - j - 1;
            }
        }
        return count;
    }
 
    public static void main (String[] args)
    {
        int arr[] = {10, 21, 22, 100, 101, 200, 300};
        System.out.println("Total number of triangles is " +
                            findNumberOfTriangles(arr));
    }
}
```
###### Implement addInterval(start, end) and getcoverage()
```java
public interface Intervals {

    /**
    * Adds an interval [from, to] into internal structure.
    */
    void addInterval(int from, int to);


    /**
    * Returns a total length covered by intervals.
    * If several intervals intersect, intersection should be counted only 
once.
    * Example:
    *
    * addInterval(3, 6)
    * addInterval(8, 9)
    * addInterval(1, 5)
    *
    * getTotalCoveredLength() -> 6
    * i.e. [1,5] and [3,6] intersect and give a total covered interval [1,6]
    * [1,6] and [8,9] don't intersect so total covered length is a sum for 
both intervals, that is 6.
    *
    * 0  1  2   3   4   5   6   7   8   9   10
    */
    int getTotalCoveredLength();
}
```
###### Exclusive array
Give an arr1, return a new arr2, arr2[i] is the 
multiplication of all elements in arr1 except arr1[i] 
###### find the intersection of two linked list(do not use hashmap)
###### Replace String
就是实现java里面的String.replace(String find, String replace)
Solution:
We should take advantage that the replacement is only one character in length. Assume that the pattern is at least one character in length, then the replacement’s length will never exceed the pattern’s length. This means that when we overwrite the existing string with replacement, we would never overwrite characters that would be read next.

Two pointers are also used for an efficient in-place replacement, which traverses the string once without any extra memory.
```java
bool isMatch(char *str, const char* pattern) {
  while (*pattern)
    if (*str++ != *pattern++)
      return false;
  return true;
}
 
void replace(char str[], const char *pattern) {
  if (str == NULL || pattern == NULL) return;
  char *pSlow = str, *pFast = str;
  int pLen = strlen(pattern);
  while (*pFast != '\0') {
    bool matched = false;
    while (isMatch(pFast, pattern)) {
      matched = true;
      pFast += pLen;
    }
    if (matched)
      *pSlow++ = 'X';
    // tricky case to handle here:
    // pFast might be pointing to '\0',
    // and you don't want to increment past it
    if (*pFast != '\0')
      *pSlow++ = *pFast++;  // *p++ = (*p)++
  }
  // don't forget to add a null character at the end!
  *pSlow = '\0';
}
```
###### (LC) Min. Window
###### (LC) isomorphic
```java
public boolean isIsomorphic(String s, String t) {
    if(s.length()!=t.length()) return false;
    HashMap<Character, Character> hm = new HashMap<>();
    //hashset may not be needed, since we could use hm.containsValue()
    HashSet<Character> hs = new HashSet(); 
    for(int i=0;i<s.length();i++){
        char cs = s.charAt(i), ct = t.charAt(i);
        if(!hm.containsKey(cs)) {
            if(hs.contains(ct)) return false;
            hm.put(cs, ct);
            hs.add(ct);
        } else {
            if(hm.get(cs) != ct) return false;        
        }
    }
    return true;
}
```
###### find the smallest character that is strictly larger than the search character
###### 判断2个linkedlist是否在某一点会重合. 0(1) space
###### 给一个string，每10个letter一组
输出所有出现次数超过一次的strings with length of 10. 一定要用rolling hashing做
```java
const unsigned PRIME_BASE = 257;
const unsigned PRIME_MOD = 1000000007;

unsigned hash(const string& s)
{
    long long ret = 0;
    for (int i = 0; i < s.size(); i++)
    {
        ret = ret*PRIME_BASE + s[i];
        ret %= PRIME_MOD; //don't overflow
    }
    return ret;
}

int rabin_karp(const string& needle, const string& haystack)
{
    //I'm using long longs to avoid overflow
    long long hash1 = hash(needle);
    long long hash2 = 0;

    //you could use exponentiation by squaring for extra speed
    long long power = 1;
    for (int i = 0; i < needle.size(); i++)
        power = (power * PRIME_BASE) % PRIME_MOD;

    for (int i = 0; i < haystack.size(); i++)
    {
        //add the last letter
        hash2 = hash2*PRIME_BASE + haystack[i];
        hash2 %= PRIME_MOD;

        //remove the first character, if needed
        if (i >= needle.size())
        {
            hash2 -= power * haystack[i-needle.size()] % PRIME_MOD;
            if (hash2 < 0) //negative can be made positive with mod
                hash2 += PRIME_MOD;
        }

        //match?
        if (i >= needle.size()-1 && hash1 == hash2)
            return i - (needle.size()-1);
    }

    return -1;
}
```
###### (LC) string reverse
```java
public class Solution {
    
    private char[] reverse(char[] cs, int s, int e){
        int ss = s;
        int ee = e;
        while(ss < ee){
            char tmp = cs[ss];
            cs[ss++] = cs[ee];
            cs[ee--] = tmp;
        }
        
        return cs;
    }
    
    public String reverseWords(String s) {
        if(s.length() == 0) return s;
        
        String ss = " " + s;
        char[] cs = reverse(ss.toCharArray(), 0, ss.length()-1);
        int start=0, end=0;
        for(int i=0;i<cs.length;i++){
            if(cs[i] != ' '){
                cs[end++] = cs[i];
            } else{
                if(i>0 && (cs[i-1] != ' ')){
                    reverse(cs, start, end-1);
                    cs[end++] = ' ';
                    start = end;
                }
            }
        }
        
        return new String(cs, 0, end > 0  ? end-1: end);
    }
}
```
###### 给定一个undirected graph和一个s节点和一个d节点，判断s和d的距离是否<=3。
距离定义为s和d之间最短路径上link的数目。如果d是s的邻居，则距离为1。 
注意，这个undirected graph使用adjacent array来表示一个节点的所有neighbors，并
且每个节点最多有n个neighbors。每个节点都有一个Idx，并且每个节点的adjacent 
array都是sorted。例如1有邻居2和3，那么1的adjacent array是[2,3](sorted)
直接的BSF解法时间复杂度是O(n^3)。要求设计Solution是时间 O(n^2)。
```
先搜索source点 distance 为2的点集合 O(N^2)，然后用哈希判断
是否和destination点 的O（N） 邻居集合有交集， 复杂度 O(N^2)
这个距离要求是三还是挺好的，要是距离是五什么的就需要双向搜索，代码就很变得很
难了。 
```
###### 设计一个iterator class处理文件line by line
###### given a stream of data and a sliding window, implement put(), getAverage()
考虑multithreading的情况
###### stream里找k个最大数的题，
我还是用了min heap，还好俩人都知道啥是min heap，也比较认同我的做法。然后大部分的时间
都在讨论multi threading的实现，我提到了read write lock，和三种fairness 
policy，不过他俩都是一脸茫然，好像他们只知道read write lock，但不知道
fairness这回事，挺奇怪的。
###### (LC) Wiggle Sort
数组排序， 排成a1<a2>a3<a4>a5.
```java
public class Solution {
    public void wiggleSort(int[] nums) {
        
        for(int i=1;i<nums.length;i++){
            
            if(i%2==1&&nums[i]<nums[i-1]){
                int tmp= nums[i];
                nums[i] = nums[i-1];
                nums[i-1] = tmp;
            }
            else if(i%2==0&&nums[i]>nums[i-1]){
                int tmp= nums[i];
                nums[i] = nums[i-1];
                nums[i-1] = tmp;
            }
        }
        
    }
}
```
###### (LC) Max Sum Path in Binary Tree
```java
public class Solution {
    int max;
    
    public int maxPathSum(TreeNode root) {
        max = Integer.MIN_VALUE;
        maxSoFar(root);
        return max;
    }
    
    public int maxSoFar(TreeNode root){
        if(root == null) return 0;
        
        int left = Math.max(0, maxSoFar(root.left));
        int right = Math.max(0, maxSoFar(root.right));
        
        max = Math.max(max, left+right+root.val);
        return left > right ? left+root.val : right+root.val;
    }
    
}
```








