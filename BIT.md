# 树状数组 (BIT) 总结


RSQ (Range Sum Query) To find the the sum from 1th to nth element in nums[].
* [307. Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)
* [308. Range Sum Query 2D - Mutable](https://leetcode.com/problems/range-sum-query-2d-mutable/)
* [315. Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)


## 1. Build 
  
  建立一个虚拟的二叉树, 每个节点有label 和 val 两个属性. val 部分我们用数组 tree[]来实现
  
  * **label**: idx of nums[]; **start form 1 instead of 0**
      
  * **val**: Sum of itself and all nums[i], for i is the label of all node in n's left subtree;(自己本身的值以及所有左子树的点的值的总和)


                                             4
                                           [+32]
                                          /     \
                                       2           6
                                     [ +6]       [+80]
                                     /   \       /   \
                                  > 1     3     5     7
                                  [ +5] [+15] [+52] [ +0]

## 2. update num[i]

* Start from node n(need to be update), go up to root, add diff to node's value if we follow a right link to it.

* update num[2] (**node3**):

                                             4
                                           [+32]
                                          / -> a right link, update node4's val
                                       2       
                                     [ +6]      
                                         \  -> a left link, do not update node2's val
                                          3 -> update targetNode's val firstly
                                        [+15]
                                        

      3->2->4 : tree[3] += diff;
                tree[4] += diff;
           
* To implement update() method, we can use bit manipulation to do the magic::

      1. Write out node n in binary.
      2. Set the counter to 0.
      3. Repeat the following while n <= max:
      4. Add diff to node n.
      5. Set the n's rightmost 0 bit to 1.
      
* Node that i & (-i) is to find the mostright 1, i += i & (-i) will set the rightmost 0 to 1:

    ```java
            int i = n; 
            while (i < tree.length) {
                tree[i] ++;
                i += i & (-i);
            }

    ```
## 3. lookup range sum from start to node-n

* Start from node n(need to lookup), go  up to root, add all node's value if we follow a left link to it.
* lookup rang sum from nums[0] (node1) to nums[2] (**node3**)

                                             4
                                           [+32]
                                          / -> a right link, do not add node4's val
                                       2       
                                     [ +6]      
                                         \  -> a left link, add node2's val
                                          3 -> add targetNode's val firstly
                                        [+15]
                                        
                                        
      RangSum(0, 2) = tree[3] + tree[2];
            
* To implement lookup() method, we can use bit manipulation to do the magic::

			1. Write out node n in binary.
			2. Set the counter to 0.
			3. Repeat the following while n != 0:
			4. Add in the value at node n.
			5. Clear the rightmost 1 bit from n.
      
* Node that i & (-i) is to find the mostright 1, i -= i & (-i) will set the rightmost 1 to 0:

    ```java
            int num = 0;
            int i = n;
            while (i > 0) {
                num +=tree[i];
                i -= i&(-i);
            }
            return num;
    ```
