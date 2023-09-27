Assignment 3: STL HashMap (short answer questions)
Names:

1. at() vs []
Explain the difference between at() and the implementation of the operator []. Why did you have to overload one and not the other?
Hint: You will likely only need to read the header comments to do this
解释一下 at() 和 运算符 [] 在实现上的区别，为什么要重载一个而不是另一个？
A: 对于不存在的键，`at()`会抛出out_of_range异常而 `[]` 会将该键插入并设置一个默认value

2. Find vs. 𝓕𝓲𝓷𝓭
In addition to the HashMap::find member function, there is also a std::find function in the STL algorithms library. If you were searching for key k in HashMap m, is it preferable to call m.find(k) or std::find(m.begin(), m.end(), k)?
Hint: on average, there are a constant number of elements per bucket. Also, one of these functions has a faster Big-O complexity because one of them uses a loop while another does something smarter.
HashMap有一个find函数，同时在STL库中也有一个默认实现的find函数，如果我们需要在一个map中找到key k，我们倾向于使用前者而非后者，为什么？
A: `std::find()`是线性查找，时间复杂度是O(n)，HashMap的`find`是Hash查找，一般情况下是O(1)，最坏情况是O(n)

3. RAII?
This HashMap class is RAII-compliant. Explain why.
这个 HashMap 是否符合 RAII 原则？
A: 符合，它在构造函数中分配资源，在析构函数中通过clear函数释放资源

4. Increments
Briefly explain the implementation of HashMapIterator's operator++, which we provide for you. How does it work and what checks does it have?
解释一下 HashMapIterator 中的 ++ 运算符的实现？
```c++
template <typename Map, bool IsConst>
HashMapIterator<Map, IsConst> HashMapIterator<Map, IsConst>::operator++(int) {
    auto copy = *this; // calls the copy constructor to create copy
    ++(*this);
    return copy;
}
```
上面代码实现为后置的++，即 `iterator++`

- 先复制当前iterator对象（不是指针）
- 调用当前iterator对象的前置++，该前置++作用为使得this指向下一个Iterator
- 返回复制的iterator

```c++
template <typename Map, bool IsConst>
HashMapIterator<Map, IsConst>& HashMapIterator<Map, IsConst>::operator++() {
    _node = _node->next; // _node can't be nullptr - that would be incrementing end()
    if (_node == nullptr) { // if you reach the end of the bucket, find the next bucket
        for (++_bucket; _bucket < _buckets_array->size(); ++_bucket) {
            _node = (*_buckets_array)[_bucket];
            if (_node != nullptr) {
                return *this;
            }
        }
    }
    return *this;
}
```

  上述为前置的++，即 `++iterator`

- 直接找当前bucket的下一个结点，若有直接返回
- 若没有则找下一个bucket的第一个结点，若有直接返回
- 如果所有bucket都没有，则返回nullptr，也就是end()