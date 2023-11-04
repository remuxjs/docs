编译器
============

remux使用 `babel插件 <https://github.com/remuxjs/babel-plugin>`_ 来实现代码转译，这个插件可以接收3个参数

* role 表示当前需要转译到的执行端
* id 是一个索引，用来重定向文件，如果不填写则是文件绝对路径
* lib 导入 ``_remuxInvoke`` 的库，默认为 ``@remux/lib``

如果你想直接尝试remux转译代码，可以在 `playground <../_static/playground>`_ 中进行测试。
