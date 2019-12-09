# VSCode使用总结：

## json配置文件解释
- launch.json，项目调试配置文档，包括调试器路径,具体的调试程序名称  
- setting.json,包括当前项目的workspace配置，例如文档大小颜色啊；还包括全局用户（User）的配置，两个目录不一样，第一个位于当前文件夹，第二个位于全局配置路径$HOME/.config/vscode/setting.json。  
- tasks.json，项目整体配置文档，包括编译器路径。  
- C++还包括c_cpp_properties.json，项目头文件路径设置文档，这个是cpptools扩展程序的配置文件  
- Ctrl+Shift+P, which brings up the Command Palette,弹出命令面板。
- 打开测试py文件，按快捷键Ctrl+Shift+B运行(Ctrl+Alt+N),这个快捷键依赖的tasks.json配置文件，这里面可以配置多个task启动项
- 如果没装Coderunner插件，那么运行程序结果无法在vscode的OUTPUT显示，而是在新开的terminal显示，装了coderunner则可以在OUTPUT的子窗口code中显示程序运行结果以及程序运行时间，而且运行命令是(Ctrl+Alt+N)所以coderunner这个插件需要进一步研究,需要在setting.json中配置code-runner.executorMap属性
- VSCode 每个版本更新后的详细介绍https://code.visualstudio.com/updates/v1_25 v1_25则是版本号；

## 插件的一些使用方法

### markdownlint使用方法  

首先markdownlint插件是进行markdown语法静态检测，但是这些语法可能并不符合原生markdown语法；具体的语法检测可以通过.markdownlint.json配置文件进行配置;具体语法规则在这个[网站](https://github.com/DavidAnson/markdownlint/blob/v0.6.0/README.md),目前为止有MD44个类型检测；.markdowlint.json配置规则可以通过markdownlint插件详细信息参考。暂时先禁止markdownlint插件，先学习原生markdown语法，后面再启用这个插件。