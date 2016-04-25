# elixir_basics

## Data types

#### Integers
Integer literals can be written as decimal (1234), hexadecimal (0xcafe), octal (0o765), and binary (0b1010).
1_000_000

#### Atoms
:fred :is_binary? :var@2 :<> :=== :"func/3" :"long john silver"

#### Floating-Point Numbers
1.0   0.2456   0.314159e1 314159.0e-5

#### Regular Expressions
~r{regexp} or ~r{regexp}opts

iex> Regex.run ~r{[aeiou]}, "caterpillar" ["a"]
iex> Regex.scan ~r{[aeiou]}, "caterpillar" [["a"], ["e"], ["i"], ["a"]]
iex> Regex.split ~r{[aeiou]}, "caterpillar"
["c", "t", "rp", "ll", "r"]
iex> Regex.replace ~r{[aeiou]}, "caterpillar", "*" "c*t*rp*ll*r"

#### Tuples
A tuple is an ordered collection of values. As with all Elixir data structures, once created a tuple cannot be modified.
{ 1, 2 } 
{ :ok, 42, "next" } 
{ :error, :enoent }
iex> {status, file} = File.open("Rakefile") {:ok, #PID<0.39.0>}

#### Lists
iex> [1,2,3]++[4,5,6]
[1, 2, 3, 4, 5, 6]
iex> [1, 2, 3, 4] -- [2, 4]
[1, 3]
iex> 1 in [1,2,3,4]
true
iex> "wombat" in [1, 2, 3, 4] 
false

#### Keyword Lists
[ name: "Dave", city: "Dallas", likes: "Programming" ]

#### Maps

iex> states = %{ "AL" => "Alabama", "WI" => "Wisconsin" } 
%{"AL" => "Alabama", "WI" => "Wisconsin"}
iex> responses = %{ { :error, :enoent } => :fatal, { :error, :busy } => :retry } 
%{{:error, :busy} => :retry, {:error, :enoent} => :fatal}
iex> colors = %{ :red => 0xff0000, :green => 0x00ff00, :blue => 0x0000ff } 
%{blue: 255, green: 65280, red: 16711680}

