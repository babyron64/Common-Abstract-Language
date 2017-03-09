<a id = "list"></a>
# List

## Index
- [Define](#define)
- [Add](#add)
- [Delete](#delete)
- [Each](#each)
- [Empty?](#empty?)
- [Head](#head)
- [Tail](#tail)
- [Init](#init)
- [Last](#last)
- [First](#first)
- [Any?](#any?)
- [All?](#all?)

<a id = "define"></a>
## 1. Define
Usage:
```
list `(list)
```

Definition:
```
>> List := {
>|     base := [
>|         $:add = List.add,
>|         ...
>|     ],
>|     add := ~,
>|     ...
>| }

>> list := _list -> (
>|     list = `() ,
>|     if (list? _list) {&list = _list}
>|     else {&list = `(_list)} ,
>|     {@-list = list, List.base}
>| )
```

Example:
```
>> x = list `(1, 2, 3)
```

<a id = "length"></a>
## 2. Length
Usage:
```
list.len
```

Definition:
```
>> List.len := (
>|     list._len
>| )
```

Example:
```
>> x = list `(1, 2, 3)
>> x.len  // => 3
```

<a id = "add"></a>
## 3. Add
Usage:
```
list.add element
```

Definition:
```
>> List.add := ele -> (
>|     list = list + ele
>| )
```

Example:
```
>> x = list `(1, 2, 3)
>> x.add 4
>> x  // => `(1, 2, 3, 4)
```

<a id = "delete"></a>
## 4. Delete
Usage:
```
list.del
```

Definition:
```
>> List.del := (
>|     list = list - (list.len - 1)
>| )
```

Example:
```
>> x = list `(1, 2, 3)
>> x.del
>> x  // => `(1, 2)
```

<a id = "each"></a>
## 5. Each
Usage:
```
list.each \obj -> block
```

Definition:
```
>> List.each := func -> (
>|     @#_limit = list.len ,
>|     @#_counter = 0 ,
>|     while (_counter < _limit) {
>|         func list.$_counter
>|     }
>| )
```

Example:
```
>> x = list `(1, 2, 3, 4)
>> y = 0
>> x.each obj -> (
>|     &y = y + obj
>| )
>> y  // => 10
```

<a id = "empty?"></a>
## 5. Empty?
Usage:
```
list.empty?
```

Definition:
```
>> List.empty? := (
>|     list.len == 0
>| )
```

Example:
```
>> x = list `(1, 2, 3)
>> x.empty?  // => true
>> y = list `()
>> y.empty?  // => false
```

<a id = "head"></a>
## 6. Head
Usage:
```
list.head
```

Definition:
```
>> List.head := (
>|     list.0
>| )
```

<a id = "tail"></a>
## 7. Tail
Usage:
```
list.tail
```

Definition:
```
>> List.tail := (
>|     list - 0
>| )
```

<a id = "init"></a>
## 8. Init
Usage:
```
list.init
```

Definition:
```
>> List.init := (
>|     init - (list.len - 1)
>| )
```

<a id = "last"></a>
## 9. Last
Usage:
```
list.last
```

Definition:
```
>> List.last := (
>|     list.$(list.len - 1)
>| )
```

<a id = "first"></a>
## 10. First
Usage:
```
list.first
```

Definition:
```
>> List.first := cond -> (
>|     @#_counter = 0 ,
>|     @#_limit = list.len ,
>|     @#_not-found? = true ,
>|     while (cond && _not-found? && x < _limit) {
>|         if (cond list.$_counter) {_not-found? = false, list.$_counter}
>|         else {&_counter = _counter + 1}
>|     } ,
>|     list.$_counter
>| )
```

Example:
```
>> x = list `(list "a", list "bb", list "ccc", list "aa")
>> x.first (obj -> (obj.len == 2))  // => "bb"
```

<a id = "any?"></a>
## 11. Any?
Usage:
```
list.any?
```

Definition:
```
>> List.any? := cond -> (
>|     @#_counter = 0 ,
>|     @#_limit = list.len ,
>|     @#_not-found? = true ,
>|     while (cond && _not-found? && x < _limit) {
>|         if (cond list.$_counter) {_not-found? = false, list.$_counter}
>|         else {&_counter = _counter + 1}
>|     } ,
>|     not _not-found?
>| )
```

Example:
```
>> x = list `(list "a", list "bb", list "ccc", list "aa")
>> x.any? (obj -> (obj.len == 2))  // => true
```

<a id = "all?"></a>
## 12. All?
Usage:
```
list.all?
```

Definition:
```
>> List.all? := cond -> (
>|     @#_counter = 0 ,
>|     @#_limit = list.len ,
>|     @#_all_true? = true ,
>|     while (cond && _all_true? && x < _limit) {
>|         if (cond list.$_counter) {&_counter = _counter + 1}
>|         else {_all_true = false}
>|     } ,
>|     all_true?
>| )
```

Example:
```
>> x = list `(list "a", list "bb", list "ccc", list "aa")
>> x.all? (obj -> (obj.len == 2))  // => false
```