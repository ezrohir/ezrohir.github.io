>  if you have code that updates existing constraints (such as adjusting the constant property on some constraints), place this in updateConstraints but outside of the check for didSetupConstraints so it can run every time the method is called.

[出处](http://stackoverflow.com/questions/18746929/using-auto-layout-in-uitableview-for-dynamic-cell-layouts-variable-row-heights)


### translatesAutoresizingMaskIntoConstraints

### 关于移除布局约束的坑

### 字段NSLayoutAttributeLeading,NSLayoutAttributeTrailing
NSLayoutAttributeLeft/NSLayoutAttributeRight 和 NSLayoutAttributeLeading/NSLayoutAttributeTrailing的区别是left/right永远是指左右,而leading/trailing在某些从右至左习惯的地区会变成,leading是右边,trailing是左边.

### Content Compression Resistance 和 Content Hugging
