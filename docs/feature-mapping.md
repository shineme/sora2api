# Sora2API 项目功能映射文档

## 📋 文档说明

本文档详细记录了 Sora2API 项目的所有功能点及其对应的代码位置，帮助开发者快速定位和修改项目功能。

## 🏗️ 项目概览

### 技术栈信息
- **后端框架**: FastAPI 0.119.0 + Python 3.8+
- **前端技术**: 原生HTML + TailwindCSS + Vanilla JavaScript
- **数据库**: SQLite + aiosqlite 0.20.0
- **认证方式**: JWT + bcrypt 4.2.1
- **配置文件**: TOML格式
- **容器化**: Docker + Docker Compose

### 项目架构
```
sora2api-main/
├── config/                   # 配置文件
│   └── setting.toml         # 主配置文件
├── src/                     # 源代码
│   ├── api/                 # API接口层
│   │   ├── routes.py       # OpenAI兼容接口 (168行)
│   │   └── admin.py        # 管理后台接口 (952行)
│   ├── core/               # 核心业务层
│   │   ├── config.py       # 配置管理
│   │   ├── database.py     # 数据库操作 (701行)
│   │   ├── models.py       # 数据模型
│   │   ├── auth.py         # 认证授权
│   │   └── logger.py       # 日志管理
│   ├── services/           # 业务服务层
│   │   ├── token_manager.py    # Token管理 (947行)
│   │   ├── load_balancer.py    # 负载均衡
│   │   ├── generation_handler.py # 生成处理 (708行)
│   │   ├── sora_client.py      # Sora客户端 (419行)
│   │   ├── proxy_manager.py    # 代理管理
│   │   ├── file_cache.py       # 文件缓存
│   │   └── token_lock.py       # Token锁
│   └── main.py             # 应用入口
├── static/                  # 静态文件
│   ├── login.html          # 登录页面
│   └── manage.html         # 管理控制台
├── requirements.txt        # Python依赖
└── docker-compose.yml      # Docker编排
```

### 项目规模统计
- **总文件数**: 27个
- **Python文件**: 20个 (74%)
- **总代码行数**: 5,079行
- **主要功能模块**: 21个界面功能 + 2个API接口

## 📊 功能映射表

### 🔐 身份认证模块

#### 管理员登录
**功能描述**: 系统管理员登录入口
**用户描述**: 登录页面、管理员登录、系统登录、Login Page、Admin Login
**代码位置**:
- 主文件: `static/login.html` (完整登录页面)
- 样式: 内联TailwindCSS (第12-25行背景样式)
- 逻辑: JavaScript登录验证 (第85-120行login函数)
**视觉特征**: 居中卡片式布局，蓝色渐变背景，包含"Sora2API"标题
**修改指引**:
- 外观修改: 编辑第12-25行的背景样式类
- 行为修改: 编辑第85-120行的login函数
- 文本修改: 编辑第30-50行的表单标签

#### 退出登录
**功能描述**: 管理员退出系统
**用户描述**: 退出按钮、登出、注销、Logout、Sign Out
**代码位置**:
- 主文件: `static/manage.html` (顶部导航栏右侧)
- 逻辑: logout函数
**视觉特征**: 红色背景按钮，位于右上角，文本为"退出"
**修改指引**:
- 样式修改: 编辑class包含"bg-red-500"的按钮元素
- 行为修改: 编辑logout函数

### 🗂️ 面板导航模块

#### 标签页切换
**功能描述**: 主导航标签切换
**用户描述**: 标签页、导航标签、选项卡、Tab切换、Tabs、Navigation Tabs
**代码位置**:
- 主文件: `static/manage.html` (主导航区域，第75-85行)
- 逻辑: showTab函数
**视觉特征**: 水平排列的五个标签按钮，包含Token管理、系统配置、日志管理、使用教程、系统信息
**修改指引**:
- 添加标签: 编辑第75-85行的标签按钮组
- 修改内容: 编辑对应的数据-tab属性值
- 样式调整: 编辑包含"tab-button"类的元素

### 🔑 Token管理模块

#### 添加Token
**功能描述**: 新增API Token
**用户描述**: 添加Token按钮、新增Token、创建Token、Add Token、Create Token
**代码位置**:
- 主文件: `static/manage.html` (Token管理面板)
- API接口: `src/api/admin.py` (add_token函数)
- 业务逻辑: `src/services/token_manager.py` (Token添加逻辑)
**视觉特征**: 绿色按钮，位于Token列表上方，文本为"添加Token"
**修改指引**:
- 按钮样式: 编辑"添加Token"按钮元素
- 后端逻辑: 编辑add_token函数
- 样式调整: 编辑按钮的bg-green-500类

#### 编辑Token
**功能描述**: 修改现有Token信息
**用户描述**: 编辑Token、修改Token、更新Token、Edit Token、Update Token
**代码位置**:
- 主文件: `static/manage.html` (Token列表中的编辑按钮)
- API接口: `src/api/admin.py` (update_token函数)
- 业务逻辑: `src/services/token_manager.py` (Token更新逻辑)
**视觉特征**: 蓝色编辑图标按钮，每行Token右侧
**修改指引**:
- 图标修改: 编辑编辑按钮的SVG图标
- 弹窗修改: 编辑editTokenModal相关的HTML结构
- 后端逻辑: 编辑update_token函数

#### 删除Token
**功能描述**: 删除指定Token
**用户描述**: 删除Token、移除Token、删除密钥、Delete Token、Remove Token
**代码位置**:
- 主文件: `static/manage.html` (Token列表中的删除按钮)
- API接口: `src/api/admin.py` (delete_token函数)
- 业务逻辑: `src/services/token_manager.py` (Token删除逻辑)
**视觉特征**: 红色删除图标按钮，每行Token右侧
**修改指引**:
- 确认弹窗: 编辑删除确认逻辑
- 图标修改: 编辑删除按钮的SVG图标
- 后端逻辑: 编辑delete_token函数

#### 搜索Token
**功能描述**: 搜索和筛选Token
**用户描述**: 搜索Token、查找Token、Token搜索框、Search Token、Find Token
**代码位置**:
- 主文件: `static/manage.html` (Token管理面板顶部)
- 逻辑: filterTokens函数
**视觉特征**: 输入框，带有搜索图标，placeholder为"搜索Token..."
**修改指引**:
- 样式修改: 编辑搜索输入框的class属性
- 提示文本: 编辑placeholder属性
- 搜索逻辑: 编辑filterTokens函数

#### 刷新Token列表
**功能描述**: 重新加载Token数据
**用户描述**: 刷新Token按钮、刷新列表、重新加载、Refresh Tokens、Reload List
**代码位置**:
- 主文件: `static/manage.html` (Token列表右上角)
- API接口: `src/api/admin.py` (get_tokens函数)
- 逻辑: loadTokens函数
**视觉特征**: 蓝色刷新图标按钮
**修改指引**:
- 图标修改: 编辑刷新按钮的SVG图标
- 加载动画: 编辑loadTokens函数中的加载状态

#### 测试Token
**功能描述**: 验证Token有效性
**用户描述**: 测试Token按钮、验证Token、测试密钥、Test Token、Validate Token
**代码位置**:
- 主文件: `static/manage.html` (Token列表中的测试按钮)
- API接口: `src/api/admin.py` (test_token函数)
- 业务逻辑: `src/services/token_manager.py` (Token测试逻辑)
**视觉特征**: 绿色测试图标按钮，每行Token右侧
**修改指引**:
- 测试逻辑: 编辑test_token函数
- 结果提示: 编辑测试结果的UI反馈

#### 激活/禁用Token
**功能描述**: 切换Token启用状态
**用户描述**: 激活开关、启用/禁用、Token状态、Activate Token、Enable/Disable
**代码位置**:
- 主文件: `static/manage.html` (Token列表中的开关)
- API接口: `src/api/admin.py` (update_token_status接口)
- 业务逻辑: `src/services/token_manager.py` (状态更新逻辑)
**视觉特征**: 切换开关，绿色表示激活状态
**修改指引**:
- 开关样式: 编辑开关的CSS类
- 状态逻辑: 编辑toggleTokenStatus函数

#### 转换为OAuth
**功能描述**: 将Token转换为OAuth格式
**用户描述**: 转换为OAuth、OAuth转换、Token转换、Convert to OAuth、OAuth Conversion
**代码位置**:
- 主文件: `static/manage.html` (Token列表中的转换按钮)
- API接口: `src/api/admin.py` (convert_to_oauth接口)
- 业务逻辑: `src/services/token_manager.py` (OAuth转换逻辑)
**视觉特征**: 紫色转换图标按钮
**修改指引**:
- 转换逻辑: 编辑对应的转换函数
- 权限验证: 编辑OAuth转换的权限检查

### ⚙️ 系统配置模块

#### 安全设置
**功能描述**: 管理员账号和密码配置
**用户描述**: 安全设置、安全配置、认证设置、Security Settings、Authentication
**代码位置**:
- 主文件: `static/manage.html` (系统配置面板)
- API接口: `src/api/admin.py` (update_config接口)
- 配置文件: `config/setting.toml` (安全相关配置)
**视觉特征**: 表单输入框，包含用户名、密码等字段
**修改指引**:
- 表单结构: 编辑安全设置区域的HTML
- 验证逻辑: 编辑对应的表单验证

#### API配置
**功能描述**: API接口相关参数配置
**用户描述**: API配置、接口配置、API设置、API Configuration、API Settings
**代码位置**:
- 主文件: `static/manage.html` (API配置区域)
- API接口: `src/api/admin.py` (update_config接口)
- 配置文件: `config/setting.toml` (API相关配置)
**视觉特征**: 多个配置输入框，如默认生成分辨率、图片质量等
**修改指引**:
- 配置项: 编辑API配置区域的HTML
- 默认值: 编辑对应的配置项

#### 代理配置
**功能描述**: 网络代理参数设置
**用户描述**: 代理配置、代理设置、网络代理、Proxy Configuration、Proxy Settings
**代码位置**:
- 主文件: `static/manage.html` (代理配置区域)
- API接口: `src/api/admin.py` (update_config接口)
- 业务逻辑: `src/services/proxy_manager.py` (代理管理逻辑)
**视觉特征**: 代理类型选择器和输入框
**修改指引**:
- 代理类型: 编辑代理类型选择器
- 地址验证: 编辑代理地址验证逻辑

#### 缓存配置
**功能描述**: 文件缓存参数设置
**用户描述**: 缓存配置、缓存设置、存储配置、Cache Configuration、Storage Settings
**代码位置**:
- 主文件: `static/manage.html` (缓存配置区域)
- 业务逻辑: `src/services/file_cache.py` (文件缓存逻辑)
**视觉特征**: 缓存大小和路径输入框
**修改指引**:
- 缓存参数: 编辑缓存配置区域的输入框
- 缓存逻辑: 编辑 `src/services/file_cache.py`

#### 超时配置
**功能描述**: 请求超时参数设置
**用户描述**: 超时配置、超时设置、时间配置、Timeout Configuration、Timeout Settings
**代码位置**:
- 主文件: `static/manage.html` (超时配置区域)
- 配置文件: 系统超时相关设置
**视觉特征**: 数字输入框，如请求超时时间、重试次数等
**修改指引**:
- 超时值: 编辑超时配置区域的输入框
- 范围验证: 编辑超时值的验证逻辑

#### 其他配置
**功能描述**: 系统高级参数设置
**用户描述**: 其他配置、高级配置、额外设置、Other Settings、Advanced Configuration
**代码位置**:
- 主文件: `static/manage.html` (其他配置区域)
- 配置文件: `config/setting.toml` (杂项配置)
**视觉特征**: 各种开关和输入框，如调试模式、自动重试等
**修改指引**:
- 配置项: 编辑其他配置区域的HTML
- 开关样式: 编辑开关组件的CSS类

#### 保存配置
**功能描述**: 应用所有配置更改
**用户描述**: 保存配置按钮、应用配置、保存设置、Save Configuration、Apply Settings
**代码位置**:
- 主文件: `static/manage.html` (配置面板底部)
- API接口: `src/api/admin.py` (update_config接口)
**视觉特征**: 蓝色保存按钮，文本为"保存配置"
**修改指引**:
- 按钮样式: 编辑保存按钮的CSS类
- 保存逻辑: 编辑saveConfig函数

### 📋 日志管理模块

#### 查看请求日志
**功能描述**: 显示系统请求日志
**用户描述**: 请求日志、访问日志、系统日志、Request Logs、Access Logs
**代码位置**:
- 主文件: `static/manage.html` (日志管理面板)
- API接口: `src/api/admin.py` (get_logs函数)
- 数据库: `src/core/database.py` (日志查询逻辑)
**视觉特征**: 表格形式展示日志数据，包含时间、IP地址、请求路径等列
**修改指引**:
- 表格列: 编辑日志表格的HTML结构
- 日志查询: 编辑 `src/api/admin.py` 的get_logs函数

### 🎨 API接口模块

#### 模型列表接口
**功能描述**: 获取可用的AI模型列表
**用户描述**: 获取模型列表、可用模型、模型接口、Get Models、List Models
**代码位置**:
- 主文件: `src/api/routes.py` (/v1/models接口)
- 逻辑: 返回支持的AI模型列表
**输入输出**: JSON格式的模型数据，包含模型ID、名称、类型等信息
**修改指引**:
- 模型列表: 编辑 `src/api/routes.py` 的models函数
- 新模型: 在返回数据中添加新模型信息

#### 生成内容接口
**功能描述**: AI内容生成主接口
**用户描述**: 生成图片/视频、AI生成、内容生成、Generate Content、Create Images/Videos
**代码位置**:
- 主文件: `src/api/routes.py` (/v1/images/generations接口)
- 业务逻辑: `src/services/generation_handler.py` (生成处理逻辑)
- 客户端: `src/services/sora_client.py` (Sora客户端)
**输入输出**: JSON格式的生成参数输入，输出生成的内容URL或Base64数据
**修改指引**:
- 生成逻辑: 编辑 `src/services/generation_handler.py`
- 新功能: 在 `src/api/routes.py` 添加新接口
- 参数验证: 编辑对应的Pydantic模型

## 🔍 快速查找索引

### 按功能类型查找
- **认证相关**: 管理员登录、退出登录
- **Token管理**: 添加、编辑、删除、搜索、刷新、测试、激活/禁用、转换OAuth
- **系统配置**: 安全设置、API配置、代理配置、缓存配置、超时配置、其他配置、保存配置
- **日志管理**: 查看请求日志
- **API接口**: 模型列表、内容生成

### 按文件位置查找
- **登录页面**: `static/login.html`
- **管理控制台**: `static/manage.html`
- **OpenAI API**: `src/api/routes.py`
- **管理后台API**: `src/api/admin.py`
- **Token管理**: `src/services/token_manager.py`
- **内容生成**: `src/services/generation_handler.py`
- **代理管理**: `src/services/proxy_manager.py`
- **文件缓存**: `src/services/file_cache.py`

### 常用修改场景

#### 界面样式修改
主要编辑 `static/manage.html` 中的TailwindCSS类名：
- 背景颜色: `bg-{color}-{number}`
- 文本颜色: `text-{color}-{number}`
- 按钮样式: `bg-green-500`, `bg-red-500`, `bg-blue-500`
- 布局类: `grid`, `flex`, `container`, `mx-auto`

#### 交互逻辑修改
编辑对应的JavaScript函数：
- 登录逻辑: `login()`
- 标签切换: `showTab()`
- Token操作: `addToken()`, `editToken()`, `deleteToken()`
- 配置保存: `saveConfig()`

#### 后端API修改
编辑对应的Python文件：
- 添加新接口: 在 `src/api/admin.py` 或 `src/api/routes.py` 添加
- 修改现有接口: 编辑对应的函数实现
- 业务逻辑: 在 `src/services/` 目录下修改对应服务

#### 配置项修改
通过Web界面或直接编辑配置文件：
- 界面修改: 在系统配置面板中调整
- 文件修改: 编辑 `config/setting.toml`
- 代码修改: 编辑 `src/core/config.py`

## 📚 相关文档链接

- [项目README](../README.md)
- [部署指南](deployment.md)
- [API文档](api-reference.md)
- [配置说明](configuration.md)

---

*本文档基于项目代码自动生成，更新时间: 2025年*