# PyLog
Pylog is Prolog-ish interpreter written in Python 3. It originated as a final project for my Programming Languages class, but I wanted to take it further because this stuff is neat.

## Grammar
```
<expr> ::= <integer>
            true
            false
            <identifier>

<fact> ::= relation(constant, ...).

<rule> ::= name(constant, ... ) :- name(constant, ...), ... .
```


## Using PyLog

You can start the repl by typing `python pylog.py`:
```
$ python pylog.py

Prolog-ish Interpreter
#quit to quit
pylog>
```

### Facts

Facts are the building blocks of PyLog. Defining a fact adds the fact to the environment. You define a fact by following it with a period:
```
pylog> happy(hannah).
Fact happy defined
```

The word inside the parentheses is a constant. Constants are always lowercase. Facts can be defined as relations between multiple constants:

```
pylog> loves(mary,sally).
Fact loves defined
```

However, once you define a fact/relation it must always be given the same number of constants:
```
pylog> loves(hannah).
Error: Fact loves takes 2 argument(s), 1 given.
```

### Query

You can check if a fact is in the environment by using a question mark instead of a period:

```
pylog> loves(mary,sally)?
yes
```

You can also use variables to check for all facts that match a certain condition. Variables always start with an uppercase letter. First, let's add a few more facts to the loves relation:

```
pylog> loves(john,sally).
pylog> loves(mary,john).
```

Now we can find all of the people who love Sally:
```
pylog> loves(X,sally)?
loves('mary', 'sally').
loves('john', 'sally').
```
Notice that facts are positional.

### Rules

Rules are the most useful part of Prolog/Datalog. You can write some pretty neat stuff with them; one of my favorite examples is the [sudoku solver](https://programmablelife.blogspot.com/2012/07/prolog-sudoku-solver-explained.html). To define a rule in PyLog, you use the `:-` operator:
```
pylog> notsad(X) :- happy(X).
Rule notsad defined
```
This rule simply says that someone being not sad is the same as them being happy. Now, we can check for facts that satisfy this rule's conditions:

```
pylog> notsad(hannah)?
yes
```

Currently, rule queries can't handle variables. That will change as soon as I get around to it.

Rules can take multiple facts, too:
```
pylog> foobar(X) :- foo(X), bar(X).
Rule foobar defined
```

Facts have to satisfy all of the rule's conditions in order to satisfy the rule:
```
pylog> bar(bob).
Fact bar defined
pylog> foo(bob).
Fact foo defined
pylog> foobar(bob)?
yes
pylog> bar(joe).
pylog> foobar(joe)?
no
```
### Env
The `#env` command prints out everything that is currently in the environment. Here we see how everything is stored:

```
pylog> #env
Relations:
{'loves': [['mary', 'sally'], ['bob', 'sally'], ['john', 'sally'], ['mary', 'john']], 'foo': [['bob']],
'bar': [['bob'], ['joe']], 'happy': [['hannah']]}
Rules:
{'foobar': {'body': [('foo', (['X'], {})), ('bar', (['X'], {}))], 'vars': ['X']}, 'notsad': {'body': 
[('happy', (['X'], {}))], 'vars': ['X']}}
```

### Quit
You can quit PyLog by typing `#quit`. See you later!


## Future Work
- Implement recursive rules
- Allow rules to handle intermediate variables. Ex: `grandparent(X,Z) :- parent(X,Y), parent(Y,Z).` Shouldn't be too hard.
- Implement and, or, and not in rules.
