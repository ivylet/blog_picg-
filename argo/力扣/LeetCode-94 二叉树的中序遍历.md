给定一个二叉树的根节点 root ，返回 它的 中序 遍历

示例 1:
输入：root = [1,null,2,3]  
输出：[1,3,2]  
  
示例 2:
输入：root = []  
输出：[]  
  
示例 3: 
输入：root = [1]  
输出：[1]  

- 提示：
树中节点数目在范围 [0, 100] 内 // -100 <= Node.val <= 100 
进阶: 递归算法很简单，你可以通过迭代算法完成吗？ // Related Topics 栈 树 深度优先搜索 二叉树 👍 1491 👎 0  
  package leetcode.editor.cn;  
```java
import javax.swing.tree.TreeNode;  
  import java.util.ArrayList;  
  import java.util.LinkedList;  
  import java.util.List;  
  import java.util.Map;  
  
class BinaryTreeInorderTraversal{  
    public static void main(String[] args) {  
         Solution solution = new BinaryTreeInorderTraversal().new Solution();  
    }  
    //leetcode submit region begin(Prohibit modification and deletion)  
// Definition for a binary tree node.
public class TreeNode {
	int val;
	TreeNode left;
	TreeNode right;
	TreeNode() {}
	TreeNode(int val) { 
	this.val = val; 
	}
	TreeNode(int val, TreeNode left, TreeNode right){
	this.val = val;
	this.left = left;
	this.right = right;
	} 
} 
```
题解:
直接用递归的方法,简单暴力.
```java
class Solution {  
	public List<Integer> inorderTraversal(TreeNode root){  
        List<Integer> list = new ArrayList<>();  
        indorder(list,root);  
        return list;  
	}  
    public void indorder(List<Integer> list, TreeNode root){  
        if (root == null) {  
            return;  
        }  
        indorder(list,root.left);  
        list.add(root.val);  
        indorder(list,root.right);  
    }  
}  
}
```
题解:
使用栈的方法来实现迭代
先一直取左孩子到底部加入栈,然后取出元素加入结果集,再回到父节点,将父节点加入结果集,再考虑父节点的右孩子,如果有就继续,无就再回到上一层.
```Java
class Solution {  
    public List<Integer> inorderTraversal(TreeNode root) {  
     List<Integer> ans = new ArrayList<Integer>();  
     Deque<TreeNode> stk = new LinkedList<TreeNode>();  
     while(root != null || !stk.isEmpty() ){  
         while (root != null){  
         // 非空就是没有到最底部
             stk.push(root);  
             root = root.left;  
         }  
         // 取出栈中最后的元素 加入结果集.
         root = stk.pop();  
         ans.add(root.val);  
         root = root.right;  
     }  
     return ans;  
  
    }  
}
```
  