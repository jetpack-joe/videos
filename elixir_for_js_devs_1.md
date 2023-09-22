---
marp: true
theme: gaia
---

<style>
  section {
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
</style>

<style scoped>
  section {
    align-items: center;
  }
</style>

# Elixir for JS Devs


<br/>

Syntax and Running Code


---

## Elixir?

> Elixir is a dynamic, functional language for building scalable and maintainable applications. 


Elixir is especially suited for the web development space because of how easy it is to write concurrent and parallel code.

Let's learn Elixir starting at a simple programming challenge.

---

## The Collatz conjecture

The Collatz conjecture states that for any positive integer:

- If it is even you divide by 2,
- If it is odd you multiply by 3 and add one
- Eventually this process will reach the number one.

Lets look at a program to calculate how many steps this would take using JavaScript and Elixir.

---

Starting with `15`, we get to `1` on the 17th step, so we should return `17`

```
| step   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   |
| number | 15  | 46  | 23  | 70  | 35  | 106 | 53  | 160 | 80  |
|--------|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| step   | 9   | 10  | 11  | 12  | 13  | 14  | 15  | 16  | 17  |
| number | 40  | 20  | 10  | 5   | 16  | 8   | 4   | 2   | 1   |
```

---

Writing functional JavaScript, you might get an answer that looks like this:

```js
function collatzSteps(n, steps = 0) {
  if (n === 1) return steps;
  if (n % 2 === 0) {
    return collatzSteps(n / 2, steps + 1);
  } else {
    return collatzSteps(n * 3 + 1, steps + 1);
  }
}
```

This solution uses _recursion_ to repeatedly call the `collatzSteps` function, incrementing the `steps` argument by one each time.

When `n === 1`, `steps` is returned.

---

Elixir does not have an early return, so it handles the recursion slightly differently:

```Elixir
defmodule Collatz do
  def steps(n, steps \\ 0)
  def steps(1, steps), do: steps
  def steps(n, steps) do
    if rem(n, 2) == 0 do
      steps(div(n, 2), steps + 1)
    else
      steps((n * 3) + 1, steps + 1)
    end
  end
end
```

---

<style scoped>
  section {
    align-items: center;
  }
</style>

#### Lets go through this line by line

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

```elixir
defmodule Collatz do
```

In Elixir, named functions have to belong to a module, so we create a `Collatz` module here using `defmodule`.

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

```elixir
def steps(n, steps \\ 0)
```

Because we have a function with multiple heads (there are 2 `def steps`), **and** because we are using a default argument (steps = 0), the Elixir compiler needs us to declare function and the defaults upfront.

<br/>

If we were not defaulting the `steps` argument to `0`, we would not need this line.

---

In Elixir, matching function heads are evaluated from top to bottom, a function head matches if its **name** and **artity** match.

Arity just means how many arguments a function takes.

In this case, all of our functions are called `steps` and they take two arguments, so you would say they have an **arity** of two.

This would be written as `steps/2` to denote this.

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

```elixir
def steps(1, steps), do: steps
```

Because this is the first head, it is checked first, this is the base case for our recursion.

If `n` is equal to `1`, then we can just return `steps`. `n` does not appear in this function, and instead we are pattern-matching against `1`.

We will return to look deeper into pattern-matching later.

This line corresponds to the following JavaScript:

```js
if (n === 1) return steps;
```

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

```elixir
def steps(n, steps) do
```

This is the second head of our function.

In Elixir, if a function can fit on one line, it is customary to write it as `def func(arg), do: <code>`

If will take more than one line (and most functions do), then you use the form

```elixir
def func(args) do
  <code>
end
```

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

```elixir
if rem(n, 2) == 0 do
```

`rem(n, 2)` in Elixir directly translates to `n % 2` in JavaScript.

Like functions, one-liner `if`s can be written as `if <check>, do: <code>`, but we are using the multiline version here.

We will take a closer look at `do: <code>` vs `do <code> end` at a later video.

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

```elixir
  steps(div(n, 2), steps + 1)
```

In Elixir, `/` always returns a float, i.e., `4 / 2` returns `2.0`.

Because we want to stick to integers only, we use `div/2`, which will always return an integer.

We increment the number of steps, and pass these values back into `steps` to continue the recursion, just like in the JS version.

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

```elixir
  else
    steps((n * 3) + 1, steps + 1)
  end
```

Likewise, we increment the steps, multiply `n` by 3 and add one, and continue the recursion.

Unlike JavaScript, curly braces `{ }` are not necessary.

Instead, Elixir looks for `end` to close off `if`s, `def`s and `defmodule`s.

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

```elixir
  end
end
```

Closes off our function and module.

---

<style scoped>
  pre {
    font-size: 24px
  }
</style>

###### Here are the two solutions next to each other

```js
function collatzSteps(n, steps = 0) {
  if (n === 1) return steps;
  if (n % 2 === 0) {
    return collatzSteps(n / 2, steps + 1);
  } else {
    return collatzSteps(n * 3 + 1, steps + 1);
  }
}
```

```Elixir
defmodule Collatz do
  def steps(n, steps \\ 0)
  def steps(1, steps), do: steps
  def steps(n, steps) do
    if rem(n, 2) == 0 do
      steps(div(n, 2), steps + 1)
    else
      steps((n * 3) + 1, steps + 1)
    end
  end
end
```

---

<style scoped>
  section {
    align-items: center;
  }
</style>

## A new problem

Lets change the problem to show off more of Elixir's features.

---

For our new problem, instead of counting the steps it takes to reach `1`, we collect all the numbers we find along the way, discarding the even ones, and then multiply them together.

Once we have our multiplied number, we need to `add` up the digits in the number to get the final answer.

For `n = 15`, ignoring even numbers, we get `15 23 35 53 5 1`.

Which multiplys to `15 * 23 * 35 * 53 * 5 * 1 = 3_199_875`
Adding these digits up: `3 + 1 + 9 + 9 + 8 + 7 + 5 = 42`

I will leave the JavaScript solution as an exercise to the viewer. Let's see how it looks in Elixir:

---

<style scoped>
  pre {
    font-size: 60px
  }
</style>

```elixir
defmodule Collatz do
  defguardp is_even(n) when rem(n, 2) == 0
  def collect(n, acc \\ [])
  def collect(1, acc), do: [1 | acc]
  def collect(n, acc) when is_even(n), do: collect(div(n,2), [n | acc])
  def collect(n, acc), do: collect(n * 3 + 1, [n | acc])

  def calculate(n) do
    n
    |> collect()
    |> Enum.reject(&is_even/1)
    |> Enum.reduce(1, fn el, acc -> el * acc end)
    |> to_string()
    |> String.split("", trim: true)
    |> Enum.map(&String.to_integer/1)
    |> Enum.reduce(&+/2)
  end
end
```

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

Lets look at the new bits:

```elixir
defguardp is_even(n) when rem(n, 2) == 0
```

This defines a **guard** that we can use in function heads. We can now use the `is_even/1` check in our function heads instead of using an `if`.

`defguardp` denotes a **private** guard, meaning it cannot be used outside of this module.

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

```elixir
def collect(n, acc \\ [])
def collect(1, acc), do: [1 | acc]
def collect(n, acc) when is_even(n), do: collect(div(n,2), [n | acc])
def collect(n, acc), do: collect(n * 3 + 1, [n | acc])
```

Instead of `steps`, we have a function called `collect`, since it collects the results of going throught the Collatz algorithm.

We use the `is_even/1` guard instead of `if` to check for even-ness.

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

```elixir
def collect(n, acc \\ [])
def collect(1, acc), do: [1 | acc]
def collect(n, acc) when is_even(n), do: collect(div(n,2), [n | acc])
def collect(n, acc), do: collect(n * 3 + 1, [n | acc])
```

Elixir uses linked-lists, instead of arrays, and it is much faster to prepend to linked-lists than append.

In our functions we prepend `n` to `acc` by using the _cons_ operator, `|`:
`[n | acc]`

---

```elixir
def calculate(n) do
  n
  |> collect()
  |> Enum.reject(&is_even/1)
  |> Enum.reduce(1, fn el, acc -> el * acc end)
  |> to_string()
  |> String.split("", trim: true)
  |> Enum.map(&String.to_integer/1)
  |> Enum.reduce(&+/2)
end
```

---

In `calculate`, we are introduced to the pipe operator, `|>`.

The pipe operator takes the value on the left, and enters it as the first arugment to the function on the right.

So the following are equivalent:

```elixir
foo(2, 3)
2 |> foo(3)
```

---

While this is a simplistic case, It makes reading a chain of functions much easier. Without the pipe operator, we would either have to capture intermediate values like

```elixir
def calculate(n) do
  collected = collect(n)
  rejected = Enum.reject(collected, &is_even/1)
  reduced = Enum.reduce(rejected, 1, fn el, acc -> el * acc end)
  stringed = to_string(reduced)
  splitted = String.split(stringed, "", trim: true)
  mapped = Enum.map(splitted, &String.to_integer/1)
  Enum.reduce(mapped, &+/2)
end
```

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

Or nest the function calls into something wholly unreadable:

```elixir
def calculate(n) do
  Enum.reduce(
    Enum.map(
      String.split(
        to_string(
          Enum.reduce(
            Enum.reject(
              collect(n), &is_even/1
            ),
          1, fn el, acc -> el * acc end
        )
      ), "", trim: true
    ), &String.to_integer/1
  ), &+/2
)
end
```

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

```elixir
def calculate(n) do
  n
  |> collect()
  |> Enum.reject(&is_even/1)
  |> Enum.reduce(1, fn el, acc -> el * acc end)
  |> to_string()
  |> String.split("", trim: true)
  |> Enum.map(&String.to_integer/1)
  |> Enum.reduce(&+/2)
end
```

The pipe operator maks it so much eacher to read.

Lets go over `calculate/1` line by line:

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

Its customary to start a pipeline chain with a bare value, `n` in this case a bare `n`, passed in as an argument to this function.

```elixir
  n
```

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

Call the collect function defined earlier

```elixir
  |> collect()
```

<br/>

This is like calling `collect(n)`.

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

```elixir
  |> Enum.reject(&is_even/1)
```

Reject even numbers from the collection.

`reject` is a function defined in the `Enum` module, so we call it with the dot syntax: `Enum.reject`

`&` is the short-hand syntax for passing a function as an argument.
Because `is_even` takes one argument, we add `/1` to the end of it.

`is_even/1` is the guard we defined earlier.

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

```elixir
  |> Enum.reduce(1, fn el, acc -> el * acc end)
```

`fn args -> <code> end` is the long-hand syntax for passing a function as an argument.

`Enum.reduce/3` is like JavaScript's reduce, though the arguments are in a different order.

In JavaScript, this looks like:

```js
arr.reduce((acc, el) => acc * el, 1);
```

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

```elixir
  |> to_string()
```

`to_string/1` is available globally.
We will go over why this is is the case in a later video.

In this case, since we have a number `3_199_875` that we want to split on characters, we convert it to a string first.

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

```elixir
  |> String.split("", trim: true)
```

`String.split/3` means we are calling the `split` function on the `String` module.

`trim: true` is an option to remove dangling empty strings `""`. Without it, we would get :

```elixir
["", "3", "1", "9", "9", "8", "7", "5", ""]
```

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

```elixir
  |> Enum.map(&String.to_integer/1)
```

Like JavaScript's map.

We are using the shorthand syntax for passing a function again.

In JavaScript, this might look like:

```js
arr.map(Number);
// or
arr.map(parseInt);
```

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

```elixir
  |> Enum.reduce(&+/2)
```

Even though `+` is an infix operator, we can use it like a regular function here.

`0` is the default accumulator for `Enum.reduce`, so we can omit it here.

---

<style scoped>
  pre {
    font-size: 44px
  }
</style>

```elixir
  end
end
```

---

<style scoped>
  pre {
    font-size: 60px
  }
</style>

Here's to full code once again:

```elixir
defmodule Collatz do
  defguardp is_even(n) when rem(n, 2) == 0
  def collect(n, acc \\ [])
  def collect(1, acc), do: [1 | acc]
  def collect(n, acc) when is_even(n), do: collect(div(n,2), [n | acc])
  def collect(n, acc), do: collect(n * 3 + 1, [n | acc])

  def calculate(n) do
    n
    |> collect()
    |> Enum.reject(&is_even/1)
    |> Enum.reduce(1, fn el, acc -> el * acc end)
    |> to_string()
    |> String.split("", trim: true)
    |> Enum.map(&String.to_integer/1)
    |> Enum.reduce(&+/2)
  end
end
```

---

Install Elixir

1. Follow the instructions on https://elixir-lang.org/install.html
2. Use a package manager such as `asdf` (https://asdf-vm.com)

I personally use asdf.

---

Copy the code above and paste it into a `collatz.exs`. Then run it with:

```bash
elixir collatz.exs
```

`.exs` denotes an Elixir script file.
`.ex` denotes a regular Elixir file.

When working in a project, `.exs` files are not compiled.

Normally these are used for tests.

---

### Interactive Elixir

From the terminal, with Elixir installed, run `iex` to start an interactive Elixir REPL.

```bash
∴ iex
Erlang/OTP 26 [erts-14.0.2] [source] [64-bit] [smp:10:10] [ds:10:10:10] [async-threads:1] [jit]

Interactive Elixir (1.15.5) - press Ctrl+C to exit (type h() ENTER for help)
iex(1)>
```

---

You can also paste the our `Collatz` module directly into IEx REPL, and call it with `Collatz.calculate(15)`.

By the way, the code and slides for this video are available on Github,
https://github.com/jetpack-joe/videos.

Link in the description.

---

<style scoped>
  section {
    display: flex;
    flex-direction: column;
    justify-content: start;
  }
</style>

#### IEx tips

1. `h` shows your documentatin

```bash
iex(1)> h Enum
```

---

2. Globally defined variables are defined in the `Kernel` module. You can access the documention for these just the same.

```bash
iex(1)> h to_string
```

For infix operators, you have to specify `Kernel`:

```bash
iex(2)> h Kernel.+/2
```

---

3. `v` gets you the previous result. `v/1` lets you pick which result.

```bash
iex(1)> 2 + 3 # notice the `1` inside the parentheses
2
iex(2)> v # prints the previous result
2
iex(3)> 3 + 3
6
iex(4)> v(2)
2
iex(5)> v(3)
6
```

---

4. You can get type information with `i`

```bash
iex(1)> i "a"
Term
  "a"
Data type
  BitString
Byte size
  1
Description
  ...
```

---

5. See other IEx helpers at https://hexdocs.pm/iex/IEx.Helpers.html

Hexdocs is where Elixir documentation lives.

Your Homework:
Check out the `Enum` and `String` modules which we used above at:

- https://hexdocs.pm/elixir/Enum.html
- https://hexdocs.pm/elixir/String.html
