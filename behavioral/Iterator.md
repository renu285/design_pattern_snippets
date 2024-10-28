
### Iterator 

The Iterator design pattern is a behavioral pattern that provides a way to access the elements of an aggregate object sequentially without exposing 
its underlying representation. It separates the traversal of a collection from the collection itself.Iterator is a behavioral design pattern that 
lets you traverse elements of a collection without exposing its underlying representation (list, stack, tree, etc.).

~~~
#include <iostream>
#include <queue>
#include <stack>

// TreeNode class
class TreeNode {
public:
    int data;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int value) : data(value), left(nullptr), right(nullptr) {}
};

// Iterator interface
class Iterator {
public:
    virtual bool hasNext() const = 0;
    virtual int next() = 0;
    virtual ~Iterator() = default;
};

// Binary Tree class
class BinaryTree {
private:
    TreeNode* root;

public:
    BinaryTree() : root(nullptr) {}

    void insert(int value) {
        root = insertRecursive(root, value);
    }

    class InOrderIterator : public Iterator {
    private:
        std::stack<TreeNode*> stack;

    public:
        InOrderIterator(TreeNode* root) {
            pushLeft(root);
        }

        bool hasNext() const override {
            return !stack.empty();
        }

        int next() override {
            TreeNode* node = stack.top();
            stack.pop();
            pushLeft(node->right);
            return node->data;
        }

    private:
        void pushLeft(TreeNode* node) {
            while (node != nullptr) {
                stack.push(node);
                node = node->left;
            }
        }
    };

    class LevelOrderIterator : public Iterator {
    private:
        std::queue<TreeNode*> queue;

    public:
        LevelOrderIterator(TreeNode* root) {
            if (root != nullptr) {
                queue.push(root);
            }
        }

        bool hasNext() const override {
            return !queue.empty();
        }

        int next() override {
            TreeNode* node = queue.front();
            queue.pop();
            if (node->left != nullptr) queue.push(node->left);
            if (node->right != nullptr) queue.push(node->right);
            return node->data;
        }
    };

    Iterator* createInOrderIterator() {
        return new InOrderIterator(root);
    }

    Iterator* createLevelOrderIterator() {
        return new LevelOrderIterator(root);
    }

private:
    TreeNode* insertRecursive(TreeNode* node, int value) {
        if (node == nullptr) {
            return new TreeNode(value);
        }
        if (value < node->data) {
            node->left = insertRecursive(node->left, value);
        } else if (value > node->data) {
            node->right = insertRecursive(node->right, value);
        }
        return node;
    }
};

int main() {
    BinaryTree tree;
    tree.insert(5);
    tree.insert(3);
    tree.insert(7);
    tree.insert(1);
    tree.insert(9);
    tree.insert(4);
    tree.insert(6);

    std::cout << "In-order traversal: ";
    Iterator* inOrderIt = tree.createInOrderIterator();
    while (inOrderIt->hasNext()) {
        std::cout << inOrderIt->next() << " ";
    }
    std::cout << std::endl;

    std::cout << "Level-order traversal: ";
    Iterator* levelOrderIt = tree.createLevelOrderIterator();
    while (levelOrderIt->hasNext()) {
        std::cout << levelOrderIt->next() << " ";
    }
    std::cout << std::endl;

    delete inOrderIt;
    delete levelOrderIt;

    return 0;
}
~~~