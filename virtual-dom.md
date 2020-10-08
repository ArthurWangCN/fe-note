## 虚拟DOM是什么
### 虚拟DOM优点
+ 减少DOM操作（优化的不是dom操作，而是dom操作的次数）
  1. 添加1000个节点，虚拟DOM将多次操作合并为一次操作
  2. 可以把多余的操作省去，例如添加1000个节点，可能只有10个是新增的
+ 跨平台：虚拟DOM本质上是一个JS对象

### 虚拟DOM缺点
需要额外的创建函数，如createElement或h，但是可以通过JSX来简化（但是要添加额外的构建过程）

### 创建虚拟DOM
+ React.createElement
+ Vue render函数
+ React JSX：通过babel转为createElement形式
+ Vue Template：通过vue-loader转为h形式



