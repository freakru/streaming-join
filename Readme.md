# Streaming Join

The goal of the project is to benchmark techniques for querying streaming joins.
Suppose we have an event stream containing two message types `A` and `B`, such
that `A` contains foreign keys to `B`. We would like to respond to queries of
the form:
```
SELECT * FROM A
JOIN B ON A.fk_b == B.id;
```
given the current state of the stream. This is the most basic case. We can
imagine more complex joins involving arbitrary relationships, such as trees and
graphs. For instance, a tree of relationships like this:
 ```
     A
    / \
   B   C
  / \
 D   E
 ```
would need to support queries like this, for example:
```
SELECT * FROM A
JOIN B on A.fk_b == B.id
JOIN E on B.fk_e == E.id;
```
We could build out a set of nested queries supporting the entire tree. Further,
we could expose these to users as a REST tree:
```
/As/
/A/[a_id]
/A/[a_id]/Bs
/A/[a_id]/B/[b_id]
/A/[a_id]/B/[b_id]/Ds
/A/[a_id]/B/[b_id]/D/[d_id]
/A/[a_id]/B/[b_id]/Es
/A/[a_id]/B/[b_id]/E/[e_id]
/A/[a_id]/Cs
/A/[a_id]/C/[c_id]
```

