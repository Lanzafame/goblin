Goblin
======

`goblin` ~~is~~ will be the first stage in the eTeak pipeline: it takes a Go expression or file, converts its AST into JSON, and emits it to stdout for consumption at a later time. 

## Usage

`goblin --file [FILENAME]` dumps a given file.  
`goblin --expr EXPR` dumps an expression.  
`goblin --stmt STMT` dumps a statement—due to a quirk in the Go AST API, this statement will be surrounded by a dummy function.

## Format

Every node is a JSON object containing at least two guaranteed keys:

* `kind` (string): this corresponds to the data type of the given node. Expressions (`Prim` and `Expr`) are `"expression"`, statements (`Statement` and `Simp`) are `"statement"`, binary and unary expressions are `"unary"` and `"binary"` respectively.
* `type` (string): this corresponds to the data constructor associated with the node. Casts have kind `"expression""` and type `"cast"`. Floats have kind `"literal"` and type `"FLOAT"`. Pointer types have kind `"type"` and type `"pointer"`.

I apologize for the semantic overlap associated with the vagueness of the words "kind" and "type". Suggestions as to better nomenclature are welcomed.

## FAQ's

**Why not use the `ast.Visitor` interface instead of recursing manually into every node?** Because `Visitor` is inherently side-effectual: it declares no return type, so it is not possible to use it to express an algebra (which is all this program really is).

## TODO

* Emit all AST nodes.
* Ensure every node conforms to the above format.
* Pull in github.com/stretchr/testify for assertions and glog for logging.
* Reorganize the directory structure so that it works with `go get`.
* Add a Makefile so I don't tear my hair out.

## Known Bugs

* The built-in `make` and `new` functions can be shadowed. Since goblin expects `make` and `new` to take types as arguments, it will reject a shadowing as a syntax error. The chances of this happening in real code are pretty low, all things considered.