依照官方说明安装好Node.js后在命令行下全局安装pnpm,@slidev/cli@0.50.0

并将@slidev/cli@0.50.0添加到环境变量(否则无法在命令行运行slidev命令)

```
>npm install -g pnpm
>npm install -g @slidev/cli@0.50.0
```

此后进入项目目录theme-default下验证pnpm和@slidev/cli@0.50.0是否安装成功

```
>pnpm -v
>slidev -v
```

执行以下命令安装theme-default主题需要的依赖文件

```
>pnpm install
```

测试slidev是否能正常运行

```
>slidev --open slide.md
```
