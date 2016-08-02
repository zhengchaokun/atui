---
order: 0
english: Atui
---

由阿里通信前端团队打造基于VUE的前端组件库

<div class="pic-plus">
  <img width="150" src="https://t.alipayobjects.com/images/rmsweb/T11aVgXc4eXXXXXXXX.svg">
  <span>+</span>
  <img width="160" src="https://t.alipayobjects.com/images/rmsweb/T16xRhXkxbXXXXXXXX.svg">
</div>

<style>
.pic-plus > * {
  display: inline-block!important;
  vertical-align: middle;
}
.pic-plus span {
  font-size: 30px;
  color: #aaa;
  margin: 0 20px;
}
</style>

---

## 特性

- Designed as Ant Design，提炼和服务企业级中后台产品的交互语言和视觉风格。
- [React Component](http://react-component.github.io/badgeboard/) 上精心封装的高质量 UI 库。
- 基于 npm + webpack + babel 的工作流，支持 ES2015。

## 安装

```bash
$ tnpm install @ali/vue-component --save
```

## 示例

```jsx
<v-button>按钮</v-button>

new Vue({
    components: {
        vButton: atui.Button
    }
})
```

引入样式：

```jsx
// css引入 greater-blue | tao-orange | or tmall-red (推荐)
import '@ali/vue-component/dist/greater-blue.css'

// 或less引入
import '@ali/vue-component/style/themes/greater-blue.css'
```

按需加载可通过此写法 `import { Alert } from '@ali/vue-component'`。


## 版本

- 稳定版：[![npm package](http://img.shields.io/npm/v/antd.svg?style=flat-square)](https://www.npmjs.org/package/antd)
- 开发版：[![](https://cnpmjs.org/badge/v/antd.svg?&tag=beta&subject=npm)](https://www.npmjs.org/package/antd)

## 浏览器支持

现代浏览器和 IE9 及以上。


## 链接

- [首页](http://ant.design/)
- [更新日志](/changelog)
- [开发脚手架](https://github.com/ant-design/antd-init/)
- [开发工具文档](http://ant-tool.github.io/)
- [React 组件](http://react-component.github.io/)
- [React 代码规范](https://github.com/react-component/react-component.github.io/blob/master/docs/zh-cn/component-code-style.md)
- [组件设计原则](https://github.com/react-component/react-component.github.io/blob/master/docs/zh-cn/component-design.md)
- [设计规范速查手册](https://os.alipayobjects.com/rmsportal/HTXUgPGkyyxEivE.png)
- [社区贡献脚手架和范例](https://github.com/ant-design/ant-design/issues/129)

## 谁在使用

- 阿里通信

> 如果你的公司和产品使用了 Ant Design，欢迎到 [这里](https://github.com/ant-design/ant-design/issues/477) 留言。

## 如何贡献

