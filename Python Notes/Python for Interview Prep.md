# Python for Interview Prep

### Represent a Graph with a Dictionary
#### HackerRank Style
~~~
tree = dict((el,[]) for el in range(1,num_nodes+1))
for e in range(0,num_edges):
    raw_line = raw_input()
    v1 = int(raw_line.split()[0])
    v2 = int(raw_line.split()[1])
    tree[v1].append(v2)
    tree[v2].append(v1)
~~~

### Ordered Dict

This problem [here](https://www.hackerrank.com/challenges/word-order) is surprisingly tricky to solve with standard lists/dicts, but becomes trivial when implemented with OrderedDict.

~~~
import collections
# Enter your code here. Read input from STDIN. Print output to STDOUT
n = int(raw_input())

words = collections.OrderedDict()
for i in range(0,n):
    word = str(raw_input())
    if word not in words:
        words[word] = 1
    else:
        words[word] += 1
      
print len(words)
print ' '.join(str(words[word]) for word in words)
~~~