def merge_sort(&prc) #[3,8,5]
  prc ||= Proc.new { |x,y| x <=> y}

  return self if self.length == 1

  middle = self.length / 2 = 1
  right = self.drop(middle).merge_sort [8,5], [5]
  left = self.take(middle).merge_sort [3], [8]

  self.merge(left, right, &prc)
end

def merge (left, right, &prc)
  arr = []

  until left.empty? || right.empty?
    if prc.call(left[0], right[0]) == 1
      arr << right.shift
    else
      arr << left.shift
  end   

  arr + left + right
end

class Array
  def flatten(x=nil)
    arr = []

    self.each do |ele|
      if x == nil && ele.is_a?(Array)
        arr += ele.flatten
      elsif ele.is_a?(Array) && x > 0
        arr += ele.flatten(x-1)
      else
        arr << ele
      end
    end
    arr
  end
end


[1, [2, 3, [4]]] #=> 2 total levels
0 level only: [1, [2, 3, [4]]]
1 level only: [1, 2, 3, [4]]
2 level only: [1, 2, 3, 4]

[2, 3, [4]] #=> 1 total levels
0 level only: [2, 3, [4]]
1 level only: [2, 3, 4]

[4] #=> 0 total level

class Array
  def myReduce(acc=nil, &prc) 
  if acc.nil?
    acc = self[0] 
   (1...self.length).each do |i| # { |acc, el acc + el}
     acc = prc.call(acc, self[i])
    end
   else
     (0...self.length).each do |i| # { |acc, el acc + el}
     acc = prc.call(acc, self[i])
    end
   end
  acc
end

class Array
  def myReduce(acc=nil, &prc)
    start_idx = 0
    if acc.nil?
      acc = self[0]
      start_idx = 1
    end

    (start_idx...self.length).each do |i| # { |acc, el| acc + el}
     acc = prc.call(acc, self[i])
    end

    acc
  end
end

Example:

```ruby
"cats are cool".shuffled_sentence_detector("dogs are cool") => false
"cool cats are".shuffled_sentence_detector("cats are cool") => true
```

class String
  def shuffled_sentence_detector(sent)
    selfHash = Hash.new(0)
    sentHash = Hash.new(0)

    self.split(' ').each { |word| selfHash[word] += 1} 
    sent.split(' ').each { |word| sentHash[word] += 1}

    selfHash == sentHash
  end
end

[0,1,1,2,3,5]

def fib(n)
  return [0,1] if n == 2  #n=4
  prevFibs = fib(n-1)
  prevFibs << prevFibs[-2] + prevFibs[-1]
            #fib(3)[-2] + fib(3)[-1] fib(3) = [0,1,1]
            #fib(2)[-2] + fib(2)[-1]
end