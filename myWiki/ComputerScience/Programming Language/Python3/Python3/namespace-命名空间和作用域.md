# namespace(命名空间)

> 无声明的情况下，赋值即私有，若外部有相同名称的变量则被遮挡
> 想修改外部变量，需声明（global、nonlocal），或者通过可变对象的内置函数

## 3 种命名空间

- build-in namespace
- global namespace
- local namespace

1. 命名空间的生命周期
命名空间的生命周期取决于对象的作用域，对象执行完成，那么该命名空间的作用域就消失了
