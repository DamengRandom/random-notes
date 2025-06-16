binary tree:
- each node has at most 2 children
- left child is less than parent
- right child is greater than parent

binary search tree:
- binary tree with a specific ordering of elements
- left child is less than parent
- right child is greater than parent

binary tree traversal:
- in-order: left, root, right
- pre-order: root, left, right
- post-order: left, right, root

Eg:

   3
  / \
 1   5
/ \ / \
0 2 4  6

0 1 2 3 4 5 6 - Inorder sorted traversal process (From Left -> Root -> Right) !!!!

3 1 0 2 5 4 6 - Preorder traversal process (From Root -> Left -> Right) !!!!

0 2 1 4 6 5 3 - Postorder traversal process (From Left -> Right -> Root) !!!!
