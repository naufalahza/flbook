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

## Asymptotic Notations
O-notation characterizes an _upper bound_ on the asymptotic behavior of a function. In other words, it says that a function grows no faster than a certain rate, based on the highest-order term.

> [!definition] O-notation
> $$\begin{aligned} & O(g(n)) = \{ f(n) : \text{there exist positive constants } c \text{ and } n_0 \text{ such that } \\ & 0 \le f(n) \le c\,g(n) \text{ for all } n \ge n_0 \}&
\end{aligned}$$

Ω-notation characterizes a *lower bound* on the asymptotic behavior of a function. In other words, it says that a function grows at least as fast as a certain rate, based—as in O-notation on the highest-order term.

> [!definition] Ω-notation
> $$\begin{aligned} & \Omega(g(n)) = \{ f(n) : \text{there exist positive constants } c \text{ and } n_0 \text{ such that } \\ & 0 \le c\,g(n) \le f(n) \text{ for all } n \ge n_0 \}\end{aligned}$$

Θ-notation characterizes a *tight bound* on the asymptotic behavior of a function. It says that a function grows precisely at a certain rate, based—once again—on the highest-order term.

> [!definition] Θ-notation
> $$\begin{aligned} & \Theta(g(n)) = \{ f(n) : \text{there exist positive constants } c_1,c_2, \text{and } n_0 \text{ such that } \\ & 0 \le c_1g(n) \le f(n) \le c_2g(n) \text{ for all } n \ge n_0 \}\end{aligned}$$

> [!theorem]
> $$\begin{aligned} & \text{For any two functions } f(n) \text{ and } g(n), \text{ we have } f(n) =\Theta(g(n)) \text{ if and only if } \\ & f(n) = O(g(n)) \text{ and } f(n) = \Omega(g(n))\end{aligned}$$

### Asymptotic Notation Graph
<div style="display: flex; flex-wrap: wrap; justify-content: space-evenly; align-items: flex-end; gap: 1px; text-align: center; font-family: 'Times New Roman', serif; background-color: white; padding: 10px 0; border-radius: 10px;">
	<!-- (a) Big-O -->
	<div style="display: flex; flex-direction: column; align-items: center;">
		<svg viewBox="0 0 240 200" width="240" height="200">
			<!-- n0 Line -->
			<line x1="53" y1="175" x2="53" y2="94" stroke="#d95f02" stroke-width="1.5" />
			<text x="45" y="190" font-style="italic">n<tspan baseline-shift="sub" font-size="0.7em">0</tspan></text>
			<!-- Axes -->
			<polyline points="0,0 0,175 175,175" fill="none" stroke="black" stroke-width="1.2" />
			<text x="178" y="180" font-style="italic">n</text>
			<!-- cg(n) -->
			<path d="M 0 143 C 11 134 24 125 32 117 C 38 109 44 101 53 95 C 66 84 84 79 110 69 C 140 57 156 36 177 17" fill="none" stroke="black" stroke-width="1.2" />
			<text x="161" y="12" font-style="italic">cg(n)</text>  
			<!-- f(n) -->
			<path d="M 0 127 C 6 130 10 132 12 139 C 13 143 15 147 18 149 C 24 151 28 110 38 88 C 41 82 45 82 49 88 C 55 95 56 117 72 119 C 82 117 96 98 108 95 C 132 84 154 77 178 71" fill="none" stroke="#0072B2" stroke-width="1.5" />
			<text x="170" y="67" font-style="italic">f(n)</text>
		</svg>
		<div style="margin-top: 10px; color: black">
			<span>\(f(n) = O(g(n))\)</span><br>
			<span>\((a)\)</span>
		</div>
	</div>
	<!-- (b) Omega -->
	<div style="display: flex; flex-direction: column; align-items: center;">
		<svg viewBox="0 0 240 200" width="240" height="200">
			<!-- n0 Line -->
			<line x1="53" y1="175" x2="53" y2="107" stroke="#d95f02" stroke-width="1.5" />
			<text x="45" y="190" font-style="italic">n<tspan baseline-shift="sub" font-size="0.7em">0</tspan></text>
			<!-- Axes -->
			<polyline points="0,0 0,175 175,175" fill="none" stroke="black" stroke-width="1.2" />
			<text x="178" y="180" font-style="italic">n</text>
			<!-- cg(n) -->
			<path d="M 0 126 Q 14 123 31 118 C 38 117 43 108 53 106 C 61 106 69 108 79 106 C 107 98 137 91 177 88" fill="none" stroke="black" stroke-width="1.2" />
			<text x="160" y="84" font-style="italic">cg(n)</text>
			<!-- f(n) -->
			<path d="M 0 142 C 4 133.3333 8 124.6667 12 116 C 14 111 17 111 20 116 C 24 122 25 128 31 137 C 34 139 35 139 38 137 C 47.3333 118.6667 55 100 66 82 C 71 73 78 70 87 68 C 117 62 146.3333 53.3333 176 46" fill="none" stroke="#0072B2" stroke-width="1.5" />
			<text x="163" y="43" font-style="italic">f(n)</text>
		</svg>
		<div style="margin-top: 10px; color: black">
			<span>\(f(n) = \Omega(g(n))\)</span><br>
			<span>\((b)\)</span>
		</div>
	</div>
	<!-- (c) Theta -->
	<div style="display: flex; flex-direction: column; align-items: center;">
		<svg viewBox="0 0 240 200" width="240" height="200">
			<!-- n0 Line -->
			<line x1="43" y1="175" x2="43" y2="100" stroke="#d95f02" stroke-width="1.5" />
			<text x="35" y="190" font-style="italic">n<tspan baseline-shift="sub" font-size="0.7em">0</tspan></text>
			<!-- Axes -->
			<polyline points="0,0 0,175 175,175" fill="none" stroke="black" stroke-width="1.2" />
			<text x="178" y="180" font-style="italic">n</text>	  
			<!-- c1g(n) -->
			<path d="M 0 175 Q 17 157 23 150 C 30 142 37 139 48 137 C 55 136 57 135 68 136 C 79 133 87 125 95 118 C 117 104 148 102 174 96" fill="none" stroke="black" stroke-width="1.2" />
			<text x="154" y="92" font-style="italic">c<tspan baseline-shift="sub" font-size="0.7em">1</tspan>g(n)</text>
			<!-- c2g(n) -->
			<path d="M 0 175 C 12 150 27 113 37 104 C 45 99 57 94 67 96 C 86 88 90 61 107 48 C 117 36 154 25 176 16" fill="none" stroke="black" stroke-width="1.2" />
			<text x="154" y="13" font-style="italic">c<tspan baseline-shift="sub" font-size="0.7em">2</tspan>g(n)</text>
			<!-- f(n) -->
			<path d="M 0 142 C 10 144 20 139 28 148 C 32 152 36 152 39 148 C 43 142 44 131 49 127 C 62 108 96 100 112 94 C 136 87 166 71 174 60" fill="none" stroke="#0072B2" stroke-width="1.5" />
			<text x="163" y="56" font-style="italic">f(n)</text>
		</svg>
		<div style="margin-top: 10px; color: black">
			<span>\(f(n) = \Theta(g(n))\)</span><br>
			<span>\((c)\)</span>
		</div>
	</div>
</div>

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
