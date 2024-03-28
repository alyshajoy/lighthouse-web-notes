# Ruby

* created in 1995
* Rails was created in 2005
* was popular from 2005-2015 (when React was created)
* there is no async in Ruby!
* is case sensitive
* no real difference between '==' and '===' in Ruby
* parentheses are always optional
* return statements are optional (unless you are trying to return early)
* hashes in ruby are the same thing as objects in js

```js
username = null

if (!username) {
  console.log("There is no username.")
}
```

```rb
username = nil
unless(username)
  puts "there is no username
end
```

Both of the blocks of code above do the same thing.

```rb
num = 10

puts "the number is 10" if num == 10

sunny = false

puts "take an umbrella" unless sunny
```

loops are often denoted by: do end (indicates a block of code)
```rb
loop do
  i += 1
  puts "hello #{i}"

  if (i >= 10)
    break
  end
end
```
Gaming Loop: (helpful in math game project)
```rb
game_over = false

until (game_over) do

end
```

- ruby's for...in === js's for...of

```rb
names = ['alice', 'bob', 'carol']

names.each do |name|
  puts "hello there #{name}!"
end

names.each do |name, index|
  puts "hello there #{name} plus #{index}!"
end
```

### Hashes

```rb
user = {
  "username" => "jstamos",
  "password" => "1234"
}

puts user["username"] # "jstamos"   -> must use [] syntax
puts user["password"] # "1234"
```

* methods that end in "?" always return a boolean
* methods that end in "!" will alter the original
* symbols => lightweight strings (have much fewer methods attached to them compared to normal strings)
  * symbols are made using : (ie. :username)

modern syntax:
```rb
user = {
  :username => "jstamos",
  :password => "1234"
}

OR

user = {
  username: "jstamos",
  password: "1234"
}

puts user[:username]
```
Use either of the above code samples -> they do the same thing!

### Blocs and Procs

- block is denoted with do...end

- Proc(edure) is a block stored in memory.
  - as close as we get in Ruby to callbacks

```rb
names = ['alice', 'bob', 'carol']

my_block = Proc.new do |name| # Proc stored everything stored between do...end
  puts "hello there #{name}"
end

names.each &my_block # & - ruby uses the ampersand to use it as an executable chunk of code

```

```rb
def accepting_block &block
  block.call 'alice'
end
```