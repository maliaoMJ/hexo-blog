---
title: JavaScript数据结构与算法-树
catalog: true
tags:
  - javascript
  - 数据结构
url: 214.html
id: 214
categories:
  - 数据结构
date: 2019-03-13 19:09:28
subtitle:
header-img:
---

#### 1\. 什么是树？

树是一种分层数据的抽象模型。现实生活中最常见的树的例子是家谱，或是公司的组织架构 图，一个树结构包含一系列存在父子关系的节点。每个节点都有一个父节点(除了顶部的第一个 节点的一个属性是深度，节点的深度取决于它的祖先节点的数量。比如，节点3有3个祖先节 9 点(5、7和11)，它的深度为3。 树的高度取决于所有节点深度的最大值。一棵树也可以被分解成层级。根节点在第0层，它 的子节点在第1层，以此类推。上图中的树的高度为3(最大高度已在图中表示——第3层)。 现在我们知道了与树相关的一些最重要的概念，下面来学习更多有关树的知识。 8.3 二叉树和二叉搜索树 二叉树中的节点最多只能有两个子节点:一个是左侧子节点，另一个是右侧子节点。这些定 节点)以及零个或多个子节点: ![](http://116.85.35.63/wp-content/uploads/2019/03/QQ20190313-183758.png) 位于树顶部的节点叫作根节点(11)。它没有父节点。树中的每个元素都叫作节点，节点分 为内部节点和外部节点。至少有一个子节点的节点称为内部节点(7、5、9、15、13和20是内部 节点)。没有子元素的节点称为外部节点或叶节点(3、6、8、10、12、14、18和25是叶节点)。 一个节点可以有祖先和后代。一个节点(除了根节点)的祖先包括父节点、祖父节点、曾祖 父节点等。一个节点的后代包括子节点、孙子节点、曾孙节点等。例如，节点5的祖先有节点7 和节点11，后代有节点3和节点6。 有关树的另一个术语是子树。子树由节点和它的后代构成。例如，节点13、12和14构成了上 图中树的一棵子树。 节点的一个属性是深度，节点的深度取决于它的祖先节点的数量。比如，节点3有3个祖先节 9 点(5、7和11)，它的深度为3。 树的高度取决于所有节点深度的最大值。一棵树也可以被分解成层级。根节点在第0层，它 的子节点在第1层，以此类推。上图中的树的高度为3(最大高度已在图中表示——第3层)。

#### 2.二叉树和二叉搜索树

二叉树中的节点最多只能有两个子节点:一个是左侧子节点，另一个是右侧子节点。这些定义有助于我们写出更高效的向/从树中插入、查找和删除节点的算法。二叉树在计算机科学中的 应用非常广泛。 二叉搜索树(BST)是二叉树的一种，但是它只允许你在左侧节点存储(比父节点)小的值， 在右侧节点存储(比父节点)大(或者等于)的值。上一节的图中就展现了一棵二叉搜索树。

#### 3\. 手动实现一个BST(二叉搜索树)

![](http://116.85.35.63/wp-content/uploads/2019/03/12121212.png) ![](http://116.85.35.63/wp-content/uploads/2019/03/1111.png) ![](http://116.85.35.63/wp-content/uploads/2019/03/22222222.png) ![](http://116.85.35.63/wp-content/uploads/2019/03/3333333-1.png) ![](http://116.85.35.63/wp-content/uploads/2019/03/444444-1.png) ![](http://116.85.35.63/wp-content/uploads/2019/03/5555555.png) ![](http://116.85.35.63/wp-content/uploads/2019/03/6666666.png.png) ![](http://116.85.35.63/wp-content/uploads/2019/03/6666666.png.png) ![](http://116.85.35.63/wp-content/uploads/2019/03/777777.png)

    // BST 二叉搜索树
    class TreeNode {
        constructor(key){
           this.key = key;
           this.left =null;
           this.right = null;
        }
    }
    // 插入节点 辅助函数
    function insertNode(node,newNode){
        if(node.key > newNode.key){
            if(node.left === null){
                node.left = newNode
            }else{
              insertNode(node.left,newNode)
            }
        }else{
            if(node.right === null){
                node.right = newNode
            }else{
                insertNode(node.right,newNode);
            }
        }
    }
    // 中序遍历 辅助函数
    function inOrderTraverseNode(node,callback){
       if(node!== null){
           inOrderTraverseNode(node.left,callback);
           callback(node.key);
           inOrderTraverseNode(node.right,callback);
       }
    }
    // 先序遍历 辅助函数
    function preOrderTraverseNode(node,callback){
       if(node !==null){
           callback(node.key);
           preOrderTraverseNode(node.left,callback);
           preOrderTraverseNode(node.right,callback);
       }
    }
    // 后续遍历 辅助函数
    function postOrderTraverseNode(node,callback){
       if(node !== null){
           postOrderTraverseNode(node.left, callback);
           postOrderTraverseNode(node.right, callback);
           callback(node.key);
       }
    }
    // 最小节点辅助函数
    function minNode(node){
        while(node && node.left !==null){
            node = node.left;
        }
        return node.key;
    }
    // 最大节点辅助函数
    function maxNode(node){
        while(node && node.right !==null){
            node = node.right;
        }
        return node.key;
    }
    // 搜索辅助函数
    function serachNode(node,key){
        if(node === null){
            return false;
        }else{
            if(key < node.key){
                return serachNode(node.left,key);
            }else if(key > node.key){
                return serachNode(node.right,key);
            }else{
                return true;
            }
        }
    }
    // 删除辅助函数
    function removeNode(node, key) {
        if (node === null) {
            return null;
        }
        if (key < node.key) {
            node.left = removeNode(node.left, key);
            return node;
        } else if (key > node.key) {
            node.right = removeNode(node.right, key);
            return node;
        } else { //键等于node.key
            //第一种情况——一个叶节点
            if (node.left === null && node.right === null) {
                node = null;
                return node;
            }
            //第二种情况——一个只有一个子节点的节点 
            if (node.left === null) {
                node = node.right;
                return node;
            } else if (node.right === null) {
                node = node.left;
                return node;
            }
            //第三种情况——一个有两个子节点的节点
            var aux = findMinNode(node.right);
            node.key = aux.key; //
            node.right = removeNode(node.right, aux.key);
            return node;
        }
    };
    // callback 辅助函数
    function printNode(value){
       console.log(value);
    }
    class BinarySearchTree {
        constructor(){
            this.root = null;
        }
        // 插入几点
        insert(key){
            let newNode = new TreeNode(key);
            if(this.root === null){
                this.root = newNode;
            }else{
                insertNode(this.root,newNode)
            }
        }
        // 中序遍历 callback 对节点操作回掉函数
        inOrderTraverse(callback){
            inOrderTraverseNode(this.root, callback);
        }
        //先序遍历
        preOrderTraverse(callback){
            preOrderTraverseNode(this.root,callback);
        }
        // 后序遍历
        postOrderTraverse(callback){
           postOrderTraverseNode(this.root,callback);
        }
        // 最小值
        min(){
            return minNode(this.root);
        }
        // 最大值
        max(){
            return maxNode(this.root);
        }
        // 搜索
        serarch(key){
            return serachNode(this.root,key);
        }
        // 删除节点
        remove(key){
            root = removeNode(root, key);
        }
    }
    
    var tree = new BinarySearchTree();
    tree.insert(11);
    tree.insert(7);
    tree.insert(15);
    tree.insert(5);
    tree.insert(3);
    tree.insert(9);
    tree.insert(8);
    tree.insert(10);
    tree.insert(13);
    tree.insert(12);
    tree.insert(14);
    tree.insert(20);
    tree.insert(18);
    tree.insert(25);
    tree.insert(6);
    console.log(tree);
    console.log('--------------------中序遍历-------------');
    console.log(tree.inOrderTraverse(printNode));
    console.log('--------------------先序遍历-------------');
    console.log(tree.preOrderTraverse(printNode));
    console.log('--------------------后序遍历-------------');
    console.log(tree.postOrderTraverse(printNode));
    console.log('--------------------最小值-------------');
    console.log(tree.min());
    console.log('--------------------最大值-------------');
    console.log(tree.max());
    
    console.log('--------------------搜索-------------');
    console.log(tree.serarch(100));
    <!----------------------中序遍历--------------->
    <!--3-->
    <!--5-->
    <!--6-->
    <!--7-->
    <!--8-->
    <!--9-->
    <!--10-->
    <!--11-->
    <!--12-->
    <!--13-->
    <!--14-->
    <!--15-->
    <!--18-->
    <!--20-->
    <!--25-->
    <!--undefined-->
    <!----------------------先序遍历--------------->
    <!--11-->
    <!--7-->
    <!--5-->
    <!--3-->
    <!--6-->
    <!--9-->
    <!--8-->
    <!--10-->
    <!--15-->
    <!--13-->
    <!--12-->
    <!--14-->
    <!--20-->
    <!--18-->
    <!--25-->
    <!--undefined-->
    <!----------------------后序遍历--------------->
    <!--3-->
    <!--6-->
    <!--5-->
    <!--8-->
    <!--10-->
    <!--9-->
    <!--7-->
    <!--12-->
    <!--14-->
    <!--13-->
    <!--18-->
    <!--25-->
    <!--20-->
    <!--15-->
    <!--11-->
    <!--undefined-->
    <!----------------------最小值--------------->
    <!--3-->
    <!----------------------最大值--------------->
    <!--25-->
    <!----------------------搜索--------------->
    <!--false-->