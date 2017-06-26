# 算法
## 树
### 交换二叉树的左右节点（包括所有子节点的左右节点）[`code`](code/algorithm/algorithmTreeSwitch.html)
```javascript
switch() {
    if(null != this.left || null != this.right){
        let temp = this.right;
        this.right = this.left;
        this.left = temp;
    }
    if (null != this.left) {
        this.left.switch();
    }
    if (null != this.right) {
        this.right.switch();
    }
}
```