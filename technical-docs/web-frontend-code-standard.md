# 规范

## Vue 2.x + 风格指南
[Vue Style Guide](https://vuejs.org/v2/style-guide/index.html)

### ESLint/Airbnb
[Airbnb JavaScript Style Guide](https://github.com/yuche/javascript)
注意这个规范十分严格,而且项目引入了ESLint, 所以不遵守的话, 没法通过编译, 目前主要引入规则如下:
- tab缩进
- 变量驼峰命名
- 不允许使用`var`, 而是用`let`或者`const`,而且每一条定义变量语句都要使用`const`|| `let`, 也就是说不允许在其他作用域定全局变量.
- 不允许使用`for迭代`,应当用`every, forEach, map, reduce`等遍历函数, 有点类似函数式编程的思想.
- 关键字,操作符周围必须有空格, 大括号前也要有空格.
- 使用字面值 ( `[], {}, "" `等) 构造变量, 而不是`new`.
- 字符串用单引号而不是双引号,模板字符串除外.
- 匿名函数用ES6的箭头函数 (注意该函数内 this 引用的对象)
- 不允许使用非严格意义的等于不等于,也就是说只能 `=== ` OR `!==`
- 每一条语句必须以 `;` 结尾
- 不允许在一个文件用多个语句引用某个文件的不同对象, 应当在一条语句内解决.
- 不允许函数修改传入参数对象

## 注释规范
** 文件头部注释 **

```text
/**
 *
 * @description 文件用途描述
 * @author your-name <your-email@example.com>
 * @created created date
 * @updated updated date
 *
 */
```
** 函数注释 **

```text
/**
 *
 * 函数描述
 * @author your-name <your-email@example.com>
 * @params {参数类型} 参数名 描述
 * @params ...
 * @return {返回类型} 描述
 *
 */
```

** 行内注释 **

```text
// your comment ...
```
## 命名规范
* 文件名: `(kebab-case)作用.性质.扩展名`
```text
profile-setting.component.ts
profile-setting.component.pug
profile-setting.component.styl
```
* 文件夹名: `kebab-case`
```text
...\profile-setting\...
...\scroll-top\...

```
* 组件名: `PascalCase`, 若在模板HTML内则`kebab-case`
```text
xxx.ts
Vue.component('MyComponent', {
  // ...
})

xxx.pug
<my-component></my-component>
```
* 普通变量名: `camelCase`
