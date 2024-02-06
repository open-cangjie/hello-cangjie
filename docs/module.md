# module 模块

模块配置文件，这里指项目的modules.json 文件，用于描述代码项目的基础元属性。

```json
{
  "name": "file name", //当前项目的名称
  "description": "项目描述", //项目描述
  "version": "1.0.0", //当前模块版本号
  "output_type": "static", //模块编译输出类型,static 指编译输出为静态库
  "cjc_version": "0.62.2", //仓颉编译器版本
  "command_option": "-Woff unused", //编译选项
  "organization": "org name", //组织名称
  "requires": { //需要和依赖的仓颉代码模块，这个功能类似于Go，直接将代码合并入当前工程来编译
    "bits": {
      "branch": "master",
      "version": "0.1.0",
      "git": "https://gitee.com/HW-PLLab/bits.git"
    },
    "charset": {
      "branch": "develop",
      "version": "0.0.2",
      "git": "https://gitee.com/HW-PLLab/charset.git"
    }
  },
  "package_requires": { //其他非代码类包依赖
    "path_option": [], //文件路径选项
    "package_option": {} //包选项
  },
  "foreign_requires": {}, //跨语言模块依赖
}
```