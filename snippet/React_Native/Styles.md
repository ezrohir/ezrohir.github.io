### Using Styles
所有的核心组件都有 `style` 属性
```
<Text style={styles.base} />
<Viwe style={styles.background} />
```
同时接受数组
```
<View style={[styles.base, styles.backgound]}>
```
根据不同条件给定 style
```
<View style={[styles.base,this.state.active && styles.active]}>
```

### Pass Styles Around
