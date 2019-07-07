# 二叉树的遍历

### 二叉树的数据结构

```c
typedef struct TNode{
    int data;
    struct TNode *lchild, *rchild;
}TNode;
```

### 二叉排序树的建立

```c
void InsertTNode(TNode * root, TNode * node){
    if(root!=NULL&&node!=NULL){
        if(node->data<root->data){
            if(root->lchild!=NULL)InsertTNode(root->lchild, node);
            else root->lchild = node;
        } else{
            if(root->rchild!=NULL)InsertTNode(root->rchild, node);
            else root->rchild = node;
        }
    }
}
```

```c
TNode * CreatBinaryTree(const int * a, int n){
    TNode * root = (TNode*)malloc(sizeof(TNode));
    root->lchild = NULL;
    root->rchild = NULL;
    root->data = a[0];
    for(int i=1;i<n;i++){
        TNode * node = (TNode*)malloc(sizeof(TNode));
        node->lchild = NULL;
        node->rchild = NULL;
        node->data = a[i];
        InsertTNode(root, node);
    }
    return root;
}
```

### 先序遍历

```c
void PreOrder(TNode * root){
    if(root!=NULL){
        printf("%d,", root->data);
        PreOrder(root->lchild);
        PreOrder(root->rchild);
    }
}
```

### 中序遍历

```c
void InOrder(TNode * root){
    if(root!=NULL){
        InOrder(root->lchild);
        printf("%d,", root->data);
        InOrder(root->rchild);
    }
}
```

### 后序遍历

```c
void PostOrder(TNode * root){
    if(root!=NULL){
        PostOrder(root->lchild);
        PostOrder(root->rchild);
        printf("%d,", root->data);
    }
}
```

