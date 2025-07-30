# 项目CICD流程

## 前端

* 前端repo直连vercel，推送后自动构建

## 后端

* 后端服务主要有springcloud微服务9个

  * gateway
  * auth

  * team

  * im

  * post

  * checkin

  * feed

  * notification

  * user

* 中间件服务
  * mysql
  * redis
  * kafka
  * nacos
  * zookeeper

* 使用windows及其的wsl进行部署并使用cloudflare tunnel进行内网穿透对外暴露服务。

* tunnel在部署中的使用主要涉及两个维度

  * 一是开发机器连接部署机器
    * 部署机器window安装配置cloudflare tunnel并在cf面板上配置
  * 二是部署机器对外暴露端口

* 配置docker file和 compose文件

* 配置 env文件

* 在服务器登陆docker

  * ```
    dckr_pat_PZm9lYrKnes4K837kYy8HeUlgPg
    ```

* 配置action文件





vasp_eMBogirQpWsJsUN5cKjLdQNUHB93LkpRe6N