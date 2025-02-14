# The last algorithms course you'll need

## Basics

### Big O Time Complexity

- A way to categorize algorithms time or memory requirements based on input
- Generalizes the growth of the algorithm (i.e., time will grow linearly, as your input grows, how fast does computation or memory grow?)
- Used to decide which data structure to use => knowing how each data structure will perform will help decide which to use
- sometimes we can exchange time for memory, but in the real world that's not always true
- Example

```ts
function sum_char_codes(n: string): number {
    let sum = 0;
    for (let i = 0; i < n.length; ++i) {
        sum += n.charCodeAt(i);
    }

    return sum;
}
```

- the algo above counts the number of chars in a string
- it grows linearly because for each new char, the for loop has to run once more => O(n)
- tip: **look for loops**!

```ts
function sum_char_codes(n: string): number {
    let sum = 0;
    for (let i = 0; i < n.length; ++i) {
        sum += n.charCodeAt(i);
    }

    for (let i = 0; i < n.length; ++i) {
        sum += n.charCodeAt(i);
    }

    return sum;
}
```

- what about the one above? O(2n)?
- not really, **constants are dropped**
- this happens because the idea behind big o is to give you an idea of how memory or time grow depending on input
- it is specially important for the upper bounds, where constants become irrelevant
- think of, for instance, the difference between an algo that grows linearly and one that grows exponentially and how different they'd get in their upper bounds
- remember, the question is "how does it grow?"
- also, keep in mind that big o is about general terms and that not all theory holds in practice
  - n is faster than n^2 but for some specific cases (specific algo for small datasets), n^2 is faster than n
- another example

```ts
// returns when a given char is found
function sum_char_codes(n: string): number {
    let sum = 0;
    for (let i = 0; i < n.length; ++i) {
        const charCode = n.charCodeAt(i);
        // Capital E
        if (charCode === 69) {
            return sum;
        }

        sum += charCode;
    }

    return sum;
}
```
- complexity still is o(n) => **focus on the worst case**
- plus, even if it's o(1/2n), consts are dropped so it's o(n)
- important points
  - growth in respect to input
  - constants are dropped
  - worst case scenario is usually the one to be discussed
- common cases:
  ![common big o cases](images/big-o-face.png)
  - o(1): constant
  - o(logn): log with base 2, it's linear
  - o(n): 
  - o(nlogn)
  - o(n^2)
  - o(2^n): not doable in regular computers, takes too long
  - o(n!): not doable in regular computers, takes too long
- more examples
- o(n^2) (two nested loops)

```ts
function sum_char_codes(n: string): number {
    let sum = 0;
    for (let i = 0; i < n.length; ++i) {
        for (let j = 0; j < n.length; ++j) {
            sum += charCode;
        }
    }

    return sum;
}
```

- o(n^3) (three nested loops)
```ts
function sum_char_codes(n: string): number {
    let sum = 0;
    for (let i = 0; i < n.length; ++i) {
        for (let j = 0; j < n.length; ++j) {
            for (let k = 0; k < n.length; ++k) {
                sum += charCode;
            }
        }
    }
    return sum;
}
```

- o(nlogn)
  - every time you go over n, you halve the amount of input you have to search for => eventually goes down to 0
- there are other ways to measure time space complexity


### Arrays Data Structure

- Arrays = contiguous memory space
- For the computer it's just a bunch of 0s and 1s and we need something to interpret it in a specific way
- Depending on how you interpret it, the positions might change
  - Example: you can create an array, use a 8bit view to set a value for its 1st space of the array and then use a 16bit array and set a value to its 2nd value and the position in the array will be much further
  - this happens because the array uses contiguous memory and 16bit view is larger than 8bit, so the 16bit's 2nd value will be further than 8bit's
- **insertions in arrays overwrite values**
- we can't grow an array so easily, they're mostly static
  - insertion: go into the memory address, add in the width of the type (usually in bytes) multiply by the offset and add it in
- **deletions** are a bit odd: it depends on the program (eg maybe you'll insert a `null` there)
- What's the big o of getting something from an array?
  - o(1): we don't need to walk the whole array, we now the offset of the type, we know the size of the array, so just do the math and get what you need
  - same for delete
- traditional arrays have no methods, some don't even have length (in c you have to inform how big the array will be)

## Search

### Linear Search & Kata Setup

- the rules for arrays here
  - pretend js has arrays (because the ds there actually isn't an array)
  - only has access to .length
- we'll start with linear search (the simplest), o(n) => as the input grows, so does the time/memory, it grows linearly

```ts
export default function linear_search(
    haystack: number[],
    needle: number,
): boolean {
    for (let i = 0; i < haystack.length; i++) {
        if (needle === haystack[i]) return true;
    }
    return false;
}
```

### Binary Search Algorithm

- important question: 'is the data ordered?'
- initial algo explanation:
  - walk a given length, check if number matches
  - if smaller, walk another length
  - if larger, go back to the start and do linear search
  - big o: o(n) => add the number of walks + linear search but since we drop constants we go back to o(n)
  - even though technically it's faster, we get the same big o as linear search
- improving it
  - always go for half the array, check if is element
  - if it is, you're done
  - if not, keep halving until you do find the array
  - complexity: o(logn)
  - the important bit here is that we are not scanning! scanning seems like not so great

### Implementing Binary Search

```ts
export default function bs_list(haystack: number[], needle: number): boolean {
    let lo = 0;
    let hi = haystack.length;
    do {
        const m = Math.floor(lo + (hi - lo) / 2);
        const value = haystack[m];
        if (value === needle) {
            return true;
        } else if (needle > value) {
            lo = m + 1;
        } else {
            hi = m;
        }
    } while (lo < hi);

    return false;
}
```

### Two Crystal Balls Problem

- Given two crystal balls that will break if dropped from high enough distance, determine the exact spot in which it will break in the most optimized way.
  - based on the previous tools we had, we could approach this two ways: (i) linearly walk the array until we find the ideal point (o(n)), or (ii) use something like a binary approach to find the first break point and then linearly walk until we get to the other break (so kinda o(n) too)
  - we could do it better by jumping sqrt(n) => note to self: return to the lecture to see why this is so

### Implementing two crystal balls

```ts
export default function two_crystal_balls(breaks: boolean[]): number {
    const jumpAmount = Math.floor(Math.sqrt(breaks.length)); // assume the array is long enough to have a sqrt

    // use first crystal ball to know where it breaks
    let i = jumpAmount;
    for (; i < breaks.length; i += jumpAmount) {
        if (breaks[i]) break;
    }

    // find the point where it breaks, jump back sqrt(n) and then forward sqrt(n)
    i = i - jumpAmount;

    for (let j = 0; j <= jumpAmount && i < breaks.length; ++j, ++i) {
        // condition after && is for if we walk outside the array
        //walk maximum a jump amount of steps
        if (breaks[i]) {
            return i;
        }
    }
    return -1;
}

```
## Sort

### Bubble sort

- Any position in a sorted array has this: Xi <= Xi +1
- Bubble sort start in the 0 position, if the next element is larger than the current one they swap
- After one iteration the largest item is at the array's last position so we don't go through the last item in the next iteration
- the formula for this is the same as gauss' sum of all items in a range: **((n + 1)*n) / 2**
- we can ignore the /2 since it's a const and we end up with a complexity of **n^2 * n**
- then, we can drop insignificant items too like n because n^2 is so much bigger (same would happen to n^2 if there was a n^3)
- so this one has a complexity of o(n^2)

```ts
export default function bubble_sort(arr: number[]): void {
    for (let i = 0; i < arr.length; i++) {
        for (let j = 0; j < arr.length - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                const temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}
```

### Linked List Data Structures

- arrays are sorta of weird data structures
  - deletion? not doable, you can zero or null something
  - insertion? not doable, you can write
  - its ungrowable because it has a fixed size
- linked lists are more flexible, a node based ds (not because of the runtime, because of how you wrap your head around the data)
- each node points to the next one, so getting a given value works different than in arrays
  - this is how it works for a singly linked list => points to one direction only
- doubly linked list has references for both next and previous elements
- cool thing about linked lists: deletion and insertion can be very fast because we don't rely on the number of nodes, we can just change the next/previous props in a couple of nodes (which are constant operations) where you want to insert/delete and you're done
- so **insertions in linked lists are o(1)** => input doesn't matter because of these constant operations (even though in practice it matters)
- *exercise*: write down the operations for deletion and insertion in linked lists
- complexity
  - for getting a value, we need to walk until the value we want
  - for head and tail we might have references so they might be constant
  - deletion from ends might be constant because of the references
  - deletion in the middle not so much, the transversal might be costly
  - insertions in the head/tail (prepend/append) can be fast because of the references
  - insertions in the middle might be costly because of the transversal
- Linked lists are useful when we need to do a bunch of operations at the head or tail of the structure (eg, we need to constantly remove the oldest item which might be the tail)
- linked lists are the most fundamental units of other data structures like graphs

### Queue

- is this an algo or a ds? it uses the ds and performs the algo on top of the ds (thinking of linked lists, insertion would be the algo which is performed on top of the linked list, which is the ds)
- a **queue** is a specific implementation of a linked list
- in a queue we add new items to the tail
  - add new item
  - change tail.next reference to new item
  - change tail reference to new item
- pops happen from the head
  - change head reference to head.next
  - point head.next to null
  - return head.value
- performance implications:
  - there's no transverse
  - pushing and popping are constant time operations 
- FIFO: first in first out
- constraints are used to limit operations and make things fast while doing so

```ts
type Node<T> = {
    value: T;
    next?: Node<T>;
};

export default class Queue<T> {
    public length: number;
    private head?: Node<T>;
    private tail?: Node<T>;

    constructor() {
        this.head = this.tail = undefined;
        this.length = 0;
    }

    enqueue(item: T): void {
        const node = { value: item } as Node<T>;
        this.length++;

        if (!this.tail) {
            // create item in empty array sort of
            this.tail = this.head = node;
            return;
        }

        this.tail.next = node;
        this.tail = node;
        // console.log(this.head);
    }

    dequeue(): T | undefined {
        if (this.length === 0 || !this.head) {
            return undefined;
        }

        this.length--;
        const head = this.head;
        this.head = this.head.next;

        head.next = undefined;

        if (!this.head) {
            this.tail = undefined;
        }

        return head.value;
    }
    peek(): T | undefined {
        return this?.head?.value;
    }
}
```

### Stack

- A stack is like a queue but backwards
![common big o cases](images/queue.png)
- you can only add and remove items from the queue's head
- to add something, you remove the pointer to the old head, point it to the new head and point the previous prop of the new head to the old head
- to remove something, save the old head, get the head to point to the previous (or next) element and then return the old head
- helpful mental model for recursion (the functions being called recursively use a stack)
- implementation:

```ts

type Node<T> = {
    value: T;
    prev?: Node<T>;
};
export default class Stack<T> {
    public length: number;
    private head?: Node<T>;

    constructor() {
        this.head = undefined;
        this.length = 0;
    }

    push(item: T): void {
        const node: Node<T> = { value: item };
        this.length++;
        if (!this.head) {
            this.head = node;
            return;
        }

        node.prev = this.head;
        this.head = node;
    }
    pop(): T | undefined {
        this.length = Math.max(this.length - 1, 0);
        if (this.length === 0) {
            const head = this.head;
            this.head = undefined;
            return head?.value;
        }
        const head = this.head as Node<T>;
        this.head = this.head?.prev;
        return head.value;
    }
    peek(): T | undefined {
        return this.head?.value;
    }
}

```

## Arrays

### Arrays vs Linked List

- arrays are nice to access values (n(1));
- you can't insert/remove items without writing the for loops to shift/unshift all the other items
- you have to allocate the space for the array upfront while linked lists only use the memory they use
- usability differs: to find a specific item you need to walk a linked list (o(n))

### ArrayList

- can we have array access with the ability to grow?
- yes, with an array list
- it is a ds that uses an array as its base and then we build on this with custom add/remove features
- the structure has a length and a capacity
- to get the value from a specific index `get(index)`:
  - check if index is within length, return the element from it, if not return `undefined`
- push(val): 
  - is len with capacity? if so, add an element at len and increase len by one
  - O(1)
  - if len === capacity, create new array, move current elements to it, add new element and add 1 to len
- pop
  - is len > 0? return element from len -1 index, otherwise return `undefined`
- enqueue
  - to add something to the front of the array list, you gotta move over all elements to create the room at the front for the new element
  - not super performant (O(n))
- dequeue
  - similar to enqueue, gotta move over all elements (O(n))
  - this is also true for middle of list deletions
- "which one is better? linked list or array list?" it depends on the situation
  - array lists are great for random access but not so great for remove from the front because you need to shift everything
- TODO: fill up array list implementation

### Ring Buffers

- array like storage
- instead of 0 as head and len as tail, can have index based head and tail
- to remove from the front, add one to the head (o(1))
- same to remove from the tail, subtract one from the tail (o(1))
- there's only relocation when the tial exceeds the head
TODO: review how it circles around using the module operator

### What's the [] in JS?

- unshift = adding to a list
- when load testing, it's cool to have values like `[10, 100, 1000, 10_000, 100_000]` so there's a nice variation in the quantities
- according to tests:
  - get: o(n)
  - push/pop: o(n)
  - unshift o(n)
- this suggests it's an array list
- TODO: return here and explain in more details why this is so (also discussed in the q&a video)

## Recursion

### Recursion

- Def: a function that calls itself until it hits a base case that does something
- The simplest example:

```ts
function foo(n: number): number {
    // base case
    if (n === 1) {
        return 1;
    }

// the function calling itself = recursing(??)
    return n + foo(n - 1)
}
```

- base case is super important: think about it and try to make it as simple and clear as possible, this really helps!
- some important bits about calling functions:
  - rA = return address
  - rV = return value
  - A = Argument
- Now if we were to call `foo(5)` we'd have something like this:
![common big o cases](images/recursion.png)
- In this call, we start with an unknown return address, a return value of 5 + ? (because we don't know what it is yet) and the argument is 5
- it then calls `foo(4)` which has the call `foo(5)` as the return address, 4 + ? as the return value and 4 as the argument
- this goes on and on until we reach the base case, which is when we figure out the first ? in the return value and then move up in the stack actually making the calculations and eventually delivering the sum (in this case) to the mysterious return address
- In general, **recursion can be broken down in 3 steps** (important to paving?)
    1. pre: something that happens before the recursion, which is the n+ (5 + ?, 4 + ? and so on)
    2. recurse: the actual recursion
    3. post: something else after recursing. In our case it was returning but it could be anything like logging something for example
- repeating for emphasis: 2 important steps:
  - solid base case
  - then recurse

### Path finding: base case

- maze solver: 
  - these are walls: #
  - there's a start `s` and an end `e`
  - at any given point you can move to one of the four directions (up, right, down, left) as long as there are no walls
  - we should also try avoiding places we've been before so we don't end up running in circles
  - base case(s):
    1. it's a wall
    2. off the map (gotta go back to the map)
    3. it's the end (my only friend)
    4. if we have seen it (been on the tile previously)
  - recursive cases: check all directions
  - "seems like the initial function never is the one you want to recurse lol"
  - exercise has a nice tip about using an array to describe directions

```ts
const dir = [
    [-1, 0],
    [1, 0],
    [0, -1],
    [0, 1],
];

function walk(
    maze: string[],
    wall: string,
    curr: Point,
    end: Point,
    seen: boolean[][],
    path: Point[],
): boolean {
    //1. Base Case
    //off the map
    if (
        curr.x < 0 ||
        curr.x >= maze[0].length ||
        curr.y < 0 ||
        curr.y >= maze.length
    ) {
        return false;
    }

    //on a wall
    if (maze[curr.y][curr.x] === wall) {
        return false;
    }

    //on the end
    if (curr.x === end.x && curr.y === end.y) {
        path.push(end);
        return true;
    }

    //seen
    if (seen[curr.y][curr.x]) {
        return false;
    }

    // 3 recurse
    // pre
    seen[curr.y][curr.x] = true;
    path.push(curr);

    // recurse
    for (let i = 0; i < dir.length; ++i) {
        const [x, y] = dir[i];
        if (
            walk(maze, wall, { x: curr.x + x, y: curr.y + y }, end, seen, path)
        ) {
            return true;
        }
    }
    // post
    path.pop();
    return false;
}

export default function solve(
    maze: string[],
    wall: string,
    start: Point,
    end: Point,
): Point[] {
    const seen: boolean[][] = [];
    const path: Point[] = [];

    for (let i = 0; i < maze.length; ++i) {
        seen.push(new Array(maze[i].length).fill(false));
    }

    walk(maze, wall, start, end, seen, path);

    return path;
}

```