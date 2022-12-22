# PyQuery 教程

## 常用语法

- 选择 `table` 节点

```python
doc.find('table')
```

- 选择直接父亲节点

```python
doc.find('table').parent('div')
```

- 根据索引选择节点

```python
caption = caption.filter(lambda i: i in tb_index)
```

- 移除所有 `div` 子节点

```python
caption = PyQuery(copy.deepcopy(table.copy()))
caption.find('div').remove()
```

其中，`table` 和 `caption` 的类型均为 PyQuery

## 伪类选择器

- 选择最后一个子节点

```python
doc("div:last-child")
```
