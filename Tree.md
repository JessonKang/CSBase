`#include<iostream>
#include<vector>
#include<algorithm>
#include<string>
#include<map>
#include<stack>
#include<queue>
using namespace std;
struct TreeNode {
	int val;
	TreeNode *left;
	TreeNode *right;
	TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
struct ListNode {
	int val;
	ListNode *next;
	ListNode(int x) : val(x), next(NULL) {}
};
class Solution {
public:
	/* 二叉树前序遍历,非递归算法，利用栈实现 */
	vector<int> preorderTraversal(TreeNode* root) {
#if 0
		vector<int> res;
		if (!root) return res;
		stack<TreeNode*> nodes;
		TreeNode *p = root;
		while (p != NULL || !nodes.empty())
		{
			while (p != NULL)
			{
				res.push_back(p->val);
				nodes.push(p);
				p = p->left;
			}
			if (!nodes.empty()) {
				p = nodes.top();
				nodes.pop();
				p = p->right;
			}
		}
		return res;
#else
		/* 统一化风格非递归代码 */
		vector<int> res;
		if (!root) return res;
		/* 当前节点，以及是否已被访问 */
		stack<pair<TreeNode*, bool>> nodes;
		nodes.push(make_pair(root, false));
		bool visited;
		while (!nodes.empty())
		{
			root = nodes.top().first;
			visited = nodes.top().second;
			nodes.pop();
			if (root == nullptr)
				continue;
			if (visited)
				res.push_back(root->val);
			else {
				if (root->right)
					nodes.push(make_pair(root->right, false));
				if (root->left)
					nodes.push(make_pair(root->left, false));
				nodes.push(make_pair(root, true));
			}
		}
		return res;
#endif
	}
	/* 二叉树中序遍历,非递归算法，利用栈实现 */
	vector<int> inorderTraversal(TreeNode* root) {
#if 0
		vector<int> res;
		if (!root) return res;
		stack<TreeNode*> nodes;
		TreeNode *p = root;
		while (p != NULL || !nodes.empty())
		{
			while (p != NULL)
			{
				nodes.push(p);
				p = p->left;
			}
			if (!nodes.empty()) {
				p = nodes.top();
				res.push_back(p->val);
				nodes.pop();
				p = p->right;
			}
		}
		return res;
#else
		/* 统一化风格非递归代码 */
		vector<int> res;
		if (!root) return res;
		/* 当前节点，以及是否已被访问 */
		stack<pair<TreeNode*, bool>> nodes;
		nodes.push(make_pair(root, false));
		bool visited;
		while (!nodes.empty())
		{
			root = nodes.top().first;
			visited = nodes.top().second;
			nodes.pop();
			if (root == nullptr)
				continue;
			if (visited)
				res.push_back(root->val);
			else {
				if (root->right)
					nodes.push(make_pair(root->right, false));
				nodes.push(make_pair(root, true));
				if (root->left)
					nodes.push(make_pair(root->left, false));
			}
		}
		return res;
#endif
	}
	/* 二叉树后序遍历,非递归算法，利用栈实现 */
	vector<int> postorderTraversal(TreeNode* root) {
#if 0
		vector<int> res;
		if (!root) return res;
		stack<TreeNode*> nodes;
		TreeNode *p = root,*pre = NULL;
		while (p != NULL || !nodes.empty())
		{
			while (p != NULL)
			{
				nodes.push(p);
				p = p->left;
			}
			p = nodes.top();
			if (p->right == NULL || p->right == pre)
			{
				res.push_back(p->val);
				nodes.pop();
				pre = p;
				p = NULL;
			}
			else {
				p = p->right;
				pre = NULL;
			}
		}
		return res;
#else
		/* 统一化风格非递归代码 */
		vector<int> res;
		if (!root) return res;
		/* 当前节点，以及是否已被访问 */
		stack<pair<TreeNode*, bool>> nodes;
		nodes.push(make_pair(root, false));
		bool visited;
		while (!nodes.empty())
		{
			root = nodes.top().first;
			visited = nodes.top().second;
			nodes.pop();
			if (root == nullptr)
				continue;
			if (visited)
				res.push_back(root->val);
			else {
				nodes.push(make_pair(root, true));
				if (root->right)
					nodes.push(make_pair(root->right, false));
				if (root->left)
					nodes.push(make_pair(root->left, false));
			}
		}
		return res;
#endif
	}
	/* 二叉树层次遍历,递归算法 */
	/*
	给你一个二叉树，请你返回其按 层序遍历 得到的节点值。 （即逐层地，从左到右访问所有节点）。
	示例：
	二叉树：[3,9,20,null,null,15,7],
    3
   / \
  9  20
    /  \
	15   7
	返回其层次遍历结果：
	[
		[3],
		[9,20],
		[15,7]
	]
	*/
	vector<vector<int>> levelOrder(TreeNode* root) {
		vector<vector<int>> ans;
		pre(root, 0, ans);
		return ans;
	}
	void pre(TreeNode *root, int depth, vector<vector<int>> &ans) {
		if (!root) return;
		if (depth >= ans.size())
			ans.push_back(vector<int> {});
		ans[depth].push_back(root->val);
		pre(root->left, depth + 1, ans);
		pre(root->right, depth + 1, ans);
	}
	/* 二叉树层次遍历,非递归算法 */
	vector<vector<int>> levelOrder2(TreeNode* root) {
		vector<vector<int>> res;
		if (!root) return res;
		int size = 0;
		queue<TreeNode*> nodes; /* 队列 */
		nodes.push(root);
		while (!nodes.empty())
		{
			vector<int> vals;
			size = nodes.size();
			while (size > 0)
			{
				TreeNode *f = nodes.front();
				nodes.pop();/* 弹出队列 */
				vals.push_back(f->val);
				if (f->left) nodes.push(f->left);
				if (f->right) nodes.push(f->right);
				size--;
			}
			res.push_back(vals);
		}
		return res;
	}
	/*
	给定一个二叉树，返回其节点值自底向上的层次遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）
	例如：
	给定二叉树 [3,9,20,null,null,15,7],
    3
   / \
  9  20
    /  \
   15   7
	返回其自底向上的层次遍历为：
	[
		[15,7],
		[9,20],
		[3]
	]
	*/
	vector<vector<int>> levelOrderBottom(TreeNode* root) {
		vector<vector<int>> res;
		if (!root) return res;
		int size = 0;
		queue<TreeNode*> nodes; /* 队列 */
		nodes.push(root);
		while (!nodes.empty())
		{
			vector<int> vals;
			size = nodes.size();
			while (size > 0)
			{
				TreeNode *f = nodes.front();
				nodes.pop();/* 弹出队列 */
				vals.push_back(f->val);
				if (f->left) nodes.push(f->left);
				if (f->right) nodes.push(f->right);
				size--;
			}
			res.push_back(vals);
		}
		reverse(res.begin(), res.end());
		return res;
	}
};
void main()
{	
	system("pause");
}`

