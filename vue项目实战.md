# Vue3 项目

## 安装脚手架

### Vue CLI

```
npm insatll -g @vue/cli
```

## 创建项目

```
vue create vue3-demo
```

> 使用vite创建vue项目

```
npm init vite-app vue3-demo-vite
cd vue3-demo-vite
npm install
```

## 在IDE中打开

这里使用VS Code

```
cd vue3-demo
code .
```

### Vue3与Vue2的区别

- 在 Vue3 template 中多个 html 元素不需要一个根元素包裹
- Vue3 添加了 Compostion API，created 阶段做的事都可以直接放在 setup 中，beforeDestory 和 destoryed



## Tips

##### 动态引入静态资源

需要使用require路径，不然无法正常显示

```
require('../assets/static/images/')
```

##### vs code 自定义用户代码代码片段

文件 > 首选项 > 用户片段 > `vue.json`

```json
{
	"Print to console": {
		"prefix": "vv3",
		"body": [
			"<template>",
			"\t<div>",
			"\t\t$1",
			"\t</div>",
			"</template>",
			"",
			"<script lang=\"ts\">",
			"import { defineComponent } from 'vue'",
			"",
			"export default defineComponent({",
			"\tname: '$TM_FILENAME_BASE',",
			"\tsetup() {",
			"\t\treturn {",
			"",
			"\t\t}",
			"\t},",
			"})",
			"</script>",
			"",
			"<style scoped>",
			"",
			"</style>"
		],
		"description": "vue3 component template"
	},
}
```

##### JavaScript 中的调试

1. debugger，断点调试
2. console.log()，控制台打印
3. alert()，弹出警告框

