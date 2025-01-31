# Algorithmic Complexity

[Complexity Graph](https://miro.medium.com/v2/resize:fit:1400/1*dWet_YU-5072Kcko7LzsuQ.jpeg)

## Find duplicates
### Brute-force Approach
Iterate through the array twice and identify any
```ruby
def find_duplicates(array)
  results = []
  array.each_with_index do |a, index_a|
    array.each_with_index do |b, index_b|
      results << a if a == b && index_a != index_b
    end
  end

  return results
end
```

#### Time Complexity
O(N^2) - Nested array interation

#### Space Complexity
O(k) - where k is the number of duplicates - this is maximally O(N) (if every element in array is a duplicate)


### Hash-map based approach

Create a hash which stores every element seen as a key. We can use hash-lookup to check if the element has already been seen before.

```ruby
def find_duplicates_using_hash(array)
  seen = {}
  results = []
  array.each do |a|
    if seen[a] == true
      results << a
    end

    seen[a] = true
  end

  return results
end
```

#### Time Complexity
O(N) - Iterating through the array is O(N). Hash Lookup is O(N) as well

#### Space Complexity
O(N) - Hash will always have size of all unique integers

#### Pros and Cons
Pros: Simple to understand and implement.
Cons: Inefficient for larger datasets due to the quadratic time complexity.

### Comparison
The hash-map approach is unambiguously better. It has significantly better time complexity (O(n) vs. O(n^2)) and while it uses more space, the space usage is still linear and usually an acceptable trade-off for the massive performance gain.

---

## Finding the Index of a Value in a Sorted Array
### Brute-force Approach (Linear Search)
Iterate through the array and return index when value equals taget
```ruby
def find_index(array, target)
  array.each_with_index do |a, index|
    return index if a == target
  end
end
```

#### Time Complexity
O(N) - Iterating through array

#### Space Complexity
O(1) - Size of variables does not increase with size of inputs

#### Pros and Cons
Pros: Works for unsorted arrays.
Cons: Inefficient for larger datasets if they are sorted.

### Binary Search

Since array is sorted, we can use a binary search.

```ruby
def find_index_binary_search(array, target)
  low = 0
  high = arr.length - 1

  while low <= high
    mid = (low + high) / 2
    if arr[mid] == target
      return mid
    elsif arr[mid] < target
      low = mid + 1
    else
      high = mid - 1
    end
  end
end
```

#### Time Complexity
O(log N) - Binary Search has log N time complexity

#### Space Complexity
O(1) - Size of variables does not increase with size of inputs

#### Pros and Cons
Pros: V efficient for sorted arrays.
Cons: Requires array to be sorted

### Comparison
For sorted arrays, binary search is unambiguously better. Its logarithmic time complexity makes it drastically faster for larger datasets. If the array is unsorted, you'd have to sort it first, making the sort+binary search approach O(n log n), which is still often better than a linear search on a large array.

---

## Finding the Index of a Value in an unsorted Array

### Brute-Force - Linear Search
Same as before

### Sort + Binary Search

#### Time Complexity
O(N log N) - Search Algorithm takes (N log N) and Binary Search is log N - This makes N log N - log-linear complexity.

#### Space Complexity
O(N) - Size of variables does not increase with size of inputs

#### Pros and Cons
Pros: Faster than brute force for very large arrays *if you will be doing many searches*.
Cons: Slower than brute force for small arrays or if you're only doing a single search.  Uses more memory.

### Comparison
If you are only performing one or a few lookups on the array, the brute-force approach is better due to its O(n) time complexity compared to O(n log n) for the sort + binary search approach.

However, if you are performing many lookups on the same array, sorting it first and then using binary search for each lookup could eventually be more efficient. The break-even point depends on the number of searches and the size of the array.

---

## Intesection of 2 arrays

For simplicity's sake assume the arrays are of equal length.

### Brute-force Approach
Iterate through both arrays and return value when they're the same

```ruby
def intersection(array_a, array_b)
  results = []
  array_a.each do |a|
    array_b.each do |b|
      results << a if a == b
    end
  end

  return results
end
```

#### Time Complexity
O(N^2) - Nested Array Iteration

#### Space Complexity
O(K) - Stores the intesection. Maximally N

#### Pros and Cons
Pros: Straightforward.
Cons: Inefficient for larger inputs

### Using Sets

```ruby
def intesection(array_a, array_b)
  set_a = Set.new(array_a)
  set_b = Set.new(array_a)

  return set_a.intersection(set_b)
end
```

#### Time Complexity
O(N) - Converting an array to a set is O(N). Getting intersection is O(N). All together - O(N + N +N) reduces down to O(N)

#### Space Complexity
O(N) - Sets will store all the inputs. The more inputs, the more space is used. Preious approach is *potentially* less space as only intersections are stored.

#### Pros and Cons
Pros: Much more efficient for larger arrays.
Cons: Uses more memory.

### Comparison
The set approach is unambiguously better. The time complexity is significantly improved, and the space complexity, while higher, is still linear and usually a worthwhile trade-off.

---

## Detect Palindrome

### Brute-force Approach
Iterate through both arrays and return value when they're the same

```ruby
def intersection(array_a, array_b)
  results = []
  array_a.each do |a|
    array_b.each do |b|
      results << a if a == b
    end
  end

  return results
end
```

#### Time Complexity
O(N) - It takes N time to reverse a string as we have to iterate through all of the characters. Comparing 2 strings takes maximally O(N) time. O(N + N) = O(N)

#### Space Complexity
O(N) - We have to create new reversed string of equal size to the input.

#### Pros and Cons
Pros: Conceptually simple. Easy to write.
Cons: Creates a copy of the string, using additional memory.

### Using Pointers
Point at first  of string and end of string. If not equal return false, otherwise continue with 2nd character and 2nd last character. Repeat until you hit the middle.

```ruby
def palindrome_two_pointers?(string)
  left = 0
  right = string.length - 1

  while left < right
    return false if string[left]!= string[right]
    left += 1
    right -= 1
  end
  true
end
```

#### Time Complexity
O(N) - In reality it takes N/2 time, as we're only iterating till we hit the middle. This simplifies down to O(N) as it is still linear.
You may be thinking - doesn't `string.length` involve iterating through entire string to calculate the size? Nope 😊 Strings inherently contain this value. It only takes O(1) time to fetch it!

> In most common Ruby implementations (CRuby, YARV, etc.), calling str.length (or equivalently str.size) is a constant-time operation, or O(1).
> Here's why:
> String Representation:  Ruby strings are often implemented internally in a way that stores the length of the string along with the string's character data.  This means that when you call str.length, the length is readily available; the interpreter doesn't have to traverse the entire string to count the characters.
> No Traversal:  str.length simply retrieves the pre-stored length value.  It doesn't iterate through the characters of the string.

#### Space Complexity
O(1) - We're not creating a variable that depends on the input size. We're just creating 2 integer variables so space is constant.

#### Pros and Cons
Pros: Very memory-efficient.  Slightly faster in practice because it avoids creating a copy of the string.
Cons: Might be slightly less immediately intuitive to some than the reversed string approach.

### Comparison
The two-pointer approach is unambiguously better.  While both approaches have O(n) time complexity, the two-pointer approach uses constant, O(1), space complexity.  The reversed string approach requires O(n) space to store the reversed copy.  Since memory usage is often a concern, especially with very large strings, the two-pointer approach is almost always the preferred method for palindrome checking.  It's also slightly faster in practice due to avoiding the string copying.