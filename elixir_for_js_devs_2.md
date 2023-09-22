---
marp: true
theme: gaia
---

<style>
  section {
    display: flex;
    flex-direction: column;
  }
  pre {
    font-size: 44px
  }
</style>

<style scoped>
  section {
    justify-content: center;
    align-items: center;
  }
</style>

# Elixir for JS Devs

Part 2

Language features

---

## Built in Documentation

Like Rust, Elixir has documentation baked right into the language.

Remember the `h` helper from IEx in our last video?
That documentation comes right from the source code. Let's see how to do it.

---

```elixir
defmodule Foo do
  @moduledoc """
  # The Foo module
  Module level documentation can be written as markdown
  inside a `@moduledoc` tag.
  """

  @doc """
  Function docs are written inside `@doc` tags above functions they document.
  """
  def bar() do
  end
end
```

---

It really is that simple.

Check-out the source code for your favorite Elixir modules and you'll see `@moduledoc`s and `@docs`.

The documenation itself is build with a tool called **ExDoc**. You can find the documentation for it on HexDocs: https://hexdocs.pm/ex_doc/readme.html

All of the documentation on HexDocs is built with ExDoc.

---

## Testing

Testing is also a first class language feature via a library called **ExUnit**. Run the following as a script to try it out.

```elixir
ExUnit.start()
defmodule ExampleTest do
  use ExUnit.Case, async: true

  test "the truth" do
    assert true
  end
end
```

---

## Package managment

Elixir's package manager is called **Hex** and it lives at https://hex.pm

The build tool is called **Mix**, and you define your packages in a `mix.exs` file, just as you would in a `package.json` file.

Mix will download and compile the dependcies automatically as part of the build process.

---

To start a project, run:

```bash
mix new my_project
```

This will create a basic structure for you, including the `mix.exs` file, tests, and the `lib` directory where your code lives.

---

Here is the structure of the basic Elixir project.

```
∴ tree my_project
my_project
├── README.md
├── lib
│   └── my_project.ex
├── mix.exs
└── test
    ├── my_project_test.exs
    └── test_helper.exs
```

---

## Macros

Macros are pieces of code that run at compile time. They can be used for all sorts of things, but are an advanced feature.

We will return to cover Macros later.

---

## Expressions

In JavaScript, the following code is invalid:

```js
let foo = if (true) {
  4
  } else {
    7
  }
}
```

`if` in JavaScript is a _statment_. It has no value.

---

#### In Elixir, everything has a value.

```elixir
foo = if true do
  4
else
  7
end
```

---

## Types

Elixir is a **strongly** but **dynamically** typed language.

Unlike JavaScript, which is _weakly_ typed, in Elixir types cannot be coerced into each other.

```js
// JavaScript:
1 + "1"; // "11"
```

```elixir
1 + "1"
** (ArithmeticError) bad argument in arithmetic expression: 1 + "1"
```

---

Here are the primitive types in Elixir:

```
- integer
- float
- atom
- reference
- function
- port
- pid
- tuple
- map
- list
- bitstring
```

---

_Integers_ are whole numbers. `-3`, `0`, and `42` are all Integers.

_Floats_ are numbers with a decimal point. `1.0`, `-2.4`. 

In Elixir, the triple equals `===` is used when you do not want a Float to equal an Intger

```elixir
1 == 1.0 # true
1 === 1.0 # false
```

Unlike JavaScript, `===` behaves exactly like `==` otherwise.

---

Atoms are symbols with a `:` preceding them. `:foo`, `:bar` `:"14"` are all atoms.

`true` and `false` (even without the colon) are also atoms. There is no boolean type in Elixir.

Likewise, `nil` is also an atom.

---

__You must never allow user input to become an an atom__, as there is a fixed number of allowed atoms, and once there are more than that, your program will crash.

Test the following to see for yourself.

```elixir
for i <- 1..3_000_000 do
  i |> to_string() |> String.to_atom()
end
```
There are compiler flags your can set to change the number of Atoms allowed, but nevertheless, you should __be very careful dynamically creating atoms__.

---

`reference`s are unique values in the VM. They are helpful for working with Concurrency. They will show up again later. You can create a reference with `make_ref/0`.

`function`s are... functions, you can assign them variables with 
`foo = fn -> :bar end`

`port`s provide a mechanism to start operating system processes external to the Erlang VM and communicate with them via message passing.

`pid`s are Process IDs. Elixir is a concurrent and parallel language, and Processes are a fundamental building block we wil return to later. You can get the current process with `self/0`.


---

## Syntatic Sugar

In the last video, we saw that the following code snippets were identical

```elixir
if true do
  1 + 1
end
```

```elixir
if true, do: 1 + 1
```

`do ... end` is actually just Syntatic sugar for `[do: ...]`

---
