## Babel 转译

由于框架依赖的 Node 最低版本为 `6.0.0`，但这个版本还不支持 `async/await`，所以在项目里使用 `async/await` 时，需要借助 babel 转译。

Babel 会将 `src/` 目录转译到 `app/` 目录下，并添加 `sourceMap` 文件。

### 关闭 Babel 转译

如果想关闭 Babel 转译，那么需要 Node 的版本大于 `7.6.0`（推荐使用 8.x.x LTS 版本），创建项目时可以指定 `-w` 参数来关闭 Babel 转译。

```sh
thinkjs new demo -w;
```

其实使不使用 Babel 转译，其实只是入口文件里引用有一些区别。

* 有 Babel 转译的入口文件（development.js）

    ```js
    const Application = require('thinkjs');
    const babel = require('think-babel');
    const watcher = require('think-watcher');
    const notifier = require('node-notifier');

    const instance = new Application({
      ROOT_PATH: __dirname,
      watcher: watcher,
      transpiler: [babel, {  //转译器，这里使用的是 babel，并指定转译参数
        presets: ['think-node']
      }],
      notifier: notifier.notify.bind(notifier), //通知器，当转译报错时如何通知
      env: 'development'
    });

    instance.run();
    ```

* 去除 Babel 转译的入口文件（development.js）

    ```js
    const Application = require('thinkjs');
    const watcher = require('think-watcher');
    const instance = new Application({
      ROOT_PATH: __dirname,
      watcher: watcher,
      env: 'development'
    });

    instance.run();
    ```

对比可以看到，去除 Babel 转译，只是移除了 `transpiler` 和 `notifier` 2 个配置，一个是指定转译器，一个是当转译报错时的通知处理方式。

当然除了指定 `-w` 参数关闭 Babel 转译，手工删除 `development.js` 里的相关代码也是可以的。