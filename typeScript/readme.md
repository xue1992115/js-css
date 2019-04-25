# typescript能够文件通过编译后，可以生成一个js文件。javascript是一个弱类型的语言，typeScript是一个强类型的语言。主要是提供了类型的校验以及支持es6。
# ts文件的优点：
- 提供参数的类型注释， 对于提供错误的参数类型会进行提示，但还会生成js文件
- interface: 用于描述一个对象，以及对象中需要的字段和字段类型。
- classes: typeScript中的类只是基于原型的OO简写。
# basic types
- boolean
- number
- string
- array
- enum
- any: 变量的类型，可以是任意的
- tuple
- void
- Null and undefined
- never
- Object
# 变量声明
- var
- let
- const
# interface： 使用接口定义对象的类型
- 属性类型的定义 label: string;
- 可选属性的类型定义：color?: string; 
- 只读属性的定义：readonly x: number;
- readonly vs const 一个简单的方法就是：使用在变量（const），还是一个属性（property）。
# function
- 函数声明的方式： 函数，函数表达式，箭头函数
- 定义输入，输出的类型

