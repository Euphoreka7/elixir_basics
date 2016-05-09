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
[ name: "Bob", city: "Dallas", likes: "Programming" ]
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
dave_greeter = greeter.("Bob")
dave_greeter.() #"Hello Bob"
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

#### |> - Pipe Operator
```
people = DB.find_customers
orders = Orders.for_customers(people)
tax    = sales_tax(orders, 2013)
filing = prepare_filing(tax)
```
the same as
```
filing = DB.find_customers
           |> Orders.for_customers
           |> sales_tax(2013)
           |> prepare_filing

```

`val |> f(a,b)` the same as `f(val,a,b)`

```
list
|> sales_tax(2013)
|> prepare_filing
```
the same as `prepare_filing(sales_tax(list, 2013))`

#### Modules
```
defmodule Mod do
  def func1 do
    IO.puts "in func1"
  end
  def func2 do
    func1
    IO.puts "in func2"
  end
end

Mod.func1
Mod.func2
```

```
defmodule Outer do
  defmodule Inner do
    def inner_func do
    end
  end
  def outer_func do
    Inner.inner_func
  end
end

Outer.outer_func
Outer.Inner.inner_func
```

```
defmodule Mix.Tasks.Doctest do
  def run do
  end
end

Mix.Tasks.Doctest.run
```

#### The import Directive
```
import Module [, only:|except: ]
import List, only: [flatten: 1, duplicate: 2]
```

```
defmodule Example do
  def func1 do
    List.flatten [1,[2,3],4]
  end
  def func2 do
    import List, only: [flatten: 1]
    flatten [5,[6,7],8]
  end
end
```

#### The alias Directive
```
defmodule Example do
  def func do
    alias Mix.Tasks.Doctest, as: Doctest
    doc = Doctest.setup
    doc.run(Doctest.defaults)
  end
end
```

#### The require Directive
The require directive ensures that the given module is loaded before your code tries to use any of the macros it defines.

#### Module Attributes
(ruby constants)

```
defmodule Example do
  @author "Bob"
  def get_author do
    @author
  end
end

Example.get_author
```

```
defmodule Example do
  @attr "one"
  def first, do: @attr

  @attr "two"
  def second, do: @attr
end
```

#### Module names
Module names are just atoms

```
is_atom IO # true
to_string IO # "Elixir.IO"
:"Elixir.IO" === IO # true
:"Elixir.IO".puts 123 # 123
```

#### Calling a Function in an Erlang Library
```
:io.format("The number is ~3.1f~n", [5.678]) # The number is 5.7
```

#### Heads and Tails
```
[head | tail] = [1, 2, 3]
head # 1
tail # [2, 3]
```

#### Using Head and Tail to Process a List
```
defmodule MyList do
  def len([]),             do: 0
  def len([_head | tail]), do: 1 + len(tail)
end
```

#### Using Head and Tail to Build a List
```
def square([]),              do: []
def square([ head | tail ]), do: [ head*head | square(tail) ]
```

#### Creating a Map Function
```
def map([], _func),             do: []
def map([ head | tail ], func), do: [ func.(head) | map(tail, func) ]

MyList.map [1,2,3,4], fn (n) -> n*n end
```

#### Keeping Track of Values During Recursion
```
defmodule MyList do
  def sum(list), do: _sum(list, 0)

  defp _sum([], total),              do: total
  defp _sum([ head | tail ], total), do: _sum(tail, head+total)
end
```

#### Generalizing Sum Function
```
defmodule MyList do
  def reduce([], value, _), do: value
  def reduce([head | tail], value, func) do
    reduce(tail, func.(head, value), func)
  end

  def mapsum([], _func), do: 0
  def mapsum([head | tail], func) do
    func.(head) + mapsum(tail, func)
  end
end

MyList.reduce([1,2,3,4,5], 0, &(&1 + &2)) # 15
MyList.mapsum [1, 2, 3], &(&1 * &1) # 14

#### Max value in list

defmodule MyList do
  def max([head | tail]) do
    _max(tail, head)
  end
  def max([]), do: nil

  defp _max([], value), do: value
  defp _max([head | tail], current_max) when head > current_max do
    _max(tail, head)
  end
  defp _max([head | tail], current_max) when head <= current_max do
    _max(tail, current_max)
  end
end
```

#### Lists of lists
```
defmodule WeatherHistory do
  def for_location([], target_loc), do: []
  def for_location([ head = [_, target_loc, _, _ ] | tail], target_loc) do
    [ head | for_location(tail, target_loc) ]
  end
  def for_location([ _ | tail], target_loc), do: for_location(tail, target_loc)
end

defmodule MyList do
  def span(from, from) do
    [from]
  end

  def span(from, to) do
    [from | span(from + 1, to)]
  end

end
```
#### Dictionaries
`Keyword` if more than one entry with the same key or ordering required. `Map` for patten matching. `HashDict` for more than hundred entries.

```
defmodule Sum do
  def values(dict) do
    dict |> Dict.values |> Enum.sum
  end
end

hd = [ one: 1, two: 2, three: 3 ] |> Enum.into HashDict.new
IO.puts Sum.values(hd) # => 6

map = %{ four: 4, five: 5, six: 6 }
IO.puts Sum.values(map) # => 15

kw_list = [name: "Bob", likes: "Programming", where: "Dallas"] # [name: "Bob", likes: "Programming", where: "Dallas"]
hashdict = Enum.into kw_list, HashDict.new # HashDict<[name: "Bob", where: "Dallas", likes: "Programming"]>
map = Enum.into kw_list, Map.new # %{likes: "Programming", name: "Bob", where: "Dallas"}

kw_list[:name] # "Bob"
hashdict[:likes] # "Programming"
map[:where] # "Dallas"

hashdict = Dict.drop(hashdict, [:where, :likes]) # #HashDict<[name: "Bob"]>
hashdict = Dict.put(hashdict, :also_likes, "Ruby") # #HashDict<[name: "Bob", also_likes: "Ruby"]>
combo = Dict.merge(map, hashdict) # %{also_likes: "Ruby", likes: "Programming", name: "Bob", where: "Dallas"}

kw_list = [name: "Bob", likes: "Programming", likes: "Elixir"] # [name: "Bob", likes: "Programming", likes: "Elixir"]
kw_list[:likes] # "Programming"
Dict.get(kw_list, :likes) # "Programming"
Keyword.get_values(kw_list, :likes) # ["Programming", "Elixir"]
```
#### Pattern Matching and Updating Maps
```
person = %{ name: "Bob", height: 1.88 } # %{height: 1.88, name: "Bob"}
%{ name: a_name } = person # %{height: 1.88, name: "Bob"}
a_name # "Bob"
%{ name: _, height: _ } = person # %{height: 1.88, name: "Bob"}
%{ name: "Bob" } = person # %{height: 1.88, name: "Bob"}

people = [
  %{ name: "Grumpy",    height: 1.24 },
  %{ name: "Bob",       height: 1.88 },
  %{ name: "Dopey",     height: 1.32 },
  %{ name: "Shaquille", height: 2.16 },
  %{ name: "Sneezy",    height: 1.28 }
]

for person = %{ height: height } <- people,
  height > 1.5,
  do: IO.inspect person

%{height: 1.88, name: "Bob"} %{height: 2.16, name: "Shaquille"}

defmodule HotelRoom do
  def book(%{name: name, height: height})
  when height > 1.9 do
    IO.puts "Need extra long bed for #{name}"
  end

  def book(%{name: name, height: height})
  when height < 1.3 do
    IO.puts "Need low shower controls for #{name}"
  end

  def book(person) do
    IO.puts "Need regular bed for #{person.name}"
  end
end

people |> Enum.each(&HotelRoom.book/1)
```
#### Updating a Map
```
m = %{ a: 1, b: 2, c: 3 } # %{a: 1, b: 2, c: 3}
m1 = %{ m | b: "two", c: "three" } # %{a: 1, b: "two", c: "three"}
m2 = %{ m1 | a: "one" } # %{a: "one", b: "two", c: "three"}
m3 = Dict.put_new(m, :z, 1) # %{a: 1, b: 2, c: 3, z: 1}
```
####  Maps and Structs
```
defmodule Subscriber do
  defstruct name: "", paid: false, over_18: true
end

s1 = %Subscriber{} # %Subscriber{name: "", over_18: true, paid: false}
s2 = %Subscriber{ name: "Bob" } # %Subscriber{name: "Bob", over_18: true, paid: false}
s3 = %Subscriber{ name: "Mary", paid: true } # %Subscriber{name: "Mary", over_18: true, paid: true}

s3.name # "Mary"
%Subscriber{name: a_name} = s3 # %Subscriber{name: "Mary", over_18: true, paid: true}
a_name # "Mary"
s4 = %Subscriber{ s3 | name: "Marie"} # %Subscriber{name: "Marie", over_18: true, paid: true}
```

```
defmodule Attendee do
  defstruct name: "", paid: false, over_18: true

  def may_attend_after_party(attendee = %Attendee{}) do
    attendee.paid && attendee.over_18
  end

  def print_vip_badge(%Attendee{name: name}) when name != "" do
    IO.puts "Very cheap badge for #{name}"
  end

  def print_vip_badge(%Attendee{}) do
    raise "missing name for badge"
  end
end

a1 = %Attendee{name: "Bob", over_18: true} %Attendee{name: "Bob", over_18: true, paid: false}
Attendee.may_attend_after_party(a1) # false
a2 = %Attendee{a1 | paid: true} # %Attendee{name: "Bob", over_18: true, paid: true}
Attendee.may_attend_after_party(a2) # true
Attendee.print_vip_badge(a2) # Very cheap badge for Bob
a3 = %Attendee{}
%Attendee{name: "", over_18: true, paid: false} iex> Attendee.print_vip_badge(a3) # (RuntimeError) missing name for badge
```
#### @derive to access fields using square brackets
```
defmodule Attendee do
  @derive Access
  defstruct name: "", over_18: false
end

a = %Attendee{name: "Sally", over_18: true} %Attendee{name: "Sally", over_18: true}
a[:name] # "Sally"
a[:over_18] # true
a.name # "Sally"
```
#### Nested Dictionary Structures
```
defmodule Customer do
  defstruct name: "", company: ""
end

defmodule BugReport do
  defstruct owner: %{}, details: "", severity: 1
end

report = %BugReport{owner: %Customer{name: "Bob", company: "Sweethome"}, details: "broken"}
report.owner.company # "Sweethome"

report = %BugReport{ report | owner: %Customer{ report.owner | company: "Cmp" }}
put_in(report.owner.company, "Cmp") # %BugReport{details: "broken", owner: %Customer{company: "Cmp", name: "Bob"}, severity: 1}

update_in(report.owner.name, &("Mr. " <> &1)) # %BugReport{details: "broken", owner: %Customer{company: "Cmp", name: "Mr. Bob"}, severity: 1}
```
#### Nested Accessors and Nonstructs
```
report = %{ owner: %{ name: "Bob", company: "Sweethome" }, severity: 1} # %{owner: %{company: "Sweethome", name: "Bob"}, severity: 1}
put_in(report[:owner][:company], "Cmp") # %{owner: %{company: "Cmp", name: "Bob"}, severity: 1}
update_in(report[:owner][:name], &("Mr. " <> &1)) # %{owner: %{company: "Sweethome", name: "Mr. Bob"}, severity: 1}
```
#### Dynamic (Runtime) Nested Accessors
```
nested = %{
    buttercup: %{
      actor: %{
        first: "Robin",
        last: "Wright"
      },
      role: "princess"
    },
    westley: %{
      actor: %{
        first: "Carey",
        last: "Ewes"
      },
    role: "music listener"
  }
}

IO.inspect get_in(nested, [:buttercup]) # %{actor: %{first: "Robin", last: "Wright"}, role: "princess"}
IO.inspect get_in(nested, [:buttercup, :actor]) # %{first: "Robin", last: "Wright"}
IO.inspect get_in(nested, [:buttercup, :actor, :first]) # "Robin"

IO.inspect put_in(nested, [:westley, :actor, :last], "Elwes")
```

```
authors = [
  %{ name: "José", language: "Elixir" },
  %{ name: "Matz", language: "Ruby" },
  %{ name: "Larry", language: "Perl" }
]


languages_with_an_r = fn (:get, collection, next_fn) ->
  for row <- collection do
    if String.contains?(row.language, "r") do
      next_fn.(row)
    end
  end
end

IO.inspect get_in(authors, [languages_with_an_r, :name]) # [ "José", nil, "Larry" ]
```
#### Sets
```
set1 = Enum.into 1..5, HashSet.new # #HashSet<[1, 2, 3, 4, 5]>
Set.member? set1, 3 # true

set2 = Enum.into 3..8, HashSet.new # #HashSet<[3, 4, 5, 6, 7, 8]>
Set.union set1, set2 # #HashSet<[7, 6, 4, 1, 8, 2, 3, 5]>
Set.difference set1, set2 # #HashSet<[1, 2]>
Set.difference set2, set1 # #HashSet<[6, 7, 8]>
Set.intersection set1, set2 # #HashSet<[3, 4, 5]>
```
#### Enum
Convert any collection into a list:
```
list = Enum.to_list 1..5 # [1, 2, 3, 4, 5]
```
Concatenate collections:
```
Enum.concat([1,2,3], [4,5,6]) # [1, 2, 3, 4, 5, 6]
Enum.concat [1,2,3], 'abc' # [1, 2, 3, 97, 98, 99]
```
Create collections whose elements are some function of the original:
```
Enum.map(list, &(&1 * 10)) # [10, 20, 30, 40, 50]
Enum.map(list, &String.duplicate("*", &1)) # ["*", "**", "***", "****", "*****"]
```
Select elements by position or criteria:
```
Enum.at(10..20, 3) # 13
Enum.at(10..20, 20) # nil
Enum.at(10..20, 20, :no_one_here) # :no_one_here
Enum.filter(list, &(&1 > 2)) # [3, 4, 5]
Enum.filter(list, &Integer.is_even/1) # [2, 4]
Enum.reject(list, &Integer.is_even/1) # [1, 3, 5]
```
Sort and compare elements:
```
Enum.sort ["there", "was", "a", "crooked", "man"] # ["a", "crooked", "man", "there", "was"]
Enum.sort ["there", "was", "a", "crooked", "man"], &(String.length(&1) <= String.length(&2)) # ["a", "man", "was", "there", "crooked"]
Enum.max ["there", "was", "a", "crooked", "man"] # "was"
Enum.max_by ["there", "was", "a", "crooked", "man"], &String.length/1 # "crooked"
```
Split a collection:
```
Enum.take(list, 3) # [1, 2, 3]
Enum.take_every list, 2 # [1, 3, 5]
Enum.take_while(list, &(&1 < 4)) # [1, 2, 3]
Enum.split(list, 3) # {[1, 2, 3], [4, 5]}
Enum.split_while(list, &(&1 < 4)) # {[1, 2, 3], [4, 5]}
```
Join a collection:
```
Enum.join(list) # "12345"
Enum.join(list, ", ") # "1, 2, 3, 4, 5"
```
Predicate operations:
```
Enum.all?(list, &(&1 < 4)) # false
Enum.any?(list, &(&1 < 4)) # true
Enum.member?(list, 4) # true
Enum.empty?(list) # false
```
Merge collections:
```
Enum.zip(list, [:a, :b, :c]) # [{1, :a}, {2, :b}, {3, :c}]
Enum.with_index(["once", "upon", "a", "time"]) # [{"once", 0}, {"upon", 1}, {"a", 2}, {"time", 3}]
```
Fold elements into a single value:
```
Enum.reduce(1..100, &(&1+&2)) # 5050

Enum.reduce(["now", "is", "the", "time"],fn word, longest ->
  if String.length(word) > String.length(longest) do
    word
  else
    longest
  end
end # "time"

Enum.reduce(["now", "is", "the", "time"], 0, fn word, longest ->
  if String.length(word) > longest do
    String.length(word)
  else
    longest
  end
end
```
Deal a hand of cards:
```
deck = for rank <- '23456789TJQKA', suit <- 'CDHS', do: [suit,rank]
['C2', 'D2', 'H2', 'S2', 'C3', 'D3', ... ]

deck |> shuffle |> take(13)
['DQ', 'S6', 'HJ', 'H4', 'C7', 'D6', 'SJ', 'S9', 'D7', 'HA', 'S4', 'C2', 'CT']

hands = deck |> shuffle |> chunk(13)
[['D8', 'CQ', 'H2', 'H3', 'HK', 'H9', 'DK', 'S9', 'CT', 'ST', 'SK', 'D2', 'HA'],
 ['C5', 'S3', 'CK', 'HQ', 'D3', 'D4', 'CA', 'C8', 'S6', 'DQ', 'H5', 'S2', 'C4'],
 ['C7', 'C6', 'C2', 'D6', 'D7', 'SA', 'SQ', 'H8', 'DT', 'C3', 'H7', 'DA', 'HT'],
 ['S5', 'S4', 'C9', 'S8', 'D5', 'H4', 'S7', 'SJ', 'HJ', 'D9', 'DJ', 'CJ', 'H6']]
```
#### Streams—Lazy Enumerables
```
s = Stream.map [1, 3, 5, 7], &(&1 + 1)
Enum.to_list s # [2, 4, 6, 8]

squares = Stream.map [1, 2, 3, 4], &(&1*&1)
plus_ones = Stream.map squares, &(&1+1)
odds = Stream.filter plus_ones, fn x -> rem(x,2) == 1 end
Enum.to_list odds # [5, 17]

[1,2,3,4]
|> Stream.map(&(&1*&1))
|> Stream.map(&(&1+1))
|> Stream.filter(fn x -> rem(x,2) == 1 end) |> Enum.to_list
```

```
IO.puts File.open!("/usr/share/dict/words")
        |> IO.stream(:line)
        |> Enum.max_by(&String.length/1)

```
Slower than prev example:
```
IO.puts File.stream!("/usr/share/dict/words") |> Enum.max_by(&String.length/1)
```
#### Infinite Streams
```
Stream.map(1..10_000_000, &(&1+1)) |> Enum.take(5) # [2, 3, 4, 5, 6]
```
**Stream.cycle**
```
Stream.cycle(~w{ green white }) |>
    Stream.zip(1..5) |>
    Enum.map(fn {class, value} ->
        ~s{<tr class="#{class}"><td>#{value}</td></tr>\n} end) |>
    IO.puts

<tr class="green"><td>1</td></tr>
<tr class="white"><td>2</td></tr>
<tr class="green"><td>3</td></tr>
<tr class="white"><td>4</td></tr>
<tr class="green"><td>5</td></tr>
```
**Stream.repeatedly**
```
Stream.repeatedly(fn -> true end) |> Enum.take(3) # [true, true, true]
Stream.repeatedly(&:random.uniform/0) |> Enum.take(3) # [0.7230402056221108, 0.94581636451987, 0.5014907142064751]
```
**Stream.iterate**
```
Stream.iterate(0, &(&1+1)) |> Enum.take(5) # [0, 1, 2, 3, 4]
Stream.iterate(2, &(&1*&1)) |> Enum.take(5) # [2, 4, 16, 256, 65536]
Stream.iterate([], &[&1]) |> Enum.take(5) # [[], [[]], [[[]]], [[[[]]]], [[[[[]]]]]]
```
**Stream.unfold**
```
fn state -> { stream_value, new_state } end
Stream.unfold({0,1}, fn {f1,f2} -> {f1, {f2, f1+f2}} end) |> Enum.take(15) # [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377]
```
**Stream.resource**
```
Stream.resource(fn -> File.open("sample") end,
                fn file ->
                  case IO.read(file, :line) do
                    line when is_binary(line) -> { [line], file }
                    _ -> {:halt, file}
                  end
                end,
                fn file -> File.close!(file) end)

```
```
defmodule Countdown do
  def sleep(seconds) do
    receive do
      after seconds*1000 -> nil
    end
  end

  def say(text) do
    spawn fn -> :os.cmd('say #{text}') end
  end

  def timer do
    Stream.resource(
      fn -> # the number of seconds to the start of the next minute
        {_h,_m,s} = :erlang.time
        60 - s - 1
      end,

      fn # wait for the next second, then return its countdown
        0 ->
          {:halt, 0}

        count ->
          sleep(1)
          { [inspect(count)], count - 1 }
      end,

      fn _ -> end # nothing to deallocate
    )
  end
end

counter = Countdown.timer
printer = counter |> Stream.each(&IO.puts/1)
speaker = printer |> Stream.each(&Countdown.say/1)
speaker |> Enum.take(5)
```
#### The Collectable Protocol
```
Enum.into 1..5, [] # [1, 2, 3, 4, 5]
Enum.into 1..5, [100, 101] # [100, 101, 1, 2, 3, 4, 5]
Enum.into IO.stream(:stdio, :line), IO.stream(:stdio, :line)
```
#### Comprehensions
```
for x <- [1, 2, 3, 4, 5], do: x * x # [1, 4, 9, 16, 25]
for x <- [1, 2, 3, 4, 5], x < 4, do: x * x # [1, 4, 9]
for x <- [1,2], y <- [5,6], do: x * y # [5, 6, 10, 12]

min_maxes = [{1,4}, {2,3}, {10, 15}]
for {min,max} <- min_maxes, n <- min..max, do: n # [1, 2, 3, 4, 2, 3, 10, 11, 12, 13, 14, 15]

first8 = [1,2,3,4,5,6,7,8]
for x <- first8, y <- first8, x >= y, rem(x*y, 10)==0, do: {x, y} # [{5, 2}, {5, 4}, {6, 5}, {8, 5}]

reports = [dallas: :hot, minneapolis: :cold, dc: :muggy, la: :smoggy]
for { city, weather } <- reports, do: { weather, city } # [hot: :dallas, cold: :minneapolis, muggy: :dc, smoggy: :la]

for << ch <- "hello" >>, do: ch # 'hello'
for << ch <- "hello" >>, do: <<ch>> # ["h", "e", "l", "l", "o"]

for << << b1::size(2), b2::size(3), b3::size(3) >> <- "hello" >>,
  do: "0#{b1}#{b2}#{b3}" # ["0150", "0145", "0154", "0154", "0157"]

name = "Bob"
for name <- ["cat", "dog"], do: String.upcase(name) # ["CAT", "DOG"]
name # "Bob"

for x <- ~w{ cat dog }, into: %{}, do: { x, String.upcase(x) } # %{"cat" => "CAT", "dog" => "DOG"}
for x <- ~w{ cat dog }, into: Map.new, do: { x, String.upcase(x) } # %{"cat" => "CAT", "dog" => "DOG"}
for x <- ~w{ cat dog }, into: %{"ant" => "ANT"}, do: { x, String.upcase(x) } # %{"ant" => "ANT", "cat" => "CAT", "dog" => "DOG"}

for x <- ~w{ cat dog }, into: IO.stream(:stdio,:line), do: "<<#{x}>>\n" <<cat>>
<<dog>>
```
#### Strings
```
IO.puts "start" IO.write "
  my
  string
"
IO.puts "end"
```
produces
```
start
  my
  string
end
```
```
IO.puts "start"
IO.write """
  my
  string
"""
IO.puts "end"
```
produces
```
start
my
string
end
```
#### Sigils
Delimiters:
```
<...>, {...}, [...], (...), |...|, /.../, "...", '...'
```
types:
```
~C A character list with no escaping or interpolation
~c A character list, escaped and interpolated just like a single-quoted string 
~R A regular expression with no escaping or interpolation
~r A regular expression, escaped and interpolated
~S A string with no escaping or interpolation
~s A string, escaped and interpolated just like a double-quoted string
~W A list of whitespace-delimited words, with no escaping or interpolation 
~w A list of whitespace-delimited words, with escaping and interpolation
```
examples:
```
~C[1\n2#{1+2}] # '1\\n2\#{1+2}'
~c"1\n2#{1+2}" # '1\n23'
~S[1\n2#{1+2}] # "1\\n2\#{1+2}"
~s/1\n2#{1+2}/ # "1\n23"
~W[the c#{'a'}t sat on the mat] # ["the", "c\#{'a'}t", "sat", "on", "the", "mat"]
~w[the c#{'a'}t sat on the mat] # ["the", "cat", "sat", "on", "the", "mat"]
```
optional type specifier: a(atoms), c(list), s(strings of characters)
```
~w[the c#{'a'}t sat on the mat]a # [:the, :cat, :sat, :on, :the, :mat]
~w[the c#{'a'}t sat on the mat]c # ['the', 'cat', 'sat', 'on', 'the', 'mat']
~w[the c#{'a'}t sat on the mat]s # ["the", "cat", "sat", "on", "the", "mat"]

~w"""
the
cat
sat
"""
["the", "cat", "sat"]

~r"""
hello
"""i
~r/hello\n/i
```
#### Single-Quoted Strings - Lists of Character Codes
```
str = 'wombat' # 'wombat'
is_list str # true
length str # 6
Enum.reverse str # 'tabmow'

str = 'wombat' # 'wombat'
:io.format "~w~n", [ str ] # [119,111,109,98,97,116]
List.to_tuple str # {119, 111, 109, 98, 97, 116}
str ++ [0] # [119, 111, 109, 98, 97, 116, 0]

'∂x/∂y' # [8706, 120, 47, 8706, 121]
'pole' ++ 'vault' # 'polevault'
'pole' -- 'vault' # 'poe'

List.zip [ 'abc', '123' ] # [{97, 49}, {98, 50}, {99, 51}]

[ head | tail ] = 'cat'
head # 99
tail # 'at'
[ head | tail ] # 'cat'
```

```
defmodule Parse do
  def number([ ?- | tail ]), do: _number_digits(tail, 0) * -1 
  def number([ ?+ | tail ]), do: _number_digits(tail, 0)
  def number(str), do: _number_digits(str, 0)
  
  defp _number_digits([], value), do: value 
  defp _number_digits([ digit | tail ], value) 
  when digit in '0123456789' do
    _number_digits(tail, value*10 + digit - ?0)
  end
  defp _number_digits([ non_digit | _ ], _) do
    raise "Invalid digit '#{[non_digit]}'"
  end
end

Parse.number('123') # 123
Parse.number('-123') # -123
Parse.number('+123') # 123
```
