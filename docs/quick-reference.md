# Sora2API 快速参考指南

## 🚀 快速开始

### 常用功能定位

| 功能需求 | 主要文件 | 关键函数/位置 |
|---------|---------|---------------|
| 修改登录页面样式 | `static/login.html` | 第12-25行背景样式 |
| 修改管理界面布局 | `static/manage.html` | 搜索"tab-button"类 |
| 添加新的Token | `static/manage.html` + `src/api/admin.py` | `addToken()` + `add_token()` |
| 修改API接口 | `src/api/routes.py` | 对应的接口函数 |
| 调整系统配置 | `static/manage.html` | `saveConfig()`函数 |
| 查看日志 | `src/api/admin.py` | `get_logs()`函数 |
| 修改生成逻辑 | `src/services/generation_handler.py` | 主处理函数 |

### 快速修改清单

#### 🎨 界面修改
```html
<!-- 修改按钮颜色 -->
<!-- 原代码 -->
<button class="bg-green-500 ...">添加Token</button>
<!-- 修改为 -->
<button class="bg-blue-500 ...">添加Token</button>

<!-- 修改输入框样式 -->
<!-- 原代码 -->
<input class="border rounded px-3 py-2 ...">
<!-- 修改为 -->
<input class="border-2 border-blue-500 rounded-lg px-4 py-2 ...">
```

#### ⚡ 功能修改
```javascript
// 修改登录逻辑
// 在 static/login.html 第85-120行
async function login() {
    // 添加自定义验证逻辑
    if (username.length < 3) {
        alert('用户名至少需要3个字符');
        return;
    }
    // ... 原有代码
}

// 修改Token测试逻辑
// 在 static/manage.html 中找到 testToken 函数
async function testToken(token) {
    // 添加自定义测试参数
    const response = await fetch('/admin/test-token', {
        // ... 修改请求参数
    });
}
```

#### 🔧 后端修改
```python
# 添加新的配置项
# 在 src/api/admin.py 的 update_config 函数中
async def update_config(config_data: dict):
    # 添加新的配置处理
    if 'new_config' in config_data:
        # 处理新配置项
        pass
    # ... 原有代码

# 修改生成逻辑
# 在 src/services/generation_handler.py 中
async def handle_generation(params):
    # 添加自定义生成逻辑
    if params.get('custom_param'):
        # 处理自定义参数
        pass
    # ... 原有代码
```

## 📍 功能速查表

### 按页面查找

#### 登录页面 (`static/login.html`)
- **功能**: 管理员认证
- **主要元素**: 用户名输入框、密码输入框、登录按钮
- **修改位置**:
  - 样式: 第12-25行 (背景)
  - 表单: 第30-50行 (输入框)
  - 逻辑: 第85-120行 (login函数)

#### 管理控制台 (`static/manage.html`)
- **功能**: 系统管理主界面
- **主要区域**:
  - Token管理面板
  - 系统配置面板
  - 日志管理面板
- **修改位置**:
  - 导航: 搜索"tab-button"类
  - Token表格: 搜索"token-table"
  - 配置表单: 搜索"config-form"

### 按API接口查找

#### OpenAI兼容接口 (`src/api/routes.py`)
- `/v1/models` - 获取模型列表
- `/v1/images/generations` - 生成图片/视频
- **修改位置**: 对应的接口函数实现

#### 管理后台接口 (`src/api/admin.py`)
- `/admin/login` - 管理员登录
- `/admin/tokens` - Token管理 (增删改查)
- `/admin/config` - 系统配置
- `/admin/logs` - 日志查看
- **修改位置**: 对应的接口函数实现

### 按服务模块查找

#### Token管理 (`src/services/token_manager.py`)
- **功能**: Token的增删改查、测试、状态管理
- **主要函数**:
  - `add_token()` - 添加Token
  - `update_token()` - 更新Token
  - `test_token()` - 测试Token
  - `delete_token()` - 删除Token

#### 内容生成 (`src/services/generation_handler.py`)
- **功能**: AI内容生成的核心业务逻辑
- **主要函数**:
  - `handle_generation()` - 处理生成请求
  - `validate_params()` - 参数验证
  - `process_result()` - 结果处理

#### 代理管理 (`src/services/proxy_manager.py`)
- **功能**: 网络代理配置和管理
- **主要函数**:
  - `get_proxy()` - 获取代理
  - `test_proxy()` - 测试代理
  - `update_proxy()` - 更新代理配置

## 🔧 常见修改场景

### 场景1: 修改界面主题颜色
```bash
# 查找所有绿色按钮
grep -r "bg-green-500" static/

# 修改为蓝色
sed -i 's/bg-green-500/bg-blue-500/g' static/manage.html
```

### 场景2: 添加新的配置项
1. 在 `static/manage.html` 添加表单字段
2. 在 `src/api/admin.py` 的 `update_config` 函数中添加处理逻辑
3. 在 `config/setting.toml` 添加默认值

### 场景3: 修改API响应格式
1. 找到对应的API函数（在 `src/api/routes.py` 或 `src/api/admin.py`）
2. 修改返回数据结构
3. 更新前端对应的解析逻辑（在 `static/manage.html`）

### 场景4: 添加新的Token类型
1. 在 `src/services/token_manager.py` 添加新的Token处理逻辑
2. 在 `static/manage.html` 的Token管理界面添加对应UI
3. 在 `src/api/admin.py` 添加新的API接口（如需要）

## 🚨 注意事项

### 修改前检查
- [ ] 备份原始文件
- [ ] 确认修改的文件位置正确
- [ ] 检查依赖关系（前端<->后端API）

### 修改后验证
- [ ] 重启服务验证修改生效
- [ ] 检查浏览器控制台是否有错误
- [ ] 验证相关功能是否正常工作

### 常见错误避免
1. **不要** 直接修改 `src/core/models.py` 中的字段类型，需要同步更新数据库
2. **不要** 删除 `static/manage.html` 中的关键CSS类，可能导致界面错乱
3. **不要** 修改API路径而不更新前端调用
4. **不要** 忘记在 `requirements.txt` 中添加新依赖

## 📞 故障排除

### 界面显示异常
1. 检查浏览器控制台错误信息
2. 确认TailwindCSS类名正确
3. 验证HTML结构完整性

### API调用失败
1. 检查网络请求状态码
2. 查看后端日志 (`docker logs` 或日志文件)
3. 验证请求参数格式

### 功能不生效
1. 确认修改的文件已保存
2. 检查服务是否重启
3. 验证代码语法正确性

## 🔗 相关链接

- [完整功能映射文档](feature-mapping.md)
- [项目README](../README.md)
- [部署指南](deployment.md)

---

*本快速参考指南会定期更新，建议收藏以便快速查阅*