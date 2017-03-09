<a id = "oop"></a>
# OOP

<a id = "class"></a>
## Class
Usage:
```
class :sub extends super block
```

Definition:
```
>> extends := :extends

>> new := base -> (
>|     base.new
>| )

>> Object := {
>|     new = [_clone]
>|     clone = [_clone]
>| }

>> class := name -> _extends -> super -> block -> (
>|     if (_extends == :extends) {
>|         if (super == _) {&super = Object} else _ ,
>|         super >> block ,
>|         [$name = block]
>|     } else _
>| )
```

Example:
```
>> class :Super extends _ { x = 1, y = 2}
>> class :Sub extends Super {
>|     y = 3
>| }
>> Sub.x  // => 1
>> Sub.y  // => 3
>> Super.y  // => 2
>> ins = new Sub
>> ins.x = 2
>> ins.x  // => 2
>> Sub.x  // => 1
```