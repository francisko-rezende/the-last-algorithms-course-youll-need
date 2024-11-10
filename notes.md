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
- ![common big o cases](images/big-o-face.png)
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