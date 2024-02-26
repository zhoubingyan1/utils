https://www.cnblogs.com/huangxiaotao/p/10305329.html

### 1.添加站点
```
站点添加成功后修改站点配置文件，直接在配置文件顶部增加
upstream nodenuxt {
    server 127.0.0.1:3003; #nuxt项目 监听端口
    keepalive 64;
}
//以下为之前默认的数据，不用管
server {
    listen 80;
    server_name mysite.com;
    location / {
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;  
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Nginx-Proxy true;
        proxy_cache_bypass $http_upgrade;
        proxy_pass http://nodenuxt; #反向代理
    }
}


```

### 2、设置反向代理

```
由于以上配置了 127.0.0.1:3003 
因此反向代理的目标URL也为 http://127.0.0.1:3003
发送域名：mysite.com  //项目的访问域名

```

### 3.本地项目下的package.json里设置, 注意：start 使用了3003端
```
"scripts": {
    "dev": "cross-env NODE_ENV=development nodemon server/index.js --watch server",
    "online": "cross-env NODE_ENV=production nuxt start",
    "build": "nuxt build",
    "start": "PORT=3003 nuxt start",
    "generate": "nuxt generate",
    "lint": "eslint --ext .js,.vue --ignore-path .gitignore .",
    "precommit": "npm run lint"
  },

```

### 4、项目本地完成后， npm run build 打包应用

```
    //打包完成后将以下四个文件上传到服务器空间
    .nuxt     //打包生成的文件夹
    static    //默认静态文件
    nuxt.config.js   //nuxt项目配置文件
    package.json   //配置文件
    package-lock.json

    //也可以将以上文件 重新放一个文件夹 然后 npm install 安装依赖  npm start 运行项目 看本地能不能运行
    

```
### 5、在服务器上部署运行
```
1. 使用命令模式：终端或者其他方式链接上服务器
2. 自己先测试一下服务器上是否安装支node npm,检查一下版本号 
    node -v
    npm -v
2. 进入到该项目的根目录
3. 运行 npm install 安装package里的依赖
4. 安装好3以后，再运行 npm start 就可以运行起来nuxt的服务渲染了

如下：
> my-nuxt@1.0.0 start /www/wwwroot/mysite.com
> PORT=3003 nuxt start

到了这里我们就可以在浏览器上输入 mysite.com 访问了。服务端渲染瞬间出来了。
#### 别得意，这里还没完成呢。
，上面的效果是并不理想的，所以我们需要开启进程守护来稳定常驻后台

```

### 6、开启进程守护
```
pm2 start npm --name "my-nuxt" -- run start  //my-nuxt为我们自定义的项目名称，也是我们自定义的守护进程名称

┌──────────┬────┬─────────┬──────┬───────┬────────┬─────────┬────────┬─────┬───────────┬──────┬──────────┐
│ App name │ id │ version │ mode │ pid   │ status │ restart │ uptime │ cpu │ mem       │ user │ watching │
├──────────┼────┼─────────┼──────┼───────┼────────┼─────────┼────────┼─────┼───────────┼──────┼──────────┤
│ my-nuxt  │ 0  │ 0.33.4  │ fork │ 23278 │ online │ 0       │ 0s     │ 0%  │ 15.6 MB   │ root │ disabled │
└──────────┴────┴─────────┴──────┴───────┴────────┴─────────┴────────┴─────┴───────────┴──────┴──────────┘

出现了以上的图案数据，说明守护进程已经开启了。

```


### 7、nuxt.config.js
```
/* eslint-disable nuxt/no-cjs-in-config */
import webpack from 'webpack'
// import i18nConfig from './locales/index'
const path = require('path')
const resolve = dir => path.join(__dirname, dir)
// 生产环境判断
const isEnvProduction = process.env.NODE_ENV === 'production'

// 加载.env.* 环境变量
// const env = require('./utils/parseEnv')

// console.log(env)

// 自动加载语言文件
// const autoLoadFile = require('./utils/autoLoadFile')
// 加载语言文件目录，取 `./locales/zh-CN` 或者 `./locales/en-US` 或者其他语言文件目录都可以
// const langFiles = autoLoadFile('./locales/zh-CN')

const base = '/'

export default {
  // 修改loading
  loading: {
    color: '#409EFF'
  },
  /*
   ** Nuxt rendering mode
   ** See https://nuxtjs.org/api/configuration-mode
   */
  mode: 'universal',
  /*
   ** Nuxt target
   ** See https://nuxtjs.org/api/configuration-target
   */
  target: 'server',
  /*
   ** Headers of the page
   ** See https://nuxtjs.org/api/configuration-head
   */
  head: {
    title:'监管链管理后台',
    meta: [
      { charset: 'utf-8' },
      {
        hid: 'description',
        name: 'description',
        content: '监管链管理后台'
      },
      {
        hid: 'description',
        name: 'description',
        content: '监管链管理后台'
      }
    ],

    link: [{ rel: 'icon', type: 'image/x-icon', href: base + 'favicon.ico' }]
  },
  /*
   ** Global CSS
   */
  css: [
    'element-ui/lib/theme-chalk/index.css',
    'normalize.css/normalize.css',
    'font-awesome/css/font-awesome.min.css',
    '@/styles/index.less'
  ],
  /*
   ** Plugins to load before mounting the App
   ** https://nuxtjs.org/guide/plugins
   */
  plugins: [
    '@/plugins/element-ui',
    '@/plugins/axios',
    { src: '@/plugins/storage', ssr: false },
    { src: '@/plugins/svgIcon', ssr: false },
    { src: '@/plugins/chart', ssr: false },
    { src: '@/plugins/download', ssr: false },
    { src: '@/plugins/quitOperation', ssr: false },
  ], //
  /*
   ** Auto import components
   ** See https://nuxtjs.org/api/configuration-components
   */
  components: true,
  /*
   ** Nuxt.js dev-modules
   */
  buildModules: [
    // Doc: https://github.com/nuxt-community/eslint-module
    // '@nuxtjs/eslint-module',
    // Doc: https://github.com/nuxt-community/stylelint-module
    // '@nuxtjs/stylelint-module'
  ],
  /*
   ** Nuxt.js modules
   */
  modules: [
    // Doc: https://axios.nuxtjs.org/usage
    '@nuxtjs/axios',

    // [
    //   'nuxt-i18n',
    //   {
    //     ...i18nConfig(langFiles)
    //   }
    // ],
    [
      '@nuxtjs/component-cache',
      {
        max: 10000,
        maxAge: 1000 * 60 * 60
      }
    ]
  ],
  /*
   ** Axios module configuration
   ** See https://axios.nuxtjs.org/options
   */
  axios: {
     // 开启代理 (如果需要判断线上线下环境，可以通过 process.env.NODE_ENV !== 'production' 来判断)
     proxy: true,
     // 给请求 url 加个前缀 /api，如果这项不配置，则需要手动添加到请求链接前面
     // 如果是多个代理的时候，则不需要配置，走手动添加代理前缀
     prefix: '/api',
     // 跨域请求时是否需要使用凭证
     credentials: true
  },
  proxy: {
    '/api': {
      // 目标接口域
      // target: 'http://10.91.255.5:33018/',
      // target:'http://43.143.115.241:10666',//本地
      target:'http://172.18.0.253:10666',// ssr渲染
      // 全局配置是否跨域
      changeOrigin: true,
      pathRewrite: {
        // 单个配置是否跨域
        // changeOrigin: true
        // 把 '/api' 替换成 '/'，具体需要替换为 '' 还是 '/' 看自己习惯
        '^/api': '/api'
      }
    }
  },
  /*
   ** Build configuration
   ** See https://nuxtjs.org/api/configuration-build/
   */
  build: {
    parallel: true,
    transpile: [/^element-ui/],
    extend(config, { isDev }) {
      if (isDev) {
        config.devtool = 'cheap-module-eval-source-map'
      }

      config.module.rules.find(item =>
        item.test.test('.svg')
      ).exclude = resolve('./icons')

      // svg-icon support
      config.module.rules.push({
        test: /\.svg$/,
        loader: 'svg-sprite-loader',
        include: [resolve('./icons')],
        options: {
          symbolId: 'icon-[name]'
        }
      })
    },
    // plugins: [new webpack.DefinePlugin({ 'process.env': env })],
    terser: {
      terserOptions: {
        compress: {
          drop_console: isEnvProduction
        }
      }
    }
  },
  router: {
    base,
    middleware: ['auth']
  },
  server: {
    port: 9090,
    host: 'localhost'
  }
}

```