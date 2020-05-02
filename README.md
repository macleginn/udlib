# UDLib.py: Well-typed dataclasses for Universal Dependencies

UDLib.py is a small library that contains convenient data types for analysing 
and manipulating Universal Dependencies trees. You may use it if the API
provided by (pyconll)[https://github.com/pyconll/pyconll] is too much. The
library exports three dataclasses (`UDNode`, `UDEdge`, and `UDTree`) and two
helper methods (`conllu2graph`, `conllu2trees`).

## Data types

Unlike `pyconll`, UDLib does not have separate notions of `tree` and `sentence`.
The sole high-level structure is the `UDTree` dataclass with the following
fields:

* `id_lines`
* `keys`
* `nodes`
* `graph`

`id_lines` contains the list of comment lines preceding the main record. `keys`
contains node id's in the original order. `nodes` is a dictionary of
`UDNode`'s indexed by their id's. `graph` is a dictionary of `UDEdge`'s indexed
by their vertices; each edge is recorded both as a 'down' edge and as an 'up'
edge. `UDTree` has the `__str__` methods that prints it as a CoNLL-U record.

The `UDNode` dataclass has all the usual UD fields and the `__str__` method that
makes it possible to print it out as a CoNLL-U line.

The `UDEdge` dataclass has three fields: `head`, `relation`, and
`directionality`. In the UD terminology, 'head' is the parent node. `UDEdge`'s
`head`, however, is simply the other end of the edge. The original UD edges have
`directionality` 'down' and the reverse edges added for convenience have
`directionality` 'up'. The relations are the UD ones; they are represented as
strings.

Graphs corresponding to enhanced relations are not supported at the
moment. You may create an alternative list of `UDEdge`'s based on the contents
of DEPS fields and pass it together with other tree components to a `UDTree`
constructor.

All dataclasses have type hints, so a reasonable IDE will be able to provide
autocompletions for `myNode`'s fields when `myNode = myTree.nodes['2']`.

## Helper methods

`conllu2graph` takes as input a CoNLL-U record as a string with possible
whitespace at the beginning and end. It returns a tuple of `id_lines, keys,
nodes, graph`, so when creating individual trees, use
`UDTree(*conllu2graph(block))`.

`conllu2trees` takes as input a string containing path to an CoNLL-U formatted
corpus. It expects the file to consist of CoNLL-Urecords separated by two
newline symbols with possible additional whitespace at the beginning and end. It
returns a list of `UDTree`'s.
