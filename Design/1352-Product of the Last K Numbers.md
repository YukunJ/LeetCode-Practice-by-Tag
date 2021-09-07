**1352. Product of the Last K Numbers**

```Tag : design/prefix/data stream/math/array```

**Description:**

Design an algorithm that accepts a stream of integers and retrieves the product of the last ```k``` integers of the stream.

Implement the ```ProductOfNumbers``` class:

+ ```ProductOfNumbers()``` Initializes the object with an empty stream.

+ ```void add(int num)``` Appends the integer ```num``` to the stream.

+ ```int getProduct(int k)``` Returns the product of the last ```k``` numbers in the current list. You can assume that always the current list has at least ```k``` numbers.

The test cases are generated so that, at any time, the product of any contiguous sequence of numbers will fit into a single 32-bit integer without overflowing.

**Example1:**

        Input
        ["ProductOfNumbers","add","add","add","add","add","getProduct","getProduct","getProduct","add","getProduct"]
        [[],[3],[0],[2],[5],[4],[2],[3],[4],[8],[2]]

        Output
        [null,null,null,null,null,null,20,40,0,null,32]

        Explanation
        ProductOfNumbers productOfNumbers = new ProductOfNumbers();
        productOfNumbers.add(3);        // [3]
        productOfNumbers.add(0);        // [3,0]
        productOfNumbers.add(2);        // [3,0,2]
        productOfNumbers.add(5);        // [3,0,2,5]
        productOfNumbers.add(4);        // [3,0,2,5,4]
        productOfNumbers.getProduct(2); // return 20. The product of the last 2 numbers is 5 * 4 = 20
        productOfNumbers.getProduct(3); // return 40. The product of the last 3 numbers is 2 * 5 * 4 = 40
        productOfNumbers.getProduct(4); // return 0. The product of the last 4 numbers is 0 * 2 * 5 * 4 = 0
        productOfNumbers.add(8);        // [3,0,2,5,4,8]
        productOfNumbers.getProduct(2); // return 32. The product of the last 2 numbers is 4 * 8 = 32 

**Hints**:

+ Keep all prefix products of numbers in an array, then calculate the product of last K elements in ```O(1)``` complexity.

+ When a zero number is added, clean the array of prefix products.
 
-----------

```python
class ProductOfNumbers:
    """
    Follow the hint, we might need to store the prefix product
    But the thinking process should go like this:
    since the input "k" is arbitrary, we have to store all the input number
    This already creates O(n) space complexity, where n := number of add() is called
    therefore in term of asymptotic analysis, it's OK to allocate another linear vector
    if it could boost our performance in calling getProduct(k) in O(1) time complexity
    essentially, we want to do prefix_prod[n] / prefix_prod[n-k] to retrieve the product for last k
    One headache is, if there is a 0 in preceding, we will might zero division problem.
    Therefore, whenever we meet a 0, we need to reset the prefix array to 1 in next position
    
    denote n := number of add() is called
    Time Complexity : __init__() is O(1), add() is O(1), getProduct is O(1)
    Space Complexity : O(n) for prefix storage
    """
    def __init__(self):
        self.prefix = [1]
        self.nonzero_count = 0
        
    def add(self, num: int) -> None:
        if num != 0:
            self.prefix.append(self.prefix[-1] * num)
            self.nonzero_count += 1
        else:
            # clear the prefix array, since a zero ruin all previous products
            self.prefix.clear()
            self.prefix.append(1)
            self.nonzero_count = 0

    def getProduct(self, k: int) -> int:
        if self.nonzero_count < k:
            # a zero appears within the closest window of k
            # the last-k-product result must be 0
            return 0
        else:
            return self.prefix[-1] // self.prefix[-1-k]


# Your ProductOfNumbers object will be instantiated and called as such:
# obj = ProductOfNumbers()
# obj.add(num)
# param_2 = obj.getProduct(k)
```

