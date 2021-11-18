参考文章：
http://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html
https://segmentfault.com/a/1190000021815477


# 一个 workflow，名字为 Github Action Example
name: GitHub Actions Build and Deploy Demo

# 触发 workflow 的事件
on:
  push:
    # 分支
    branches:
      - main

# 一个workflow由执行的一项或多项job
```
jobs:

  build-and-deploy:

    # runs-on 字段指定运行所需要的虚拟机环境
    # ubuntu-latest，ubuntu-18.04 或 ubuntu-16.04
    # windows-latest，windows-2019 或 windows-2016
    # macOS-latest 或 macOS-10.14
    runs-on: ubuntu-latest
    # steps字段指定每个 Job 的运行步骤，可以包含一个或多个步骤。每个步骤都可以指定以下三个字段。
    # jobs.<job_id>.steps.name：步骤名称。
    # jobs.<job_id>.steps.run：该步骤运行的命令或者 action。
    # jobs.<job_id>.steps.env：该步骤所需的环境变量。
    
    steps:
      - name: Checkout
        uses: actions/checkout@v2 # If you're using actions/checkout@v2 you must set persist-credentials to false in most cases for the deployment to work correctly.
        with:
          persist-credentials: false
      - name: Node version
        run: node -v
      # 生成静态文件
      - name: Install and Build
        run: |
          npm install
          npm run-script build
      # 部署到 GitHub Pages
      - name: Deploy
        uses: JamesIves/github-pages-deploy-action@releases/v3
        with:
          ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }}
          BRANCH: gh-pages
          FOLDER: build
          
```
