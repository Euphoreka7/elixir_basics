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

#### Functions Can Return Functions
```
fun1 = fn -> fn -> "Hello" end end
fun1 = fn -> (fn -> "Hello" end) end
fun1.()
other = fun1.()
fun1.().() # "Hello"
other.() # "Hello"
```

```
fun1 = fn ->
          fn ->
             "Hello"
          end
       end
```

```
greeter = fn name -> (fn -> "Hello #{name}" end) end
dave_greeter = greeter.("Dave")
dave_greeter.() #"Hello Dave"
```
#### Parameterized Functions
```
add_n = fn n -> (fn other -> n + other end) end
add_two = add_n.(2)
add_five = add_n.(5)
add_two.(3) # 5
add_five.(7) # 12
```

#### Passing Functions As Arguments
```
times_2 = fn n -> n * 2 end
apply = fn (fun, value) -> fun.(value) end
apply.(times_2, 6) # 12

list = [1, 3, 5, 7, 9]
Enum.map list, fn elem -> elem * 2 end    # [2, 6, 10, 14, 18]
```

#### The & Notation
```
add_one = &(&1 + 1) # same as add_one = fn (n) -> n + 1 end
add_one.(44) # 45

square = &(&1 * &1)
square.(8) # 64

speak = &(IO.puts(&1))
speak.("Hello") # Hello

divrem = &{ div(&1,&2), rem(&1,&2) }
divrem.(13, 5) # {2, 3}

l = &length/1 # &:erlang.length/1
l.([1,3,5,7]) # 4

len = &Enum.count/1 # &Enum.count/1
len.([1,2,3,4]) # 4
```

## Modules and Named Functions
```
defmodule Times do
  def double(n) do
    n*2
  end
end

Times.double 4 # 8

def double(n), do: n * 2

def greet(greeting, name), do: (
  IO.puts greeting
  IO.puts "How're you doing, #{name}?"
)
```
#### Function Calls and Pattern Matching
```
defmodule Factorial do
  def of(0), do: 1
  def of(n), do: n * of(n-1)
end

```

#### Guard Clauses
```
defmodule Guard do
  def what_is(x) when is_number(x) do
    IO.puts "#{x} is a number"
  end
  def what_is(x) when is_list(x) do
    IO.puts "#{inspect(x)} is a list"
  end
  def what_is(x) when is_atom(x) do
    IO.puts "#{x} is an atom"
  end
end

defmodule Factorial do
  def of(0), do: 1
  def of(n) when n > 0 do
    n * of(n-1)
  end
end
```

#### Default Parameters
```
defmodule Example do
  def func(p1, p2 \\ 2, p3 \\ 3, p4) do
    IO.inspect [p1, p2, p3, p4]
  end
end

Example.func("a", "b")           # => ["a",2,3,"b"]
Example.func("a", "b", "c")      # => ["a","b",3,"c"]
Example.func("a", "b", "c", "d") # => ["a","b","c","d"]

defmodule Params do
  def func(p1, p2 \\ 123)
  def func(p1, p2) when is_list(p1) do
    "You said #{p2} with a list"
  end
  def func(p1, p2) do
    "You passed in #{p1} and #{p2}"
  end
end

IO.puts Params.func(99)          # You passed in 99 and 123
IO.puts Params.func(99, "cat")   # You passed in 99 and cat
IO.puts Params.func([99])        # You said 123 with a list
IO.puts Params.func([99], "dog") # You said dog with a list

defmodule Chop do
  def guess(result, range) do
    guess(result, range, div(range.last + range.first, 2))
  end

  def guess(result, range, try) when result == try do
    result
  end

  def guess(result, range, try) when result > try do
    IO.puts "Is it #{try}"
    guess(result, try..range.last, div(try + range.last, 2))
  end

  def guess(result, range, try) when result < try do
    IO.puts "Is it #{try}"
    guess(result, range.first..try, div(range.first + try, 2))
  end
end
```

#### Private Functions
```
defp fun(a), do: false
```
