## intro

固体建模的表示方式

**CSG**: constructive solid geometry 物体由多个基本几何体的bool操作implicit构成，只存储构成树，叶子结点表示基本几何体

**B-rep**: boudary representation 显示的表示边界面

cgal中：

数据结构：nef polygon

bool操作：boolean set operations