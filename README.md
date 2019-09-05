# lyj-pack
webpack 手写版

### webpack简单版实现的功能
- 获取配置，从入口开始分析结构
- 执行并分析模块的依赖关系，生成 modules 和 entry
>> babylon 生成 ast，babel/traverse 替换 require 方法为 __webpack_require__ (polyfill 不同平台)
- 渲染通用模板，生成打包后的 js 文件

``` javascript
(function(modules) { // webpackBootstrap   webpack入口函数
	var installedModules = {};
	function __webpack_require__(moduleId) {
		if(installedModules[moduleId]) {
			return installedModules[moduleId].exports;
    }
    var module = installedModules[moduleId] = {
			i: moduleId,
			l: false,
			exports: {}
		};
		modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);
		return module.exports;
	}
return __webpack_require__(__webpack_require__.s = "<%-entryId%>");//入口模块
})

({
  <%for(let key in modules) {%>
    "<%-key%>":   //key->模块的路径
    (function(module, exports,__webpack_require__) {  
      eval(`<%-modules[key]%>`);
    }),
  <%}%>

});
```