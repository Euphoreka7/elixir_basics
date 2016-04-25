# Elixir Basics

## Data types

#### Integers
`1234`, `0xcafe`, `0o765`, `0b1010`, `1_000_000`

#### Atoms
`:fred` `:is_binary?` `:var@2` `:<>` `:===` `:"func/3"` `:"long john silver"`

#### Floating-Point Numbers
`1.0`   `0.2455`   `0.314159e1` `314159.0e-5`

#### Regular Expressions
`~r{regexp}` or `~r{regexp}opts`

```
Regex.run ~r{[aeiou]}, "caterpillar" # ["a"]
Regex.scan ~r{[aeiou]}, "caterpillar" # [["a"], ["e"], ["i"], ["a"]]
Regex.split ~r{[aeiou]}, "caterpillar" # ["c", "t", "rp", "ll", "r"]
Regex.replace ~r{[aeiou]}, "caterpillar", "*" # "c*t*rp*ll*r"
```

#### Tuples
A tuple is an ordered collection of values. Once created a tuple cannot be modified.
```
{ 1, 2 } 
{ :ok, 42, "next" } 
{ :error, :enoent }
{ status, file } = File.open("Rakefile") # {:ok, #PID<0.39.0>}
```

#### Lists
```
[1,2,3]++[4,5,6] # [1, 2, 3, 4, 5, 6]
[1, 2, 3, 4] -- [2, 4] # [1, 3]
1 in [1,2,3,4] # true
"wombat" in [1, 2, 3, 4] # false
```

#### Keyword Lists
```
[ name: "Dave", city: "Dallas", likes: "Programming" ]
```

#### Maps
```
states = %{ "AL" => "Alabama", "WI" => "Wisconsin" } 
responses = %{ { :error, :enoent } => :fatal, { :error, :busy } => :retry } 
colors = %{ :red => 0xff0000, :green => 0x00ff00, :blue => 0x0000ff } 
```

#### Binaries
```
bin = << 1, 2 >> # <<1, 2>>
byte_size bin # 2
```

```
bin = <<3 :: size(2), 5 :: size(4), 1 :: size(2)>> # <<213>>
:io.format("~-8.2b~n", :binary.bin_to_list(bin)) # 11010101
byte_size bin # 1
```

## Operators

#### Comparison operators
```
a === b # strict equality (so 1 === 1.0 is false) 
a !== b # strict inequality (so 1 !== 1.0 is true) 
a == b  # value equality (so 1 == 1.0 is true) 
a != b  # value inequality (so 1 != 1.0 is false) 
a > b   # normal comparison
a >= b  # : 
a < b   # : 
a <= b  # :
```
#### Boolean operators
Expect true or false as first argument.
```
a or b   # true if a is true, otherwise b
a and b  # false if a is false, otherwise b 
not a    # false if a is true, true otherwise
```

#### Relaxed Boolean operators
Expect arguments of any type.
```
a || b  # a if a is truthy, otherwise b
a && b  # b if a is truthy, otherwise a
!a      # false if a is truthy, otherwise true
```

#### Arithmetic operators
```
+ - * / div rem
```

#### Join operators
```
binary1 <> binary2 # concatenates two binaries
list1   ++ list2   # concatenates two lists
list1   -- list2   # returns elements in list1 not in list2
```

#### The in operator
`a in enum` # tests if a is included in enum

## Anonymous Functions
```
sum = fn (a, b) -> a + b end
f1 = fn a, b -> a * b end
greet = fn -> IO.puts "Hi there" end
sum.(1, 2) # 3
```

```
handle_open = fn
  {:ok, file} -> "Read data: #{IO.read(file, :line)}"
  {_, error} -> "Error: #{:file.format_error(error)}"
end
handle_open.(File.open("files/file1.exs"))
```





