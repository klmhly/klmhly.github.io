---
title: HTML 篇
date: 2018-12-07 12:20:56
categories: 
- WEB
tags: 
- HTML
---
> HTML 基础
<!-- more --> 

## 一、概要
### 1. HTML 历史
- Internet
- World Wide Web(WWW):
HTML、URI/URL、HTTP

### 2. HTML 文档
- HTML 是一种标记语言

### 3. HTML 结构
- DOCTYPE - 文档申明
- html - 根元素
- head - 文档元信息（用于浏览器和搜素引擎）
- body - 文档数据（在浏览器中显示的信息）

### 4. <head\> - 元信息
- <title\> - 文档的标题
- <meta\> - 元信息
- <script\> - 脚本
- <style\> - 样式
- <link\> - 引用文档
- <base\> - 定义所有相关路径的基地址

### 5. <body\> - 数据
- Headings(标题)
- Text(文本)
- Lists(列表)
- Links(链接)
- Tables(表格)
- Images/Objects(图片/对象)

---

## 二、文本元素
### 1. 标题
- <h1\> ~ <h6\> 六级标题

### 2. 块元素和行内元素
- 块元素自动换行
- 行内元素不换行

### 3. 文本换行和空格
- HTML 忽略换行和超过一个的空格
- <pre\> - 保留所有空格
- <br\> - 换行
- <hr\> - 水平线
- 字符实体

### 4. 文档元素
- <sup\><sub\> - 上标和下标
- <cite\> - 引用
- <abbr\><acronym\> - 缩写
- <em\><strong\> - 加粗强调
- <code\><samp\> - 代码块和程序输出
- <kbd\><var\> - 键盘输入和代码变量
- <blockquote\><q\> - 块级引用和行内引用

---

## 三、列表
### 1. 无序列表
- <ul\><li\> 符号标识

## 2. 有序列表
- <ol\><li\> - 数字标识或字母标识

## 3. 自定义列表

---

## 四、链接和锚点
### 1. 锚点
- 资源锚点
```html
<a href="">资源锚点</a>
```
- 名字锚点
```html
<a name="">名字锚点</a>
```
### 2. 链接文档
- 链接可以使用绝对地址和相对地址
- 链接内容：文本，图片等

### 3. 文档内链接
- 通过 name 链接
- 通过 id 链接

---

## 五、表格
### 1. 表格结构
- Caption - 表格标题
- Header - 列标题
- Body - 表格数据
- Footer - 表格摘要，总结等
```html
<table>
    <caption></caption>
    <thead>
        <tr>
            <th></th>
            <th></th>
        </tr>
    </thead>
    <tfoot>
        <tr>
            <td></td>
        </tr>
    </tfoot>
    <tbody>
        <tr>
            <td></td>
            <td></td>
        </tr>
    </tbody>
</table>
```
### 2. 行和列 - 结构数据
- <tr\> - 表格行：可用于 header, body 和 footer
- <th\> - 表格标题数据：header 单元格数据
- <td> - 表格数据：body 和 footer 单元格数据
- colspan/rowspan - 跨多列/跨多行 

### 3. 格式化表格
- border - 表格边框
- cellpadding - 单元格内边距
- cellspacing - 单元格外间距

---

## 六、表单
### 1. input - 文本输入域
- 单行文本域: `<input type="text" >`
- E-mail 地址域: `<input type="email">`
- 密码域: `<input type="password">`
- 搜索域: `<input type="search">`
- 电话号码域: `<input type="tel">`
- URL域: `<input type="url">`

### 2. textarea - 多行文本域
- 多行文本域: `<textarea cols="30" rows="10"></textarea>`

### 3. select - 选择框
- 一个选择框
```
<select id="simple" name="simple">
  <option>Banana</option>
  <option>Cherry</option>
  <option>Lemon</option>
</select>
```
- 多选选择框
```
<select multiple id="multi" name="multi">
  <option>Banana</option>
  <option>Cherry</option>
  <option>Lemon</option>
</select>
```

### 4. datalist - 自动补全输入框
```
<input name="myfruit" type="text" list="fruit"> 
<datalist id = "fruit">
   <option value="">apple</option>
    <option value="">banana</option>
    <option value="">orange</option>
    <option value="">pear</option>
</datalist>

```

### 5. checked - 选中项
- 复选框：`<input type="checkbox" checked value="apple">`
- 单选按钮：`<input type="radio" checked name="1" value="meal"`
同一组的选项name相同

### 6. 一个完整例子
```
<form action="/my-handling-form-page" method="post">
    <div>
        <label for="name">Name:</label>
        <input type="text" id="name" />
    </div>
    <div>
        <label for="mail">E-mail:</label>
        <input type="email" id="mail" />
    </div>
    <div>
        <label for="msg">Message:</label>
        <textarea id="msg"></textarea>
    </div>
</form>
```
## 七、图片和对象
### 1. 图片
- <img\> - 插入图片

### 2. 对象
- <object\> - 插入插件