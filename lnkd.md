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
###### (LC) Search for the range
###### (LC) Roman To Integer / Integer to Roman
###### (LC) Text Justification
###### (LC) String to integer
###### (LC) Insert interval
###### (LC) Celebrity 
###### (LC) Build BST from its inorder and post-order
###### (LC) First Common Ancestor with parent pointer
###### (LC) Kth element in an array
###### (LC) Maximum Product Subarray
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
###### (LC) Level order tree traversal
###### All factors of a number
###### (LC) DNA Sequence
###### (LC) Search in 2D matrix I/II
###### Design a data structure supporting O(1) add, remove, removeRandom
###### (LC) max points on a line
###### (LC) 给你一个BST的pre-order traverse的结果，让你返回in-order traverse的结果
###### (LC) Tree level order traversal
###### (LC) Edit distance
###### Find triangle in an array
Given an array of integers, find out a triple of integers such that
they form a triangle. i.e. given a,b,c from the array, a +b >c, b +c >a, a 
+c >b, 返回任何三个就可以了。 
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
###### (LC) Min. Window
###### (LC) isomorphic
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
###### 给定一个undirected graph和一个s节点和一个d节点，判断s和d的距离是否<=3。
距离定义为s和d之间最短路径上link的数目。如果d是s的邻居，则距离为1。 
注意，这个undirected graph使用adjacent array来表示一个节点的所有neighbors，并
且每个节点最多有n个neighbors。每个节点都有一个Idx，并且每个节点的adjacent 
array都是sorted。例如1有邻居2和3，那么1的adjacent array是[2,3](sorted)
直接的BSF解法时间复杂度是O(n^3)。要求设计Solution是时间 O(n^2)。
###### 设计一个iterator class处理文件line by line
###### given a stream of data and a sliding window, implement put(), getAverage()
考虑multithreading的情况
###### stream里找k个最大数的题，
我还是用了min heap，还好俩人都知道啥是min heap，也比较认同我的做法。然后大部分的时间
都在讨论multi threading的实现，我提到了read write lock，和三种fairness 
policy，不过他俩都是一脸茫然，好像他们只知道read write lock，但不知道
fairness这回事，挺奇怪的。
###### 数组排序， 排成a1<a2>a3<a4>a5.
###### (LC) Max Sum Path in Binary Tree
###### (LC) Reverse words in a string
















