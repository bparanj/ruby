## Symbol to Proc

### Concepts Covered

1. *args in the method argument.
2. Providing default value for block variable.
3. Using send to message an object.

In this lesson, you will learn how to convert code within a block to use ampersand-colon shortcut. So, this code:

```ruby
result = ['hi', 'how'].map {|x| x.upcase }
```

becomes:

```ruby
result = ['hi', 'how'].map(&:upcase)
```

Our goal is to go from:

```
map {|x| x.upcase } ----> map(&:upcase)
```

::: tip NOTE
The map method takes a block to execute in the first case. It takes a Symbol object as an argument in the second case.
:::

::: tip PRE-REQUISITE CONCEPT
The & in the argment converts the Symbol object to a Proc object by calling to_proc on it.
:::

The object in this case is an instance of Symbol. We need to implement `to_proc` instance method in Symbol class.

```ruby
class Symbol
  def to_proc
    Proc.new {|o| o.upcase }
  end
end

result = ['hi', 'how'].map(&:upcase)
p result
```

This is a quick and dirty solution that will work only for `upcase`. We need the ablility to send any message to the block variable. 

```ruby
def to_proc
  Proc.new {|o| o.send(self) }
end
```

We can use `send` method to send a message that is represented by the symbol. However, this solution will only work for blocks that take only one block variable.

## Multiple Block Variables

Our solution will break if we have more than one block variable. Consider:

```ruby
output = [1,2,3,4].inject(0) {|result, element| result + element }
p output
```

In this case, the result and element are the two block variables. Ruby is capable of simplifying the code.

```ruby
output = [1,2,3,4].inject(&:+)
p output
```

If we use our current `to_proc` implementation, this code will break. We can fix by passing the arguments to the `send` method.

```ruby
def to_proc
  Proc.new {|o, args| o.send(self, args) }
end
```

But this will break our previous one block variable example:

```ruby
result = ['hi', 'how'].map(&:upcase)
p result
```

We can handle the one block variable case by initializing the second block variable to `nil`. We also need to use the splat operator in the second argument to `send` method.

```ruby
def to_proc
  Proc.new {|o, args=nil| o.send(self, *args) }
end
```

Now we can handle one or more block variables. Our final solution:

```ruby
class Symbol
  def to_proc
    Proc.new {|o, args=nil| o.send(self, *args) }
  end
end

output = [1,2,3,4].inject(&:+)
p output
```
