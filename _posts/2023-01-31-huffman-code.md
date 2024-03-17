# Huffman Tree

I got a homework of Huffman Tree for a Information Theory course in the uni, so wrote a script for that.

```python
import re
from pprint import pprint

class Tree:

    def __init__(self, prob, symbol, children=None):
        self.prob = prob
        self.symbol = symbol
        self.children = children

    def __str__(self):

        if self.children is None:
            return f"{self.symbol}({self.prob})"

        return "[" + ", ".join([str(child) for child in self.children]) + "]"

    def get_table(self, code=""):
        
        if self.children is None:
            return {self.symbol: code}

        return {**self.children[0].get_table(code + '0'), **self.children[1].get_table(code + '1')}

symbols = [Tree(float(m.group(2)), m.group(1)) for m in [re.search(r"(\w)\((\d*(?:\.\d*)*?)\)", entry) for entry in input().split(',')]]

while len(symbols) > 1:

    symbols.sort(key=lambda x: x.prob)
    a, b = symbols.pop(0), symbols.pop(0)
    symbols.append(Tree(a.prob + b.prob, f"[{a}, {b}]", (a, b)))

print(symbols[0])

pprint(symbols[0].get_table())

```

The input is shown below.

```
A(7.8),B(2),C(4),D(3.8),E(11),F(1.4),G(3),H(2.3),I(8.6),J(0.21),K(0.97),L(5.3),M(2.7),N(7.2),O(6.1),P(2.8),Q(0.19),R(7.3),S(8.7),T(6.7),U(3.3),V(1),W(0.91),X(0.27),Y(1.6),Z(0.44)
```

With the script above, the result below is acquired.

```
[[[S(8.7), [[[V(1.0), [Z(0.44), [X(0.27), [Q(0.19), J(0.21)]]]], H(2.3)], L(5.3)]], [E(11.0), [[M(2.7), P(2.8)], [G(3.0), [F(1.4), Y(1.6)]]]]], [[[O(6.1), T(6.7)], [[U(3.3), D(3.8)], N(7.2)]], [[R(7.3), A(7.8)], [[[[W(0.91), K(0.97)], B(2.0)], C(4.0)], I(8.6)]]]]
{'A': '1101',
 'B': '111001',
 'C': '11101',
 'D': '10101',
 'E': '010',
 'F': '011110',
 'G': '01110',
 'H': '00101',
 'I': '1111',
 'J': '001001111',
 'K': '1110001',
 'L': '0011',
 'M': '01100',
 'N': '1011',
 'O': '1000',
 'P': '01101',
 'Q': '001001110',
 'R': '1100',
 'S': '000',
 'T': '1001',
 'U': '10100',
 'V': '001000',
 'W': '1110000',
 'X': '00100110',
 'Y': '011111',
 'Z': '0010010'}
```

From the code above, **ASSESS** is **1101,000,000,010,000,000** (Total 19 bit) and **WHY** is **1110000,00101,011111** (Total 18 bit)

Therefore, the code **WHY** is a bit shorter, but it is not that different. For **WHY**, it is shorter to use uniform codes (26 alphabets letters -> 5 bit for one letter -> 15 bit).
