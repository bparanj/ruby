## Proc-Block Conversions

### Executing a Block

Methods in Ruby are capable of taking a block. We can execute the code in the block by using the `yield` keyword.

```ruby
def greeter
  yield
end

greeter { puts 'hello' }
```

We need to use `yield` inside the greeter method to execute the code in the block.

### Ruby Creating a Proc Object from Block 

Ruby can create a Proc object from a block for us. For this to happen, we need to prefix the block with an ampersand. You can see the syntax in line 1:

```ruby
def greeter(&block)
  block
end

greet = greeter { puts 'hello' }
greet.call
```

This is another way to execute the code in the block. In this case, we converted a block to a Proc object and invoked the call method on the Proc object returned by the greeter method.

::: tip SELF DISCOVERY
Print the class name of the block before the block is returned. What is the output?
:::

Instead of returning the Proc object. We can also call the Proc inside the method.

```ruby
def greeter(&block)
  block.call
end

greeter  { puts 'hello' }
```

The `block.call` is an alternative to using `yield` to execute a block. 

::: tip SELF DISCOVERY

What happens when you run the code without providing a block?
:::

What error do you get, if you run:

```ruby
def greeter(&block)
  block.call
end

greeter  
```

and

```ruby
def greeter
  yield
end

greeter
```

::: warning MISSING BLOCK

In both cases, if you don't provide a block, you will get an error. 
:::

We can use `block_given?` to prevent the program from crashing.

```ruby
def greeter
  yield if block_given?
end
```

```ruby
def greeter
  block.call if block_given?
end
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

The point is this line:

```ruby
Proc.new(&block)
```

is equivalent to :

```ruby
Proc.new { puts 'hello' }
```

In line 1 `greeter(&block)`, the `greeter` method is not taking an argument. Eventhough the syntax looks like an argument block is passed in because we are using the brackets ( ) in the greeter method definition. As you can see when we call the greeter method, we are not passing an argument:

```ruby
greeter { puts 'hello' }
```

The greeter does not have an argument. Only the block is provided. The code can be re-written:

```ruby
greeter() { puts 'hello' }
```

Ruby allows us not to use the empty paranthesis if there are no arguments. We can ask Ruby to print the number of arguments of the greeter method.

```ruby
p self.method(:greeter).arity
```

This prints 0. 

Similarly, the Proc constructor is not taking an argument:

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

This will print `Proc`. When the `&` is prefixed to a Proc object, it gets converted to a block.

::: tip Block to Proc

Prefixing & to a block converts a block to a Proc. 

:::

::: tip Proc to Block

Prefixing & to a Proc object converts a Proc object to a block
:::

### Converting a Proc to a Block

A simple example to illustrate how to convert a Proc object to a block by prefixing a Proc object with ampersand.

```ruby
def greeter
  yield
end

proc = Proc.new { puts 'hello' }
greeter(&proc)
```

In line 5, we create a Proc object explicitly from a block. In line 6, prefixing ampersand on proc converts the proc object to a block.

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

In this chapter, we discussed the significance of the ampersand in the conversion of block to a Proc object and vice-versa. 