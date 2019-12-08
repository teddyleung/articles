# Spicy Ruby Array Methods

In this tutorial, we're going to cover several Ruby array methods that will help level up your code! Basic "for" loops work, but array methods tend to make your code shorter, cleaner, and more descriptive. Getting comfortable with these array methods will make you a better developer.

## The Basic Loops

Before we dive into array methods, let's make sure we understand the most basic way to iterate over an array: the "for..in" loop.

```ruby
numbers = [1, 2, 3, 4, 5]

for number in numbers do
  puts number
end
```

For each element in the **numbers** array, Ruby assigns the **number** variable that element in the array. We can then access that element via **number** and print it to the console.

However, you'll find that the "for..in" loop is not commonly used. What you'll see more often is the **each** array method.

```ruby
numbers = [1, 2, 3, 4, 5]

numbers.each do |number|
  puts number
end
```

The **each** loop works similarly to the "for..in" loop. Ruby iterates over the **numbers** array and inputs the current element as the block parameter **number**. We can then access the element via **number** and print it to the console.

Going forward, we may compare the other ruby array methods with **each** to see how the other array methods result in cleaner code.

## Find

If you want to find an element in an array that matches some criteria, Ruby provides the **find** method. The **find** method iterates over an array and returns the first element where the block returns true. If no element is found, it returns nil.

Find the student with the name David:

```ruby
students = [
  { name: 'Alice', age: 15 },
  { name: 'Bob', age: 12 },
  { name: 'Carol', age: 17 },
  { name: 'David', age: 13 },
  { name: 'Elisa', age: 18 }
]

# Using .find
def find_student_with_find(name, students)
  students.find do |student|
    student[:name] == name
  end
end


# Using .each
def find_student_with_each(name, students)
  students.each do |student|
    if student[:name] == name
      return student
    end
  end

  # return nil if name is not found
  nil
end


p find_student_with_find('David', students)
p find_student_with_each('David', students)
```

## Select

If you want to filter an array to keep only the elements that match certain criteria, Ruby provides the **select** method. The **select** method is very similar to the **find** method, except it returns **all** elements that match the criteria instead of just the **first** one.

As with the **find**, provide the **select** method with a block that returns true when the criteria is met.

Return an array of students who are at least 15 years old:
```ruby
students = [
  { name: 'Alice', age: 15 },
  { name: 'Bob', age: 12 },
  { name: 'Carol', age: 17 },
  { name: 'David', age: 13 },
  { name: 'Elisa', age: 18 }
]

# Using .select
def get_students_with_select(age, students)
  students.select do |student|
    student[:age] >= age
  end
end


# Using .each
def get_students_with_each(age, students)
  filteredStudents = []
  
  students.each do |student|
    if student[:age] >= age
      filteredStudents.push(student)
    end
  end

  filteredStudents
end


p get_students_with_select(15, students)
p get_students_with_each(15, students)
```

## Sort

Sometimes you may want to sort your array. Ruby has a method for that, and it's called **sort**. The **sort** method accepts an optional block with two parameters, say **first** and **second**, which returns 1 (if first > second), 0 (if first == second), or -1 (if first < second).

Sort the students by age in ascending order and descending order:
```ruby
students = [
  { name: 'Alice', age: 15 },
  { name: 'Bob', age: 12 },
  { name: 'Carol', age: 17 },
  { name: 'David', age: 13 },
  { name: 'Elisa', age: 18 }
]

def sort_students_asc(students)
  students.sort do |studentA, studentB|
    studentA[:age] <=> studentB[:age]
  end
end

def sort_students_desc(students)
  students.sort do |studentA, studentB|
    studentB[:age] <=> studentA[:age]
  end
end

p sort_students_asc(students)
p sort_students_desc(students)
```

Notice the use of the "<=>" (spaceship) operator. It returns 1 (if left side > right side), 0 (if left side == right side), or -1 (if left side < right side).

Recall that **sort** optionally accepts a block. If you don't provide one, the sort method will compare the array elements as is and sort in ascending order.

```ruby
numbers = [5, 3, 2, 4, 1]

numbers.sort
# returns [1, 2, 3, 4, 5]

# The default sort works similarly to this
numbers.sort do |numberA, numberB|
  numberA <=> numberB
end
```

## Map

You'll often want to take an array, do something to each element, and return a new array. This is where **map** comes in handy. The method **map** accepts a block which operates on each element of the array. The return value within the block becomes the new element in the new array.

Let's say we wanted to take an array of numbers and return a new array with the square of each number:

```ruby
numbers = [1, 2, 3, 4, 5]

# Using .map
def square_numbers_with_map(numbers)
  numbers.map do |number|
    number**2
  end
end

# Using .each
def square_numbers_with_each(numbers)
  squared_numbers = []

  numbers.each do |number|
    squared_numbers.push(number**2)
  end

  squared_numbers
end

p square_numbers_with_map(numbers)
p square_numbers_with_each(numbers)
```

Sometimes it's useful to have access to the index position of the array. Ruby provides a chainable **with_index** method that provides access to the index value. By default, the **with_index** method starts the index count at 0, but you can optionally provide an offset value if you would like to start counting from a different number.

```ruby
# Using .map and .with_index
numbers = [1, 2, 3, 4, 5]

def power_of_index(numbers)
  numbers.map.with_index do |number, index|
    number**index
  end
end

# Using .with_index with an offset value
def power_of_index_with_offset(numbers)
  numbers.map.with_index(2) do |number, index|
    number**index
  end
end

p power_of_index(numbers)
# Prints [1, 2, 9, 64, 625]

p power_of_index_with_offset(numbers)
# Prints [1, 8, 81, 1024, 15625]
```

## Reduce

If you need to combine all elements within an array into a single value, Ruby provides the **reduce** method, which is also aliased as **inject**. Both work the same, so we will use **reduce**. 

The **reduce** method accepts an optional initial value and a block or a symbol. If a block is provided, it needs to accept two parameters: an accumulator and a parameter for the current element. If you provide a symbol, then each element of the array will be passed to the named method of the accumulator. Let's have a look!

What is the sum of all numbers in an array?
```ruby
numbers = [1, 2, 3, 4, 5]

# Using .reduce
def sum_with_reduce(numbers)
  numbers.reduce do |sum, number|
    sum + number
  end
end

# Using .each
def sum_with_each(numbers)
  sum = 0
  
  numbers.each do |number|
    sum += number
  end

  sum
end

p sum_with_reduce(numbers)
p sum_with_each(numbers)
```

In the **sum_with_reduce** function above, we passed a block to the **reduce** method which took an accumulator parameter called **sum** and a parameter which gets passed the current element called **number**.

In the above examples, we did not pass an initial value. By default, the initial value is the first element in the array. We can also explicitly pass initial values to our **reduce** methods.

```ruby
numbers = [1, 2, 3, 4, 5]

# Using .reduce
def sum_with_reduce(numbers)
  # Inputted an initial value 100
  numbers.reduce(100) do |sum, number|
    sum + number
  end
end

p sum_with_reduce(numbers)
# Prints 115
```

Finally, instead of a block, we can instead pass a symbol, which references that named method of the accumulator.

```ruby
# This is syntactic sugar for the .+ method
p 1 + 2

# This is what's actually happening
p 1.+(2)

# 1, which is an integer, has a method called :+
p 1.methods.include?(:+)
```

In Ruby, the "+" (plus) operator is a method within the Integer class. Knowing this, we can do some nifty things with **reduce** and symbols.

Sum all the numbers in an array but use symbol instead of block:
```ruby
numbers = [1, 2, 3, 4, 5]

# Without an initial value
sum = numbers.reduce(:+)

# With an initial value
sum_plus_100 = numbers.reduce(100, :+)

p sum
# Prints 15

p sum_plus_100
# Prints 115
```

## Uniq

Sometimes you want to filter an array and only keep the unique values. Ruby let's you do this with **uniq**.

```ruby
letters = ['a', 'a', 'a', 'b', 'b', 'c', 'c', 'c']

letters.uniq
# Returns ['a', 'b', 'c']
```

The **uniq** method also optionally accepts a block that describes what specifically needs to be compared for each element.

```ruby
pets = [
  {type: 'dog', name: 'Schneider'},
  {type: 'cat', name: 'Fluffy'},
  {type: 'dog', name: 'Lucky'}
]

pets.uniq {|pet| pet[:type]}
# Returns [{:type=>"dog", :name=>"Schneider"}, {:type=>"cat", :name=>"Fluffy"}]
```

And for the final trick to bring up the spice level, we'll talk about the Set union **|**. The **uniq** method is useful when you have an array with repeated values. Sometimes these repeated values come about when you concatentate two arrays. Wouldn't it be great if you could concatenate and filter for only unique values at the same time? The Set union lets you do that.

```ruby
array1 = [1, 2, 3]
array2 = [1, 2, 4]

# Using Set union
p array1 | array2
# Prints [1, 2, 3, 4]

# Using .concat then .uniq
newArray = array1.concat(array2)
# [1, 2, 3, 1, 2, 4]
p newArray.uniq
# Prints [1, 2, 3, 4]
```

## Conclusion

And there you have it - six Ruby array methods to turn up the heat!