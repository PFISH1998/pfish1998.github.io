<h1 align="center">Material-T</h1>

<p align="center">一款Material Design风格的主题，灵感来源于<a href="https://github.com/creativetimofficial/material-kit">Material-Kit</a></p>

![screenshot-index](https://github.com/invom/Material-T/raw/master/screenshot-index.png)


## 在线预览 Live Preview

[invom's blog](https://invom.cc)

## 安装 Installation

**1.** 获取最新版本

```bash
cd themes
git clone -b master https://github.com/invom/Material-T.git Material-T
  ```

**2.** 修改`/_config.yml`

```yml
// 关闭归档页的默认分页：
archive_generator:
  per_page: 0  
  yearly: true
  monthly: true
  daily: false
  order_by: -date

// 关闭默认代码高亮
highlight:
  enable: false
  line_number: false
  auto_detect: false
  tab_replace:
```

**3.** 创建About Page

```bash
hexo new page about
```

修改`/source/about/index.md`，添加属性`type: "about"`

**4.** 在`/_config.yml`中启用`Material-T`

## 更新

`git pull`


## [配置 Configuration](https://github.com/invom/Material-T/wiki)

## [TODO](https://github.com/invom/Material-T/projects)


## 贡献 Contributing

项目会持续更新，期待您的Pull Request


## License

MIT
