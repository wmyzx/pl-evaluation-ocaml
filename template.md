# OCaml

## History of OCaml
OCaml or "Objective Categorical Abstract Machine" was created in 1996 by Jérôme Vouillon, Didier Rémy.  OCaml is influced by C, Caml and Standart ML. So, they took the best features of C and Caml for the language that they wants to create. They wants to create an object-orianted Caml. Also, other programming language creators were influenced by the OCaml, like F#, Rust, Scala. OCaml is a general-purpose, multi-paradigm programming language which extends the Caml dialect of ML with object-oriented features.

## Why was OCaml invented?
Type systems and type inference for object-oriented programming has been a hot area of research since the early 1990's. Didier Rémy, later joined by Jérôme Vouillon, designed an elegant and highly expressive type system for objects and classes. This design was integrated and implemented within Caml Special Light, leading to the Objective Caml language and implementation, first released in 1996 and renamed to OCaml in 2011.

## Why shall we use OCaml?
Objective Caml was the first language to combine the full power of object-oriented programming with ML-style static typing and type inference. It supports many advanced OO programming idioms (type-parametric classes, binary methods, mytype specialization) in a statically type-safe way, while these idioms cause unsoundness or require run-time type checks in other OO languages such as C++ and Java.

## How to setup OCaml in different platforms?
#### Windows

* First install [cygwin](https://cygwin.com/) and a few additionals
  packages: rsync, patch, diffutils, curl, make, unzip, git, m4, perl,
  and of course mingw64-i686-gcc-core and/or mingw64-x86_64-gcc-core.


* Then proceed inside a cygwin shell:

```bash
$ tar -xf 'opam32.tar.xz' # or tar -xf 'opam64.tar.xz'
$ bash opam32/install.sh  # --prefix /usr/foo, the default prefix is /usr/local
                          # maybe you have to add /usr/local/bin to your PATH
$ opam init default "https://github.com/fdopen/opam-repository-mingw.git#opam2" -c "ocaml-variants.4.07.1+mingw32" --disable-sandboxing
# or, if you prefer the 64-bit version - 'opam switch -a' will list other supported versions
$ opam init default "https://github.com/fdopen/opam-repository-mingw.git#opam2" -c "ocaml-variants.4.07.1+mingw64" --disable-sandboxing
$ eval $(ocaml-env cygwin)
```

#### Linux
Most Linux distributions allow OCaml and/or OPAM to be installed directly through the system package manager.

##### Ubuntu or Debian
```bash
apt install ocaml-nox # If you don't want X11 support.
apt install ocaml
```
##### Fedora
```bash
dnf install ocaml
dnf search ocaml   # List packages related to OCaml
```
#### macOS
On macOS OCaml and/or OPAM can be installed via the existing third-party package systems.


##### Homebrew 
```bash
brew install ocaml
brew install opam
```
##### MacPorts
```bash
port install ocaml
port install opam
```
#### FreeBSD
```bash
pkg install ocaml-nox11 # If you don't want X11 support
pkg install ocaml
pkg install ocaml-opam
```
#### OpenBSD
```bash
pkg_add ocaml
pkg_add opam
```
#### NetBSD
```bash
pkg_add ocaml
pkg_add opam
```
#### Browser
The following pages allow to directly try snippets of OCaml in your browser:

* [Try OCaml](https://try.ocamlpro.com/) by OCamlPRO.
* An [OCaml toplevel](http://ocsigen.org/js_of_ocaml/dev/manual/files/toplevel/index.html) compiled by the js_of_ocaml project.
* [IOCamlJS](https://andrewray.github.io/iocamljs/) has OCaml toplevels with interactive notebook functionality.

## OCaml Basic and Example codes
### Comments
OCaml comments are delimited by (* and *), like this:
```ocaml
(* This is a single-line comment. *)

(* This is a
 * multi-line
 * comment.
 *)
```
### Calling functions
OCaml, in common with other functional languages, writes and brackets function calls differently, and this is the cause of many mistakes. Here is the same function call in OCaml:
```ocaml
repeated "hello" 3  
```
### Defining a function
The OCaml syntax is pleasantly concise. Here's a function which takes two floating point numbers and calculates the average:
```ocaml
let average a b =
  (a +. b) /. 2.0;;
```
### Recursive functions
Unlike in C-derived languages, a function isn't recursive unless you explicitly say so by using let rec instead of just let. Here's an example of a recursive function:
```ocaml
# let rec range a b =
    if a > b then []
    else a :: range (a+1) b;;
```
### Recursive Factorial Function
```ocaml
# let rec fact x =
   	 if x <= 1 then 1 else x * fact (x - 1);;
```
### Power Function
```ocaml
# let rec power f n = 
    if n = 0 then fun x -> x 
    else compose f (power f (n - 1));;
```
### Insertion Sort
Insertion sort is defined using two recursive functions.
```ocaml
# let rec sort = function
    | [] -> []
    | x :: l -> insert x (sort l)
  and insert elem = function
    | [] -> [elem]
    | x :: l -> if elem < x then elem :: x :: l
                else x :: insert elem l;;
```
### Symbolic computation
Let us consider simple symbolic expressions made up of integers, variables, let bindings, and binary operators. Such expressions can be defined as a new data type, as follows:
```ocaml
# type expression =
    | Num of int
    | Var of string
    | Let of string * expression * expression
    | Binop of string * expression * expression;;
```
Evaluation of these expressions involves an environment that maps identifiers to values, represented as a list of pairs.
```ocaml
# let rec eval env = function
    | Num i -> i
    | Var x -> List.assoc x env
    | Let (x, e1, in_e2) ->
       let val_x = eval env e1 in
       eval ((x, val_x) :: env) in_e2
    | Binop (op, e1, e2) ->
       let v1 = eval env e1 in
       let v2 = eval env e2 in
       eval_op op v1 v2
  and eval_op op v1 v2 =
    match op with
    | "+" -> v1 + v2
    | "-" -> v1 - v2
    | "*" -> v1 * v2
    | "/" -> v1 / v2
    | _ -> failwith ("Unknown operator: " ^ op);;
```
