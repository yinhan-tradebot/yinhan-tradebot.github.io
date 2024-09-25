### 如何使用 Markdown 编辑数理化公式等

- 数理化公式
  - 行内公式 `$ y = f(x) $` 生成 $ y = f(x) $
  - 居中公式

```md
$$
\vec{f} = m \vec{a} \\
2 H_2 + O_2 \longrightarrow 2 H_2 O
$$
```

生成:

$$
\vec{f} = m \vec{a} \\
2 H_2 + O_2 \longrightarrow 2 H_2 O
$$

您可以通过学习 [LaTeX][1] 来编辑更复杂的公式。

---

- 程序代码

<pre><code class="hljs language-md">```py
def func():
  print("hello world")
```
</code></pre>

生成:

```py
def func():
  print("hello world")
```

[1]: https://m.bilibili.com/video/BV11h41127FD

---

- 基础 Markdown 语法示例

```md
## 2级标题

### 3级标题

**粗体** 

*斜体*

> 引用

1. 选项1
2. 选项2

- 选项1
  - 选项1.1

`行内代码`

分割线

---

[链接][2]

![图片][3]

[2]: https://www.mirror-neuron.tech
[3]: https://www.mirror-neuron.tech/assets/icons/neuron.png
```

生成:

## 2级标题

### 3级标题

**粗体** 

*斜体*

> 引用

1. 选项1
2. 选项2

- 选项1
  - 选项1.1

`行内代码`

分割线

---

[链接][2]

![图片][3]

[2]: https://www.mirror-neuron.tech
[3]: https://www.mirror-neuron.tech/assets/icons/neuron.png

