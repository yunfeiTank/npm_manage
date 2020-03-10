# npm

* 实现自定义包发布到npm;
    1. npm init 初始化
    (```)
        package name：yunfei        //插件名称
        version: (1.0.0)            //版本；修改后的插件，再次上传，需修改版本号
        description:                //插件描述
        entry point:(index.js)      //入口文件（默认index.js）
        test command:               //测试脚本命令
        git repository:             //相应的github仓库地址，方便他人读取源码
        keyword:                    //关键词，便于npm 搜索
        author:yunfei               //作者
        license:(ISC)               //执照
    (```)
    2. npm login    //登录
    3. npm addUser  //分别输入用户名、密码、邮箱
    4. npm publish  //直接发布

* 私有云（npm）搭建
    | 优点：
        1. 便于管理企业内的业务组件或模块；
        2. 私密性；
        3. npm模块的稳定快速和安全；
    
    | 方案：
        1. git + ssh方式；
        2. verdaccio；

    | 实施（方案2）：
        - 安装verdaccio
        ``` npm install -g verdaccio ```
        - 安装完成后 直接运行verdaccio即可
        - 浏览器地址[http://localhost:4873/](http://localhost:4873/)即可查看私有库
        - 同时在配置文件内设置 ``` listen: 0.0.0.0:4873 ```；实现外网访问
        - 配置文件如下：
        (```)
            #
            # This is the default config file. It allows all users to do anything,
            # so don't use it on production systems.
            #
            # Look here for more config file examples:
            # https://github.com/verdaccio/verdaccio/tree/master/conf
            #

            # path to a directory with all packages
            storage: ./storage
            # path to a directory with plugins to include
            plugins: ./plugins

            web:
            title: Verdaccio
            # comment out to disable gravatar support
            # gravatar: false
            # by default packages are ordercer ascendant (asc|desc)
            # sort_packages: asc

            auth:
            htpasswd:
                file: ./htpasswd
                # Maximum amount of users allowed to register, defaults to "+inf".
                # You can set this to -1 to disable registration.
                # max_users: 1000

            # a list of other known repositories we can talk to
            uplinks:
            npmjs:
                url: https://registry.npmjs.org/

            packages:
            '@*/*':
                # scoped packages
                access: $all
                publish: $authenticated
                unpublish: $authenticated
                proxy: npmjs

            '**':
                # allow all users (including non-authenticated users) to read and
                # publish all packages
                #
                # you can specify usernames/groupnames (depending on your auth plugin)
                # and three keywords: "$all", "$anonymous", "$authenticated"
                access: $all

                # allow all known users to publish/publish packages
                # (anyone can register by default, remember?)
                publish: $authenticated
                unpublish: $authenticated

                # if package is not available locally, proxy requests to 'npmjs' registry
                proxy: npmjs

            # You can specify HTTP/1.1 server keep alive timeout in seconds for incoming connections.
            # A value of 0 makes the http server behave similarly to Node.js versions prior to 8.0.0, which did not have a keep-alive timeout.
            # WORKAROUND: Through given configuration you can workaround following issue https://github.com/verdaccio/verdaccio/issues/301. Set to 0 in case 60 is not enough.
            server:
            keepAliveTimeout: 60

            middlewares:
            audit:
                enabled: true
            # 这里开发外网访问
            listen: 0.0.0.0:4873
            # log settings
            logs:
            - { type: stdout, format: pretty, level: http }
            #- {type: file, path: verdaccio.log, level: info}
            #experiments:
            #  # support for npm token command
            #  token: false

        (```)

        - 当前npm服务指向 私有npm 地址
        ``` npm set registry http://localhost:4873 ```
        - 注册用户（用户名 密码 邮箱）
        ``` npm adduser -registry http://localhost:4873 ```
        - 查看用户是否注册
        ``` npm who am i ```
        - 上传私有云
        ``` npm publish --registry=http://localhost:4873 ```

## 附件

    * 包源
    (```)
        npm ---- https://registry.npmjs.org/
        cnpm --- http://r.cnpmjs.org/
        taobao - https://registry.npm.taobao.org/
        nj ----- https://registry.nodejitsu.com/
        rednpm - http://registry.mirror.cqupt.edu.cn/
        npmMirror  https://skimdb.npmjs.com/registry/
        edunpm - http://registry.enpmjs.org/
    (```)