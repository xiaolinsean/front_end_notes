
<!-- 
参考：https://yangjh.oschina.io/gitbook/

 -->
 
- [1、执行 gitbook -V 或 gitbook init 命令 卡顿](#1执行-gitbook--v-或-gitbook-init-命令-卡顿)
- [2、Template render error 报错](#2template-render-error-报错)
- [3、引入图片](#3引入图片)



## 1、执行 gitbook -V 或 gitbook init 命令 卡顿

执行 gitbook -V 或 gitbook init 命令，均会显示： Installing GitBook 3.2.3 ……. 等待较长时间都没有成功，这时检查一下 npm 地址：
```bash
npm config list
```

如果默认的还是国外镜像的话，可以换成淘宝镜像，再重新试一下
```bash
npm config set registry=http://registry.npm.taobao.org -g
```

然后重新再试，如果还是不行，尝试安装其他 gitbook 版本试试：
```bash
gitbook uninstall 3.2.3 #卸载
gitbook fetch 3.0.0 #安装
```

注意：gitbook安装时可能会比较慢，耐心多等待会。

## 2、Template render error 报错

我们在执行 `gitbook serve` 时，可能会出现如果编译错误：
```bash
Template render error: (xxx.md) [Line 244, Column 43] expected variable end
```

原因是 gitbook 对于 md 文件中有连续的大括号，例如 `{{path}}`, 会认为大括号内的 `path` 当做是一个模板变量，所以才会报错

解决方法：可以在 md 文件的开头和结尾加上如下内容，gitbook 编译时就会忽略模板变量

```md
{% raw %}
  
    正文内容...
 
{% endraw %}
```

## 3、引入图片

- 公网绝对地址
  ```md
  ![logo](http://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Markdown-mark.svg/208px-Markdown-mark.svg.png)
  ```

- 项目内相对地址
  ```md
  ![png](../static/imgs/markdown.png)
  ```

