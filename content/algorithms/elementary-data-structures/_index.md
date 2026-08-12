---
title: 2. Elementary Data Structures
weight: 2
lastmod: 2026-08-13T00:00:00+07:00
---
{{< katex >}}
## Interfaces
When discussing data structures, it is important to understand the difference between a data structure's interface and its implementation. An interface describes what a data structure does, while an implementation describes how the data structure does it.

An interface, sometimes also called an abstract data type, defines the set of operations supported by a data structure and the semantics, or meaning, of those operations. An interface tells us nothing about how the data structure implements these operations; it only provides a list of supported operations along with specifications about what types of arguments each operation accepts and the value returned by each operation.

A data structure implementation, on the other hand, includes the internal representation of the data structure as well as the definitions of the algorithms that implement the operations supported by the data structure. Thus, there can be many implementations of a single interface. For example, we will see implementations of the Sequence interface using arrays and using pointer-based data structures. Each implements the same interface, Sequence, but in different ways.

### Sequence Interface
Sequences maintain a collection of items in an extrinsic order, where each item stored has a rank in the sequence, including a first item and a last item. By extrinsic, we mean that the first item is ‘first’, not because of what the item is, but because some external party put it there. Sequences are generalizations of stacks and queues, which support a subset of sequence operations. 

List can be represented as a sequence \(x_0, x_1,\dots,x_{n-1}\)

<table>
  <tr>
    <td rowspan="2">Container</td>
    <td style="padding-left: 0px"><code>build(X)</code></td>
    <td>given an iterable <code>X</code>, build sequence from items in <code>X</code></td>
  </tr>
  <tr>
    <td><code>len()</code></td>
    <td>return n, the number of stored items</td>
  </tr>
  <tr>
    <td rowspan="3">Static</td>
    <td style="padding-left: 0px"><code>iter_seq()</code></td>
    <td>return \(x_0, x_1,\dots,x_{n-1}\) in sequence order</td>
  </tr>
  <tr>
    <td><code>get_at(i)</code></td>
    <td>return \(x_i\)</td>
  </tr>
  <tr>
    <td><code>set_at(i, x)</code></td>
    <td>replace the \(x_i\) item with \(x\)</td>
  </tr>
  <tr>
    <td rowspan="6">Dynamic</td>
    <td style="padding-left: 0px"><code>insert_at(i, x)</code></td>
    <td>add \(x\) as the \(i\)th item, displacing \(x_i,\dots,x_{n-1}\);<br>Set \(x_{j+1}=x_j\) for all \(j\in\{n-1,\dots,i\}\), increment \(n\), and set \(x_i=x\)</td>
  </tr>
  <tr>
    <td><code>delete_at(i)</code></td>
    <td>remove and return the \(i\)th item, displacing \(x_i,\dots,x_{n-1}\);<br>Set \(x_j=x_{j+1}\) for all \(j\in\{i,\dots,n-2\}\), decrement \(n\)</td>
  </tr>
  <tr>
    <td><code>insert_first(x)</code></td>
    <td>add \(x\) as the first item (equivalent to <code>insert_at(0,x)</code>)</td>
  </tr>
  <tr>
    <td><code>delete_first()</code></td>
    <td>remove and return the first item (equivalent to <code>delete_at(0)</code>)</td>
  </tr>
  <tr>
    <td><code>insert_last(x)</code></td>
    <td>add \(x\) as the last item (equivalent to <code>insert_at(len(),x)</code>)</td>
  </tr>
  <tr>
    <td><code>delete_last()</code></td>
    <td>remove and return the last item (equivalent to <code>delete_at(len()-1)</code>)</td>
  </tr>
</table>

### Set Interface
By contrast, Sets maintain a collection of items based on an intrinsic property involving what the items are, usually based on a unique key, `x.key`, associated with each item `x`. Sets are generalizations of dictionaries and other intrinsic query databases.

<table>
  <tr>
    <td rowspan="2">Container</td>
    <td style="padding-left: 0px"><code>build(X)</code></td>
    <td>given an iterable <code>X</code>, build set from items in <code>X</code></td>
  </tr>
  <tr>
    <td><code>len()</code></td>
    <td>return \(n\), the number of stored items</td>
  </tr>
  <tr>
    <td rowspan="1">Static</td>
    <td style="padding-left: 0px"><code>find(k)</code></td>
    <td>return the stored item with key \(k\)</td>
  </tr>
  <tr>
    <td rowspan="2">Dynamic</td>
    <td style="padding-left: 0px"><code>insert(x)</code></td>
    <td>add \(x\) to set (replace item with key <code>x.key</code> if one already exists)</td>
  </tr>
  <tr>
    <td><code>delete(k)</code></td>
    <td>delete the stored item with key \(k\)</td>
  </tr>
  <tr>
    <td rowspan="5">Order</td>
    <td style="padding-left: 0px"><code>iter_ord()</code></td>
    <td>return the stored items one-by-one in key order</td>
  </tr>
  <tr>
    <td><code>find_min()</code></td>
    <td>return the stored item with smallest key</td>
  </tr>
  <tr>
    <td><code>find_max()</code></td>
    <td>return the stored item with largest key</td>
  </tr>
  <tr>
    <td><code>find_next(k)</code></td>
    <td>return the stored item with smallest key larger than \(k\)</td>
  </tr>
  <tr>
    <td><code>find_prev(k)</code></td>
    <td>return the stored item with largest key smaller than \(k\)</td>
  </tr>
</table>
(Note that find operations return <code>None</code> if no qualifying item exists.)

## Implementation
### Array Sequence
Computer memory is a finite resource. On modern computers many processes may share the same main memory store, so an operating system will assign a fixed chunk of memory addresses to each active process. The amount of memory assigned depends on the needs of the process and the availability of free memory. For example, when a computer program makes a request to store a variable, the program must tell the operating system how much memory (i.e. how many bits) will be required to store it. To fulfill the request, the operating system will find the available memory in the process’s assigned memory address space and reserve it (i.e. allocate it) for that purpose until it is no longer needed. Memory management and allocation is a detail that is abstracted away by many high level languages including Python, but know that whenever you ask Python to store something, Python makes a request to the operating system behind-the-scenes, for a fixed amount of memory in which to store it.

Now suppose a computer program wants to store two arrays, each storing ten 64-bit words. The program makes separate requests for two chunks of memory (640 bits each), and the operating system fulfills the request by, for example, reserving the first ten words of the process's assigned address space to the first array \(A\), and the second ten words of the address space to the second array \(B\). Now suppose that as the computer program progresses, an eleventh word \(w\) needs to be added to array \(A\). It would seem that there is no space near \(A\) to store the new word: the beginning of the process's assigned address space is to the left of \(A\) and array \(B\) is stored on the right. Then how can we add \(w\) to \(A\)? One solution could be to shift \(B\) right to make room for \(w\), but tons of data may already be reserved next to \(B\), which you would also have to move. Better would be to simply request eleven new words of memory, copy \(A\) to the beginning of the new memory allocation, store \(w\) at the end, and free the first ten words of the process's address space for future memory requests.

A fixed-length array is the data structure that is the underlying foundation of our model of computation (you can think of your computer's memory as a big fixed-length array that your operating system allocates from). Implementing a sequence using an array, where index \(i\) in the array corresponds to item \(i\) in the sequence allows `get_at` and `set_at` to be \(O(1)\) time because of our random access machine. However, when deleting or inserting into the sequence, we need to move items and resize the array, meaning these operations could take linear-time in the worst case. Below is a full Python implementation of an array sequence.

```python
class Array_Seq:
	def __init__(self): # O(1)
		self.A = []
		self.size = 0

	def __len__(self): return self.size # O(1)
	def __iter__(self): yield from self.A # O(n) iter_seq

	def build(self, X): # O(n)
		self.A = [a for a in X] # pretend this builds a static array
		self.size = len(self.A)

	def get_at(self, i): return self.A[i] # O(1)
	def set_at(self, i, x): self.A[i] = x # O(1)

	def _copy_forward(self, i, n, A, j): # O(n)
		for k in range(n):
			A[j + k] = self.A[i + k]

	def _copy_backward(self, i, n, A, j): # O(n)
		for k in range(n - 1, -1, -1):
			A[j + k] = self.A[i + k]

	def insert_at(self, i, x): # O(n)
		n = len(self)
		A = [None] * (n + 1)
		self._copy_forward(0, i, A, 0)
		A[i] = x
		self._copy_forward(i, n - i, A, i + 1)
		self.build(A)

	def delete_at(self, i): # O(n)
		n = len(self)
		A = [None] * (n - 1)
		self._copy_forward(0, i, A, 0)
		x = self.A[i]
		self._copy_forward(i + 1, n - i - 1, A, i)
		self.build(A)
		return x
	
	# O(n)
	def insert_first(self, x): self.insert_at(0, x)
	def delete_first(self): return self.delete_at(0)
	def insert_last(self, x): self.insert_at(len(self), x)
	def delete_last(self): return self.delete_at(len(self) - 1
```

### Linked List
A linked list is a different type of data structure entirely. Instead of allocating a contiguous chunk of memory in which to store items, a linked list stores each item in a node, node, a constant-sized container with two properties: `node.item` storing the item, and `node.next` storing the memory address of the node containing the next item in the sequence.

![Linked List Node](/flbook/images/linkedlistnode.png)

```python
class Linked_List_Node:
	def __init__(self, x): # O(1)
		self.item = x
		self.next = None

	def later_node(self, i): # O(i)
		if i == 0: return self
			assert self.next
		return self.next.later_node(i - 1)
```

Such data structures are sometimes called **pointer-based** or **linked** and are much more flexible than array-based data structures because their constituent items can be stored anywhere in memory. A linked list stores the address of the node storing the first element of the list called the head of the list, along with the linked list’s size, the number of items stored in the linked list. It is easy to add an item after another item in the list, simply by changing some addresses (i.e. relinking pointers). In particular, adding a new item at the front (head) of the list takes \(O(1)\) time. However, the only way to find the \(i\,\)th item in the sequence is to step through the items one-by-one, leading to worst-case linear time for `get_at` and `set_at` operations. Below is a Python implementation of a full linked list sequence

![Linked List](/flbook/images/linkedlist.png)

```python
class Linked_List_Seq:
	def __init__(self): # O(1)
		self.head = None
		self.size = 0

	def __len__(self): return self.size # O(1)

	def __iter__(self): # O(n) iter_seq
		node = self.head
		while node:
			yield node.item
		node = node.next

	def build(self, X): # O(n)
		for a in reversed(X):
			self.insert_first(a)

	def get_at(self, i): # O(i)
		node = self.head.later_node(i)
		return node.item

	def set_at(self, i, x): # O(i)
		node = self.head.later_node(i)
		node.item = x

	def insert_first(self, x): # O(1)
		new_node = Linked_List_Node(x)
		new_node.next = self.head
		self.head = new_node
		self.size += 1

	def delete_first(self): # O(1)
		x = self.head.item
		self.head = self.head.next
		self.size -= 1
		return x

	def insert_at(self, i, x): # O(i)
		if i == 0:
			self.insert_first(x)
			return
		new_node = Linked_List_Node(x)
		node = self.head.later_node(i - 1)
		new_node.next = node.next
		node.next = new_node
		self.size += 1

	def delete_at(self, i): # O(i)
		if i == 0:
			return self.delete_first()
		node = self.head.later_node(i - 1)
		x = node.next.item
		node.next = node.next.next
		self.size -= 1
		return x

	# O(n)
	def insert_last(self, x): self.insert_at(len(self), x)
	def delete_last(self): return self.delete_at(len(self) - 1)
```
### Dynamic Array
The array's dynamic sequence operations require linear time with respect to the length of array \(A\). Is there another way to add elements to an array without paying a linear overhead transfer cost each time you add an element? One straight-forward way to support faster insertion would be to over-allocate additional space when you request space for the array. Then, inserting an item would be as simple as copying over the new value into the next empty slot. This compromise trades a little extra space in exchange for constant time insertion. Sounds like a good deal, but any additional allocation will be bounded; eventually repeated insertions will fill the additional space, and the array will again need to be reallocated and copied over. Further, any additional space you reserve will mean less space is available for other parts of your program

![Inserting Dynamic Array](/flbook/images/dynarray.png)

Then how does Python support appending to the end of a length \(n\) Python List in worst-case \(O(1)\) time? The answer is simple: it doesn't. Sometimes appending to the end of a Python List requires \(O(n)\) time to transfer the array to a larger allocation in memory, so sometimes appending to a Python List takes linear time. However, allocating additional space in the right way can guarantee that any sequence of \(n\) insertions only takes at most \(O(n)\) time (i.e. such linear time transfer operations do not occur often), so insertion will take \(O(1)\) time per insertion on average. We call this asymptotic running time **amortized constant time**, because the cost of the operation is amortized (distributed) across many applications of the operation.

![Amortized Time](/flbook/images/amortized.png)

To achieve an amortized constant running time for insertion into an array, our strategy will be to allocate extra space in proportion to the size of the array being stored. Allocating \(O(n)\) additional space ensures that a linear number of insertions must occur before an insertion will overflow the allocation. A typical implementation of a dynamic array will allocate double the amount of space needed to store the current array, sometimes referred to as table doubling. However, allocating any constant fraction of additional space will achieve the amortized bound. Python Lists allocate additional space according to the following formula (from the Python source code written in C):

```c
new_allocated = (newsize >> 3) + (newsize < 9 ? 3 : 6);
```

Here, the additional allocation is modest, roughly one eighth of the size of the array being appended (bit shifting the size to the right by \(3\) is equivalent to floored division by \(8\)). But the additional allocation is still linear in the size of the array, so on average, \(n/8\) insertions will be performed for every linear time allocation of the array, i.e. amortized constant time.

What if we also want to remove items from the end of the array? Popping the last item can occur in constant time, simply by decrementing a stored length of the array (which Python does). However, if a large number of items are removed from a large list, the unused additional allocation could occupy a significant amount of wasted memory that will not available for other purposes. When the length of the array becomes sufficiently small, we can transfer the contents of the array to a new, smaller memory allocation so that the larger memory allocation can be freed. How big should this new allocation be? If we allocate the size of the array without any additional allocation, an immediate insertion could trigger another allocation. To achieve constant amortized running time for any sequence of \(n\) appends or pops, we need to make sure there remains a linear fraction of unused allocated space when we rebuild to a smaller array, which guarantees that at least \(\Omega(n)\) sequential dynamic operations must occur before the next time we need to reallocate memory.

Below is a Python implementation of a dynamic array sequence, including operations `insert_last` (i.e., Python list `append`) and `delete_last` (i.e., Python list `pop`), using table doubling proportions. When attempting to append past the end of the allocation, the contents of the array are transferred to an allocation that is twice as large. When removing down to one fourth of the allocation, the contents of the array are transferred to an allocation that is half as large. Of course Python Lists already support dynamic operations using these techniques; this code is provided to help you understand how amortized constant `append` and `pop` could be implemented.

```python
class Dynamic_Array_Seq(Array_Seq):
	def __init__(self, r = 2): # O(1)
		super().__init__()
		self.size = 0
		self.r = r
		self._compute_bounds()
		self._resize(0)
	
	def __len__(self): return self.size # O(1)
	
	def __iter__(self): # O(n)
		for i in range(len(self)): yield self.A[i]
	
	def build(self, X): # O(n)
		for a in X: self.insert_last(a)
	
	def _compute_bounds(self): # O(1)
		self.upper = len(self.A)
		self.lower = len(self.A) // (self.r * self.r)

	def _resize(self, n): # O(1) or O(n)
		if (self.lower < n < self.upper): return
		m = max(n, 1) * self.r
		A = [None] * m
		self._copy_forward(0, self.size, A, 0)
		self.A = A
		self._compute_bounds()

	def insert_last(self, x): # O(1)a
		self._resize(self.size + 1)
		self.A[self.size] = x
		self.size += 1

	def delete_last(self): # O(1)a
		self.A[self.size - 1] = None
		self.size -= 1
		self._resize(self.size)

	def insert_at(self, i, x): # O(n)
		self.insert_last(None)
		self._copy_backward(i, self.size - (i + 1), self.A, i + 1)
		self.A[i] = x

	def delete_at(self, i): # O(n)
		x = self.A[i]
		self._copy_forward(i + 1, self.size - (i + 1), self.A, i)
		self.delete_last()
		return x

# O(n)
def insert_first(self, x): self.insert_at(0, x)
def delete_first(self): return self.delete_at(0)
```

<table border="1">
    <tr>
        <th rowspan="3" >Data<br>Structure</th>
        <th colspan="5" style="text-align: center;">Operation, Worst Case \(O(\cdot)\)</th>
    </tr>
    <tr>
        <th style="text-align: center;">Container</th>
        <th style="text-align: center;">Static</th>
        <th colspan="3" style="text-align: center;">Dynamic</th>
    </tr>
    <tr>
        <th><code>build(X)</code></th>
        <th><code>get_at(i)</code><br><code>set_at(i,x)</code></th>
        <th><code>insert_first(x)</code><br><code>delete_first()</code></th>
        <th><code>insert_last(x)</code><br><code>delete_last()</code></th>
        <th><code>insert_at(i, x)</code><br><code>delete_at(i)</code></th>
    </tr>
    <tr>
        <td>Array</td>
        <td class="complexity" type="linear">\(n\)</td>
        <td class="complexity" type="constant">\(1\)</td>
        <td class="complexity" type="linear">\(n\)</td>
        <td class="complexity" type="linear">\(n\)</td>
        <td class="complexity" type="linear">\(n\)</td>
    </tr>
    <tr>
        <td>Linked List</td>
        <td class="complexity" type="linear">\(n\)</td>
        <td class="complexity" type="linear">\(n\)</td>
        <td class="complexity" type="constant">\(1\)</td>
        <td class="complexity" type="linear">\(n\)</td>
        <td class="complexity" type="linear">\(n\)</td>
    </tr>
    <tr>
        <td>Dynamic Array</td>
        <td class="complexity" type="linear">\(n\)</td>
        <td class="complexity" type="constant">\(1\)</td>
        <td class="complexity" type="linear">\(n\)</td>
        <td class="complexity" type="constant">\(1_{(a)}\)</td>
        <td class="complexity" type="linear">\(n\)</td>
    </tr>
</table>
\((a)\) Denotes the amortized running time



## Sources
- [Open Data Structure: Interfaces](https://opendatastructures.org/ods-python/1_2_Interfaces.html)
- [MIT OCW: Introduction to Algorithms](https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-spring-2020/)
