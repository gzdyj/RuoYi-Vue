# AGENTS.md — RuoYi-Vue v3.9.2

## 项目概览
若依管理系统，Spring Boot 4.0.6 (JDK 17) + Vue 2.6 (Element UI) 前后端分离。

## 模块结构
| 模块 | 职责 |
|------|------|
| ruoyi-admin | 启动入口，端口 9000 |
| ruoyi-framework | 安全框架、拦截器 |
| ruoyi-system | 业务逻辑 + MyBatis Mapper |
| ruoyi-common | 公共工具类、基类 |
| ruoyi-quartz | 定时任务 |
| ruoyi-generator | 代码生成器（Velocity） |
| ruoyi-ui/ | Vue 2 前端（Vue CLI） |

## 构建/运行命令
```bash
# 后端（无 Maven Wrapper，直接用 mvn）
mvn clean package -Dmaven.test.skip=true      # 打包（测试总是跳过！）
mvn spring-boot:run -pl ruoyi-admin           # 开发运行
java -jar ruoyi-admin/target/ruoyi-admin.jar  # 运行 jar

# 前端（在 ruoyi-ui/ 下）
npm run dev            # 开发服务器（http://localhost:80）
npm run build:prod     # 生产构建（产出 gz 压缩文件在 dist/）
npm run build:stage    # 预发布构建

# 便捷脚本（根目录）
bin/package.bat        # 等同于 mvn clean package -Dmaven.test.skip=true
bin/run.bat            # 运行 ruoyi-admin.jar
```

## 环境依赖
- JDK 17+
- MySQL 8（数据库 ry-vue，默认 root/root）— 首次运行前导入 sql/ 下的 SQL
- Redis（localhost:6379，无密码）— **启动必须，否则报错**
- Node.js >= 8.9（前端）

## 关键配置
- 主配置：`ruoyi-admin/src/main/resources/application.yml`（端口 9000）
- 数据源：`application-druid.yml`（`spring.profiles.active: druid` 自动激活）
- Token 密钥：`application.yml` 中 `token.secret`
- API 文档：Springdoc Swagger UI → `/swagger-ui.html`
- 文件上传路径：`D:/ruoyi/uploadPath`（Windows），Linux 需改为 `/home/ruoyi/uploadPath`
- 代码生成器配置：`ruoyi-generator/src/main/resources/generator.yml`

## 前端架构要点
- Vue 2 + Vue Router 3 + Vuex 3 + Element UI 2.15
- 开发代理：`/dev-api` → `http://localhost:9000`（路径重写为空）
- 动态路由：`permission.js` 先 `GetInfo` 再 `GenerateRoutes`，使用 **已弃用的 `router.addRoutes()`（勿改为 `addRoute()`）**
- 全局组件在 `main.js` 注册：Pagination、RightToolbar 等
- 全局工具方法挂在 `Vue.prototype`：`parseTime`、`handleTree`、`download` 等
- 请求封装 `utils/request.js`：Axios 拦截器处理 Bearer Token、401 过期弹窗、防重复提交
- 编辑器缩进：2 空格，LF 行尾
- `babel-plugin-dynamic-import-node` 仅 dev 环境启用（提升 HMR 速度）
- 无 ESLint / Prettier / 测试脚本，前端没有 lint 和 test 命令
- SVG 图标：`svg-sprite-loader`，图标目录 `src/assets/icons`

## 后端架构要点
- MyBatis + PageHelper 分页（`helperDialect: mysql`）
- JWT 鉴权 + Spring Security
- API 响应格式：`{ code: 200, data: ..., msg: "..." }`，code=401 触发重新登录
- Token 头：`Authorization: Bearer <value>`

## 开发流程
1. 启动 MySQL + Redis
2. 导入 `sql/` 下的 SQL 初始化
3. 后端：`mvn spring-boot:run -pl ruoyi-admin`（或 IDE 运行 `RuoYiApplication`）
4. 前端：`cd ruoyi-ui && npm run dev`
5. 访问 `http://localhost`，登录 `admin/admin123`
