## webpack
是一个模块打包器，构建工具会将前端所有的资源文件都作为模块处理，它将根据模块的依赖关系进行分析，生成对应的资源
### 五个核心概念
1、entry：webpack应该使用哪个模块，来作为构建内部依赖图的开始
2、output：在哪里输出文件，以及如何命名这些文件
3、loader：处理那些非JavaScript文件
4、plugins：执行范围更广的任务，从打包到优化都可以实现
5、mode：有生产模式production和开发模式development

webpack可以打包js和json文件，生产模式和开发模式的区别，开发模式没有对文件进行压缩
```
webpack 输入 -o 输出 --mode=development
webpack 输入 -o 输出 --mode=production
```

### 编写一个配置文件
