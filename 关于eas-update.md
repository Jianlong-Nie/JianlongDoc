



首先解释一下为什么你在运行 `eas update --auto` 之后app没有发生更新，请看图:![image.png](https://s2.loli.net/2024/12/20/Ue8YBPr4I5thWFd.png)

因为你是在git上的`staging`分支运行了`eas update --auto` ，`--auto`会默认指定`branch`为当前git的分支为`staging`，然后我们目前并没有相关的`channel`和`staging`分支相关联，![image.png](https://note.youdao.com/yws/res/8/WEBRESOURCEf2349c2d02a5e25d2cdc7092da438928)

目前默认的对应testflight的相关branch是testflight,也就是我们在eas.json里面指定channel之后，eas会自动生成相关的branch与之对应。所以当你运行`eas update --auto`相当于运行了`eas update --branch staging`是一样的，而我们的staging channel其实只是一个基础的channel，testflight和internalgoogle 继承了它而已，打包的时候我们运行的是`eas build --platform ios --auto-submit --non-interactive --no-wait --profile testflight`

![image.png](https://note.youdao.com/yws/res/3/WEBRESOURCE356fdcd2cc4d9b4087bf956d2ec3eea3)

当然，如果你觉得这个关联关系不合理，可以进行更改，branch和channel之间的关系，`eas channel:edit` 如视频：



然后，我觉得目前关于eas update相关的几个注意点，

1.请一定要注意，eas update只能更新纯js相关的更改，比如app.config.ts做了修改，那么请不要用eas update

2.如果package.json 做了修改，请不要使用eas update

3.如果native相关的配置做了修改，请不要用eas update

4.如果有任何native代码做了修改或相关的库做了修改，请不要用eas update

5.以上任何相关文件做了修改之后，建议我们需要versions.json里面的appversion（因为我们的eas update更新策略是根据appversion进行的，也就是说，其实我们运行eas update的时候，它只会更新和当前app.config.ts里面的appVersion相对应的）





