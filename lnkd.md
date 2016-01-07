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









