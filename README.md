# Collatz Conjecture PWA

A small project made from the desire of practicing PWA development and teach about the Collatz Conjecture.

The calculator accepts any positive integer number and applies recursively the following operations until reaching the 4-2-1 cycle, or trivial cycle.

$3x+1, \forall x \rightarrow  x \div 2 \notin \mathbb{Z}^{+}$

$x/2, \forall x \rightarrow  x \div 2 \in \mathbb{Z}^{+}$

Once the number 1 is reached, the calculator stops printing new lines in the textarea.

The calculator runs the formula employing BigInt numbers, as a means to allow calculating results with numbers greater than $2^{68}$, which was the highest number evaluated according to Verisatium in their video about the Collatz Conjecture published in July 30th, 2021:

[![Watch the video](https://img.youtube.com/vi/094y1Z2wpJg/default.jpg)](https://youtu.be/094y1Z2wpJg)

In case there is a non-trivial cycle hidden among the largest positive integers, this PWA could help a curious user find it and employ it to disprove the conjecture. For most, this PWA might provide more empirical evidence on how the conjecture upholds without having a proper mathematical proof. The only warning I can give is to not get crazy with numbers too large, as they may take longer to process or crash the browser.

## Hat Tip in the Implementation of an Observation

During the development of this PWA, I did notice that the trivial cycle is always reached when $3x+1$ produces a pure multiple of 4 (such as 16, 64 or 256), so a condition to inform the user such number appeared was added by evaluating $\log_4 x \in \mathbb{N}$. Due to the limitation the Math class has in evaluating BigInt numbers, the [Basenumber.js library](https://alexsp3.github.io/Basenumber.js/) by Alexandro Palacios was employed, adding a try-catch block observing a threshold in which the logarithm base 4 could no longer produce integer numbers due to how large the input could become.

## Observations for People Interested in proving the Conjecture

As indicated above, whenever $3x+1$ produces a pure multiple of 4, the trivial cycle is reached. Out of curiosity, I subtracted one from every pure multiple of 4 I could test and found out that every one of them is preceded by a multiple of 3. So perhaps a demonstration could employ a $3x+1 \equiv 4^y \forall x, y \in \mathbb{Z}^{+}$ relation. This relation might also be the reason why other Collatz-like problems do show more than one cycle, as $3x+1$ uniquely uses the relation of some multiples of 3 and pure multiples of 4, while the others like $5x+1$ or $7x+1$ seem to lack this relation, unless x is a multiple of 3.

The following JS code snippet was used to test the relation up to $4^{100000}$ in a browser, admittedly at the time of publishing this PWA, I don't know if a mathematical proof already exists for it, but in case it exists, it may be useful for a proof of the conjecture:
```
function(x) {
    let precededBy3;
    for (var i = 1n; i <= BigInt(x); i++) {
        precededBy3 =  (4n**i - 1n) % 3n === 0n;
        if (!precededBy3) {
            console.log('4^'+i+' is not preceded by a multiple of 3');
            break;
        }
    }
    if (precededBy3) {
        console.log('Cycle completed, all pure multiples of 4 up to the power of ' + x + ' are preceded by a multiple of 3');
    }
}
```

Additionally, 16 is the pure multiple of 4 that appeared most often when testing the PWA, while higher ones often required inputting an odd number that would satisfy the relation (i.e. inputting "21" so $3x+1$ would produce 64, which in turn goes down to the trivial cycle), so another angle to consider in a proof might be to search for the probability of a fair number that can be expressed as $5*2^x \forall x \in \mathbb{Z}^{+}$, which invariably descends to 5, then ascends to 16 and reaches the trivial cycle.

## Special Thanks

Thanks to the Youtube channel [Veritasium](https://www.youtube.com/@veritasium) for making the video that brought this problem to my attention; to Professor Jeffrey C. Lagarias for participating in it and editing ["The Ultimate Challenge: The 3x + 1 Problem"](https://books.google.com/books/about/The_Ultimate_Challenge.html?id=hekJ7JDMEVkC) I borrowed twice before making this PWA; and to [Alexandro Palacios](https://github.com/AlexSp3) for authoring the library that enabled me to test my observation about pure multiples of 4 while employing BigInt numbers in the PWA.

## License

This Project is published under the MIT License