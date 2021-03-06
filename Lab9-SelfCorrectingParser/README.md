# Assignment 9

**Solution for error recovery**

The solution is not complete, as it cannot handle inserting missing tokens (only check for replacements and deletes tokens). Also, I have adopted a greedy strategy to quickly choose which token to replace the wrong token with, which is wrong in some cases (the correct way would be try all tokens and choose the one which has the longest match, but that creates an entirely too big recursion tree).

The best way would probably be to use the initial specification of parser generated by code, then fill in the blank entries manually by logic and inspection

My approach is a mixture of all. It tries to repair the input and parse, and if it can&#39;t, simply gives up and announces **error** else announces **Can be derived**.

**Error correcting actions taken**:

- Delete duplicated tokens (Except duplicate brackets) to handle &quot;; ;&quot;, &quot;+ +&quot;, errors

- Replace tokens where we know for certain what the next token would be: Example: Only **id** can follow **scan**

- Handled assignment and equality mismatch specifically (:= and =) as it&#39;s the most common, etc.

- Try all tokens recursively in case of token mismatch

Currently in the program Q2\_5.java, an erroneous version is given which will correct itself and parse the input (with information). Some other sample screenshots are also included in the ```Images``` directory.

For the correct input (as given in Q5, part 2 initially), please uncomment the top String variable **str**.

**Note:** For including deletion, the blank entries in table could simply be filled with {nonterminal} -\&gt; {epsilon}, but that was failing much more often and was closer to _panic mode_ error recovery and not _phrase level_, so I discarded that.

The grammar is given in **er1.txt**. The format is all terminals on line 1, then all non-terminals on line 2 followed by all productions. Epsilon is denoted by *@*.

**A demo:**

*prog int id ; ; scan if id := ic then id := ( ic + ic } int id ; = prog $*

```Error: Repeated token ; at position 5
Error: Missing token id at position 6
Corrected token set is: [prog, int, id, ;, scan, id, if, id, :=, ic, then, id, :=, (, ic, +, ic, }, int, id, ;, =, prog, $]
Error: Should use equality rather than assignment at token 9
Error correction was done: Replaced token 18 with )
Error correction was done: Replaced token 19 with ;
Error correction was done: Replaced token 21 with :=
Error correction was done: Replaced token 22 with ;
Error correction was done: Replaced token 23 with end
```

_Can be derived :)_