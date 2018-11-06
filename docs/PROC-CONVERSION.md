## Proc-Block Conversions

### Executing the Block

Ruby is capable of executing a block by taking a block in a method.

```ruby
def greeter
  yield
end

greeter { puts 'hello' }
```

We need to use `yield` inside the greeter method to execute the code in the block.

### Ruby Creating a Proc Object from Block 

Ruby can create a Proc object from a Block for us. We need to prefix the method argument with ampersand.

```ruby
def greeter(&block)
  block
end

greet = greeter { puts 'hello' }
greet.call
```

::: tip SELF DISCOVERY
Print the class name of the block before the block is returned. What is the output?
:::

We can also call the Proc inside the method.

```ruby
def greeter(&block)
  block.call
end

greeter  { puts 'hello' }
```

### Converting a Proc to a Block

If we prefix a Proc object with an ampersand, it becomes a block.

```ruby
def greeter(&block)
  Proc.new(&block)
end

greet = greeter { puts 'hello' }
greet.call
```

In line 1, the block is converted to a Proc. In line 2, the Proc constructor takes a Proc object and converts it to a block. Compare the above example to the following:

```ruby
greet = Proc.new { puts 'hello' }
greet.call
```

The line:

```ruby
Proc.new(&block)
```

is equivalent to :

```ruby
Proc.new { puts 'hello' }
```

In line 1 `greeter(&block)`, the `greeter` method is not taking an argument. Eventhough the syntax looks like an argument block is passed in because we are using the brackets ( ). As you can see when we call the greeter method, we are not passing an argument:

```ruby
greeter { puts 'hello' }
```

The greeter does not have an argument. Only the block is provided. Similarly, the Proc constructor is not taking an argument:

```ruby
Proc.new(&block)
```

The constructor is taking a block. 

```ruby
def greeter(&block)
  p block.class
  Proc.new(&block)
end
```

This will print `Proc`. When the & is prefixed to a Proc object, it gets converted to a block.

::: tip Block to Proc

Prefixing & to a block converts a block to a Proc. 

:::

::: tip Proc to Block

Prefixing & to a Proc object converts a Proc object to a block
:::

### Converting a Proc to a Block

A simple example illustrating how to convert a Proc object to a block by prefixing a Proc object with ampersand.

```ruby
def greeter
  yield
end

proc = Proc.new { puts 'hello' }
greeter(&proc)
```

Compare this with the first code example.

```ruby
def greeter
  yield
end

greeter { puts 'hello' }
```

::: tip BEHIND THE SCENES

Ruby converts 

greeter(&proc) 

to 

greeter { puts 'hello' }

:::
