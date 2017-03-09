# Data

## Index
- [Int](#int)
- [Bool](#bool)

<a id = "int"></a>
## Int
Usage:
```
++ symbol
-- symbol
```

Definition:
```
>> ++ := x -> (
>|     if (int? x) {&x = x + 1} else _
>| )

>> -- := x -> (
>|     if (int? x) {&x = x - 1} else _
>| )
```

Example:
```
>> x = 1
>> ++ x  // => _
>> x  // => 2
>> -- x  // => _
>> x  // => 1
```

<a id = "bool"></a>
## Bool
Usage:
```
object == object

true
false

~ bool

object != object

bool && bool
bool || bool
```

Definition:
```
*system defined*
>> a == b := if (a == b) then {$:true} else {$:false}

>> true := x -> y -> x
>> false := x -> y -> y

>> bool? := val -> (
>|     (val != true && val != false) false true
>| )

>> ~ := bool -> (
>|     if (bool? bool) {y -> x -> y >- x >- bool} else _
>| )

>> not := bool -> (
>|     ~ bool
>| )

>> != := x -> y -> (
>|     ~ (x == y)
>| )

>> && := x -> y -> (
>|     if (bool? x && bool? y) {
>|         x y false
>|     } else _
>| )

>> || := x -> y -> (
>|     if (bool? x && bool? y) {
>|         x true y
>|     } else _
>| )
```

Example:
```
>> true 1 2  // => 1
>> false 1 2  // => 2

>> bool? true  // => true
>> bool? 1  // => false

>> ~ true  // => false

"a" == "a"  // => true
1 != 2  // => false

true && true  // => true
true && false  // => false
true || true  // => true
true || false  // => true
```