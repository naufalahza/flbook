---
title: 1. Algorithms Intro
---
Thumbnail: {{% highlight "#FF000036" "#FF0000" %}}[Dread Crystal Recipe from Divine Journey 2](https://github.com/Divine-Journey-2/Divine-Journey-2){{% /highlight %}}

{{< katex >}}
## Algorithm
An {{% highlight "#0BCDAA36" "#0BCDAA" %}}<b>algorithm</b>{{% /highlight %}} is a sequence of steps or instructions that transform the input into the output to solve a problem

## Correctness
### Mathematical Induction
**Mathematical induction** is a method for proving that a statement \(P(n)\) is true for every natural number \(n\), that is, that the infinitely many cases \(P(0),P(1),P(2),P(3),\dots\) all hold. This is done by first proving a simple case, then also showing that if we assume the claim is true for a given case, then the next case is also true.

### Loop Invariant
A **loop invariant** is a condition that is true at the beginning and end of every iteration of a loop. Loop invariants help us understand why an algorithm is correct. When you’re using a loop invariant, you need to show three things:

>[!note] **Initialization:**
>The invariant is true before a first iteration of the loop.

>[!note] **Maintenance:**
>If the invariant is true before an iteration of the loop, then the invariant stays true after iteration finishes.

>[!note] **Termination:**
>When the loop terminates, the invariant gives us a useful property that helps show that the algorithm is correct.

A loop-invariant proof is a form of **mathematical induction**, where to prove that a property holds, you prove a base case and an inductive step. Here, showing that the invariant holds before the first iteration corresponds to the base case, and showing that the invariant holds from iteration to iteration corresponds to the inductive step.

The third property is perhaps the most important one, since you are using the loop invariant to show correctness. Typically, you use the loop invariant along with the condition that caused the loop to terminate. Mathematical induction typically applies the inductive step infinitely, but in a loop invariant the "induction" stops when the loop terminates.

# Efficiency
What makes a computer program efficient? One program is said to be more efficient than another if it can solve the same problem input using fewer resources. We expect that a larger input might take more time to solve than another input having smaller size. In addition, the resources used by a program, e.g. storage space or running time, will depend on both the algorithm used and the machine on which the algorithm is implemented. We expect that an algorithm implemented on a fast machine will run faster than the same algorithm on a slower machine, even for the same input. We would like to be able to compare algorithms, without having to worry about how fast our machine is.

For a concrete example, let us pit a faster computer (computer A) running insertion sort against a slower computer (computer B) running merge sort. They each must sort an array of 10 million numbers. (Although 10 million numbers might seem like a lot, if the numbers are eight-byte integers, then the input occupies about 80 megabytes, which fits in the memory of even an inexpensive laptop computer many times over.) Suppose that computer A executes 10 billion instructions per second (faster than any single sequential computer at the time of this writing) and computer B executes only 10 million instructions per second (much slower than most contemporary computers), so that computer A is 1000 times faster than computer B in raw computing power. To make the difference even more dramatic, suppose that the world's craftiest programmer codes insertion sort in machine language for computer A, and the resulting code requires \(2n^2\) instructions to sort \(n\) numbers. Suppose further that just an average programmer implements merge sort, using a high-level language with an inefficient compiler, with the resulting code taking \(50n \lg n\) instructions. To sort 10 million numbers, computer A takes

$$
\begin{aligned}&\frac{2\cdot(10^7)^2\text{ instructions}}{10^{10}\text{ instructions/second}}=20,000\text{ seconds (more than 5.5 hours)}&\end{aligned}
$$



while computer B takes
$$
\begin{aligned}&\frac{50\cdot10^7\lg10^7\text{ instructions}}{10^{7}\text{ instructions/second}}\approx1163\text{ seconds (under 20 minutes)}&\end{aligned}
$$

By using an algorithm whose running time grows more slowly, even with a poor compiler, computer B runs more than 17 times faster than computer A! The advantage of merge sort is even more pronounced when sorting 100 million numbers: where insertion sort takes more than 23 days, merge sort takes under four hours. Although 100 million might seem like a large number, there are more than 100 million web searches every half hour, more than 100 million emails sent every minute, and some of the smallest galaxies (known as [ultra-compact dwarf galaxies](https://en.wikipedia.org/wiki/Dwarf_galaxy#Ultra-compact_dwarfs)) contain about 100 million stars. In general, as the problem size increases, so does the relative advantage of merge sort.

# Asymptotic Notations
O-notation characterizes an _upper bound_ on the asymptotic behavior of a function. In other words, it says that a function grows no faster than a certain rate, based on the highest-order term.

> [!definition] O-notation
> $$\begin{aligned} & O(g(n)) = \{ f(n) : \text{there exist positive constants } c \text{ and } n_0 \text{ such that } \\ & 0 \le f(n) \le c\,g(n) \text{ for all } n \ge n_0 \}&
\end{aligned}$$

Ω-notation characterizes a _lower bound_ on the asymptotic behavior of a function. In other words, it says that a function grows at least as fast as a certain rate, based—as in O-notation on the highest-order term.

> [!definition] Ω-notation
> $$\begin{aligned} & \Omega(g(n)) = \{ f(n) : \text{there exist positive constants } c \text{ and } n_0 \text{ such that } \\ & 0 \le c\,g(n) \le f(n) \text{ for all } n \ge n_0 \}\end{aligned}$$

Θ-notation characterizes a _tight bound_ on the asymptotic behavior of a function. It says that a function grows precisely at a certain rate, based—once again—on the highest-order term.

> [!definition] Θ-notation
> $$\begin{aligned} & \Theta(g(n)) = \{ f(n) : \text{there exist positive constants } c_1,c_2, \text{and } n_0 \text{ such that } \\ & 0 \le c_1g(n) \le f(n) \le c_2g(n) \text{ for all } n \ge n_0 \}\end{aligned}$$

> [!theorem]
> $$\begin{aligned} & \text{For any two functions } f(n) \text{ and } g(n), \text{ we have } f(n) =\Theta(g(n)) \text{ if and only if } \\ & f(n) = O(g(n)) \text{ and } f(n) = \Omega(g(n))\end{aligned}$$

![Asymptotic Notation Graph](/images/clrs-asymptotic-notation.png)

**Common asymptotic notations**
| Function Name | Asymptotic Notation         |
| ------------- | --------------------------- |
| constant      | \(\Theta(1)\)               |
| logatithmic   | \(\Theta(\lg n)\)           |
| linear        | \(\Theta(n)\)               |
| log-linear    | \(\Theta(n\lg n)\)          |
| quadratic     | \(\Theta(n^2)\)             |
| polynomial    | \(\Theta(n^c)\)             |
| exponential   | \(\Theta(2^{\Theta(n^c)})\) |

## Model of Computation
In order to precisely calculate the resources used by an algorithm, we need to model how long a computer takes to perform basic operations. Specifying such a set of operations provides a **model of computation** upon which we can base our analysis. We will use the \(w\)-bit **Word-RAM** (RAM in this case stands for Random Access Machine) model of computation, which models a computer as a random access array of machine words called memory, together with a processor that can perform operations on the memory. A machine word is a sequence of \(w\) bits representing an integer from the set \(\{0,\dots,2^w−1\}\).

A Word-RAM processor can perform basic binary operations on two machine words in constant time, including addition, subtraction, multiplication, integer division, modulo, bitwise operations, and binary comparisons. In addition, given a word \(a\), the processor can read or write the word in memory located at address \(a\) in constant time. If a machine word contains only \(w\) bits, the processor will only be able to read and write from at most \(2^w\) addresses in memory. So when solving a problem on an input stored in \(n\) machine words, we will always assume our Word-RAM has a word size of at least \(w\ge\lg n\) bits, or else the machine would not be able to access all of the input in memory. To put this limitation in perspective, a Word-RAM model of a byte-addressable 64-bit machine \((w=64)\) allows inputs up to \(\sim10^{10}\) GB in size.

![Simple 64 Bit CPU](/images/simple-64bit-cpu.png)

## Data Structure
A {{% highlight "#90CAF936" "#90CAF9" %}}<b>data structure</b>{{% /highlight %}} is a way to store and organize data in order to facilitate access and modifications. Using the appropriate data structure or structures is an important part of algorithm design. No single data structure works well for all purposes, and so you should know the strengths and limitations of several of them.


## Sources
- [Book: Introduction to Algorithms Fourth Edition ](https://mitpress.mit.edu/9780262046305/introduction-to-algorithms/)
- [Wikipedia: Mathematical Induction](https://en.wikipedia.org/wiki/Mathematical_induction)
- [MIT OCW: Introduction to Algorithms](https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-spring-2020/)
- [Open Data Structures: Model of Computation](https://opendatastructures.org/ods-python/1_4_Model_Computation.html)
