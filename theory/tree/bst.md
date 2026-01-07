# [Binary Search Tree](https://ko.wikipedia.org/wiki/이진_탐색_트리)

루트노드를 기준으로 두 개의 자식노드를 가질 수 있는 노드들로 구성된 트리<br>
노드의 왼쪽 서브트리는 그 노드보다 작은 값들을 담고, 오른쪽 서브트리는 더 큰 값을 가진 값들을 담는다.<br>
어느 노드를 확인 하더라도 하위 트리는 이진 탐색 트리를 만족한다.

추가(삽입), 검색, 제거 등의 기능이 필요하며 정렬된 데이터를 이진 탐색을 통해 빠르게 검사할 수 있음


## 변경 Log
> 노드, 이진탐색 트리의 기본 구조, 함수를 작성함<br>


## 이진 탐색 트리 코드



```cpp
///////////////////////////////////////////////////////
///BST.h
///////////////////////////////////////////////////////
class BST
{
private:
	Node* root;
	int count;

public:
	BST()
	{
		root = nullptr;
		count = 0;
	}

	~BST()
	{
		Clear(root);
	}

	int Count() const {
		return count;
	}

	void Add(int value);
	bool Remove(int value);
	Node* Search(int value);

	void Clear(Node*& ptr);
};
///////////////////////////////////////////////////////
//BST.cpp
///////////////////////////////////////////////////////
void  BST::Add(int value)
{
	count++;

	if (root)
		root = new Node(value);
	else
		root->Add(new Node(value));
};

bool  BST::Remove(int value)
{
	if (!root)
		return false;
	bool result = Node::Remove(root, value);

	count -= result;
	return result;
};

Node* BST::Search(int value)
{
	if (root == nullptr)
		return nullptr;
	else
		return root->Search(value);
};

void BST::Clear(Node*& ptr)
{
	if (!ptr) return;
	Clear(ptr->right);
	Clear(ptr->left);

	delete(ptr);
};

///////////////////////////////////////////////////////
//Node.h
///////////////////////////////////////////////////////
class Node
{
public:
	int data;
	Node* left;
	Node* right;

public:
	Node(int data) : data(data){
		left = nullptr;
		right = nullptr;
	}

	Node* Search(int value);
	void Add(Node* n);
	static bool Remove(Node*& ptr, int value);
	static Node* GetMin(Node* ptr);

};

///////////////////////////////////////////////////////
//Node.cpp
///////////////////////////////////////////////////////
Node* Node::Search(int value)
{
	if (data == value)
		return this;
	else if (left && data > value)
		return left->Search(value);
	else if (right && data < value)
		return right->Search(value);

	return nullptr;
}

void Node::Add(Node* n)
{
	if (n->data <= this->data) {
		if (left)
			left = n;
		else
			left->Add(n);
	}
	else {
		if (right)
			right = n;
		else
			right->Add(n);
	}
}

bool Node::Remove(Node*& ptr, int value)
{
	//종단노드
	if (!ptr)
		return false;
	//잎노드로 이동
	if (value < ptr->data)
		return Remove(ptr->left, value);
	else if (value > ptr->data)
		return Remove(ptr->right, value);
	//내가(ptr) 제거될 대상임
	else
	{
		//오른쪽 서브트리의 가장 작은 값으로 변경
		if (ptr->left && ptr->right)
		{
			Node* rightMin = GetMin(ptr->right);
			ptr->data = rightMin->data;
			
			return Remove(ptr->right, rightMin->data);
		}
		else
		{
			Node* temp = ptr;
			ptr = ptr->left ? ptr->left : ptr->right;
			delete temp;
			return true;
		}
	}
}

Node* Node::GetMin(Node* ptr)
{
	while (ptr->left)
	{
		ptr = ptr->left;
	}
	return ptr;
}
```