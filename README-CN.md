# Xboard API 文档

## 文档说明
本文档是 Xboard 系统的完整 API 接口参考，按访问权限（访客、用户、管理员等）分类整理了所有可用接口，包括接口路径、用途、返回数据等详细信息，适用于开发者对接 Xboard 系统时查阅。

原始文档来源：[https://github.com/cedar2025/Xboard/issues/553](https://github.com/cedar2025/Xboard/issues/553)



## API 基础 URL
- V1 API: `/api/v1/`
- V2 API: `/api/v2/`

## 身份验证
- **访客 (Guest)**: 无需身份验证
- **用户 (User)**: 需要用户身份验证（用户中间件）
- **员工 (Staff)**: 需要员工权限（员工中间件）
- **管理员 (Admin)**: 需要管理员权限（管理员中间件）
- **客户端 (Client)**: 需要客户端身份验证（客户端中间件）
- **服务器 (Server)**: 需要服务器身份验证（服务器中间件）

## 🌐 访客 API（公开 - 无需身份验证）

### 套餐计划
- `GET /api/v1/guest/plan/fetch`
  - 用途：获取所有可供公开查看的订阅套餐
  - 返回：包含价格、功能和限制的可用套餐列表
  - 数据：套餐 ID、名称、价格（月付/季付/年付）、传输限制、速度限制、设备限制、容量信息

### 配置
- `GET /api/v1/guest/comm/config`
  - 用途：获取公共配置设置
  - 返回：公共应用配置
  - 数据：服务条款 URL、邮箱验证设置、邀请要求、reCAPTCHA 设置、应用描述、应用 URL、logo

### 支付 Webhook
- `GET/POST /api/v1/guest/payment/notify/{method}/{uuid}`
  - 用途：处理来自支付提供商的支付通知
  - 返回：支付处理状态
  - 数据：支付确认和处理结果

### Telegram Webhook
- `POST /api/v1/guest/telegram/webhook`
  - 用途：处理 Telegram 机器人 webhook 事件
  - 返回：Webhook 处理状态
  - 数据：Telegram 机器人事件处理结果

## 🔐 身份验证 API（Passport）

### V1 身份验证
- `POST /api/v1/passport/auth/register`
  - 用途：用户注册
  - 返回：注册成功/失败
  - 数据：用户账户创建状态

- `POST /api/v1/passport/auth/login`
  - 用途：用户登录
  - 返回：身份验证令牌和用户信息
  - 数据：JWT 令牌、用户详情、会话信息

- `GET /api/v1/passport/auth/token2Login`
  - 用途：基于令牌的登录
  - 返回：登录状态
  - 数据：身份验证状态

- `POST /api/v1/passport/auth/forget`
  - 用途：密码重置请求
  - 返回：重置邮件状态
  - 数据：密码重置确认

- `POST /api/v1/passport/auth/getQuickLoginUrl`
  - 用途：生成快速登录 URL
  - 返回：快速登录 URL
  - 数据：临时登录 URL

- `POST /api/v1/passport/auth/loginWithMailLink`
  - 用途：通过邮件链接登录
  - 返回：登录状态
  - 数据：身份验证确认

### 通信
- `POST /api/v1/passport/comm/sendEmailVerify`
  - 用途：发送邮件验证
  - 返回：邮件发送状态
  - 数据：验证邮件送达确认

- `POST /api/v1/passport/comm/pv`
  - 用途：页面浏览跟踪
  - 返回：跟踪状态
  - 数据：分析跟踪确认

### V2 身份验证
与 V1 端点相同，但前缀为 `/api/v2/passport/`。

## 👤 用户 API（已认证用户）

### 用户管理
- `GET /api/v1/user/info`
  - 用途：获取当前用户信息
  - 返回：用户资料数据
  - 数据：邮箱、传输限制、登录历史、订阅状态、余额、佣金信息、Telegram ID、头像 URL

- `POST /api/v1/user/update`
  - 用途：更新用户偏好设置
  - 返回：更新状态
  - 数据：更新后的用户偏好（提醒等）

- `POST /api/v1/user/changePassword`
  - 用途：修改用户密码
  - 返回：密码修改状态
  - 数据：密码更新确认

- `GET /api/v1/user/resetSecurity`
  - 用途：重置安全凭证（UUID、令牌）
  - 返回：新的订阅 URL
  - 数据：带有更新令牌的新订阅 URL

- `GET /api/v1/user/checkLogin`
  - 用途：检查登录状态
  - 返回：登录状态和权限
  - 数据：登录状态、管理员权限

- `GET /api/v1/user/getStat`
  - 用途：获取用户统计信息
  - 返回：用户统计摘要
  - 数据：待处理订单数、开放工单数量、邀请用户数

- `GET /api/v1/user/getSubscribe`
  - 用途：获取订阅信息
  - 返回：订阅详情和 URL
  - 数据：套餐详情、订阅 URL、使用统计、重置计划

- `POST /api/v1/user/transfer`
  - 用途：将佣金转移到余额
  - 返回：转移状态
  - 数据：转移确认和更新后的余额

- `POST /api/v1/user/getQuickLoginUrl`
  - 用途：生成快速登录 URL
  - 返回：快速登录 URL
  - 数据：临时登录 URL

### 会话管理
- `GET /api/v1/user/getActiveSession`
  - 用途：获取活跃会话
  - 返回：活跃会话列表
  - 数据：会话详情、登录时间、IP 地址

- `POST /api/v1/user/removeActiveSession`
  - 用途：移除特定会话
  - 返回：会话移除状态
  - 数据：会话终止确认

### 订单与账单
- `POST /api/v1/user/order/save`
  - 用途：创建新订单
  - 返回：订单创建状态
  - 数据：订单详情、支付信息

- `POST /api/v1/user/order/checkout`
  - 用途：结算订单
  - 返回：支付处理信息
  - 数据：支付 URL、订单状态

- `GET /api/v1/user/order/check`
  - 用途：检查订单状态
  - 返回：订单状态
  - 数据：订单处理状态

- `GET /api/v1/user/order/detail`
  - 用途：获取订单详情
  - 返回：详细的订单信息
  - 数据：订单项、价格、状态、支付信息

- `GET /api/v1/user/order/fetch`
  - 用途：获取用户的订单
  - 返回：用户订单列表
  - 数据：带有状态和详情的订单历史

- `GET /api/v1/user/order/getPaymentMethod`
  - 用途：获取可用支付方式
  - 返回：支付选项列表
  - 数据：支付提供商、费用、可用性

- `POST /api/v1/user/order/cancel`
  - 用途：取消订单
  - 返回：取消状态
  - 数据：订单取消确认

### 套餐计划
- `GET /api/v1/user/plan/fetch`
  - 用途：获取已认证用户的可用套餐
  - 返回：带有用户特定可用性的套餐列表
  - 数据：包含价格、用户资格、续订选项的套餐

### 邀请
- `GET /api/v1/user/invite/save`
  - 用途：生成邀请码
  - 返回：邀请码
  - 数据：邀请码和分享信息

- `GET /api/v1/user/invite/fetch`
  - 用途：获取邀请统计信息
  - 返回：邀请数据
  - 数据：邀请码、使用统计、佣金

- `GET /api/v1/user/invite/details`
  - 用途：获取详细的邀请信息
  - 返回：邀请详情
  - 数据：详细的邀请统计和收益

### 支持与通信
- `GET /api/v1/user/notice/fetch`
  - 用途：获取用户通知
  - 返回：通知列表
  - 数据：系统公告、更新、警报

- `POST /api/v1/user/ticket/save`
  - 用途：创建支持工单
  - 返回：工单创建状态
  - 数据：工单 ID 和详情

- `GET /api/v1/user/ticket/fetch`
  - 用途：获取用户的工单
  - 返回：支持工单列表
  - 数据：工单历史、状态、回复

- `POST /api/v1/user/ticket/reply`
  - 用途：回复工单
  - 返回：回复状态
  - 数据：回复确认

- `POST /api/v1/user/ticket/close`
  - 用途：关闭工单
  - 返回：关闭状态
  - 数据：工单关闭确认

- `POST /api/v1/user/ticket/withdraw`
  - 用途：撤回工单
  - 返回：撤回状态
  - 数据：工单撤回确认

### 服务器
- `GET /api/v1/user/server/fetch`
  - 用途：获取可用服务器
  - 返回：用户可访问的服务器列表
  - 数据：服务器详情、位置、状态、协议

### 优惠券
- `POST /api/v1/user/coupon/check`
  - 用途：验证优惠券代码
  - 返回：优惠券有效性和折扣信息
  - 数据：优惠券详情、折扣金额、有效期

### Telegram 集成
- `GET /api/v1/user/telegram/getBotInfo`
  - 用途：获取 Telegram 机器人信息
  - 返回：机器人连接信息
  - 数据：机器人详情、连接状态

### 配置
- `GET /api/v1/user/comm/config`
  - 用途：获取用户特定配置
  - 返回：用户配置设置
  - 数据：用户偏好、功能可用性

- `POST /api/v1/user/comm/getStripePublicKey`
  - 用途：获取用于支付的 Stripe 公钥
  - 返回：Stripe 配置
  - 数据：用于支付处理的 Stripe 公钥

### 知识库
- `GET /api/v1/user/knowledge/fetch`
  - 用途：获取知识库文章
  - 返回：帮助文章列表
  - 数据：文章、分类、内容

- `GET /api/v1/user/knowledge/getCategory`
  - 用途：获取知识库分类
  - 返回：分类列表
  - 数据：分类结构和组织

### 统计
- `GET /api/v1/user/stat/getTrafficLog`
  - 用途：获取流量使用日志
  - 返回：流量使用历史
  - 数据：流量日志、使用模式、时间戳

### V2 用户 API
- `GET /api/v2/user/resetSecurity`
  - 用途：重置安全凭证
  - 返回：新的安全凭证
  - 数据：更新后的安全令牌

- `GET /api/v2/user/info`
  - 用途：获取用户信息（V2）
  - 返回：用户资料数据
  - 数据：增强的用户资料信息

## 📱 客户端 API（客户端身份验证）

### 订阅
- `GET /api/v1/client/subscribe`
  - 用途：获取订阅配置
  - 返回：VPN 应用的客户端配置
  - 数据：服务器配置、协议、连接详情

### 应用配置
- `GET /api/v1/client/app/getConfig`
  - 用途：获取应用配置
  - 返回：应用配置设置
  - 数据：应用设置、功能、URL

- `GET /api/v1/client/app/getVersion`
  - 用途：获取应用版本信息
  - 返回：版本详情
  - 数据：当前版本、更新可用性

## 🖥️ 服务器 API（服务器身份验证）

### UniProxy
- `GET /api/v1/server/UniProxy/config`
  - 用途：获取服务器配置
  - 返回：服务器配置
  - 数据：服务器设置和参数

- `GET /api/v1/server/UniProxy/user`
  - 用途：获取服务器的用户数据
  - 返回：服务器的用户信息
  - 数据：用户访问权限、配额、设置

- `POST /api/v1/server/UniProxy/push`
  - 用途：向服务器推送数据
  - 返回：推送状态
  - 数据：数据同步确认

- `POST /api/v1/server/UniProxy/alive`
  - 用途：服务器心跳检测
  - 返回：存活状态
  - 数据：服务器健康状态

- `GET /api/v1/server/UniProxy/alivelist`
  - 用途：获取存活服务器列表
  - 返回：活跃服务器列表
  - 数据：服务器状态和可用性

- `POST /api/v1/server/UniProxy/status`
  - 用途：更新服务器状态
  - 返回：状态更新确认
  - 数据：服务器状态更新

### Shadowsocks Tidalab
- `GET /api/v1/server/ShadowsocksTidalab/user`
  - 用途：获取 Shadowsocks 的用户数据
  - 返回：Shadowsocks 用户配置
  - 数据：Shadowsocks 特定的用户设置

- `POST /api/v1/server/ShadowsocksTidalab/submit`
  - 用途：提交 Shadowsocks 数据
  - 返回：提交状态
  - 数据：数据提交确认

### Trojan Tidalab
- `GET /api/v1/server/TrojanTidalab/config`
  - 用途：获取 Trojan 服务器配置
  - 返回：Trojan 配置
  - 数据：Trojan 服务器设置

- `GET /api/v1/server/TrojanTidalab/user`
  - 用途：获取 Trojan 的用户数据
  - 返回：Trojan 用户配置
  - 数据：Trojan 特定的用户设置

- `POST /api/v1/server/TrojanTidalab/submit`
  - 用途：提交 Trojan 数据
  - 返回：提交状态
  - 数据：数据提交确认

## 👨‍💼 员工 API（员工身份验证）
注意：员工功能存在，但似乎集成到管理员路由中，而非拥有专用的员工路由。员工用户具有 `is_staff` 标志，可以访问某些具有有限权限的管理员功能。

### 用户管理（员工级别）
- 员工可以管理非管理员、非员工用户
- 有限的用户更新能力
- 向用户发送电子邮件
- 封禁用户
- 访问用户信息

## 🔧 管理员 API（管理员身份验证）

### 配置管理
- `GET /api/v2/{admin_path}/config/fetch`
  - 用途：获取系统配置
  - 返回：完整的系统设置
  - 数据：所有配置参数

- `POST /api/v2/{admin_path}/config/save`
  - 用途：保存系统配置
  - 返回：保存状态
  - 数据：配置更新确认

- `GET /api/v2/{admin_path}/config/getEmailTemplate`
  - 用途：获取邮件模板
  - 返回：邮件模板配置
  - 数据：邮件模板和设置

- `GET /api/v2/{admin_path}/config/getThemeTemplate`
  - 用途：获取主题模板
  - 返回：主题配置
  - 数据：可用主题和设置

- `POST /api/v2/{admin_path}/config/setTelegramWebhook`
  - 用途：配置 Telegram webhook
  - 返回：Webhook 设置状态
  - 数据：Telegram 集成状态

- `POST /api/v2/{admin_path}/config/testSendMail`
  - 用途：测试邮件配置
  - 返回：邮件测试结果
  - 数据：邮件发送测试状态

### 套餐管理
- `GET /api/v2/{admin_path}/plan/fetch`
  - 用途：获取所有套餐（管理员视图）
  - 返回：带有管理员详情的完整套餐列表
  - 数据：所有套餐及其用户数量、收入、管理员设置

- `POST /api/v2/{admin_path}/plan/save`
  - 用途：创建/更新套餐
  - 返回：套餐保存状态
  - 数据：套餐创建/更新确认

- `POST /api/v2/{admin_path}/plan/drop`
  - 用途：删除套餐
  - 返回：删除状态
  - 数据：套餐删除确认

- `POST /api/v2/{admin_path}/plan/update`
  - 用途：更新套餐详情
  - 返回：更新状态
  - 数据：套餐更新确认

- `POST /api/v2/{admin_path}/plan/sort`
  - 用途：重新排序套餐
  - 返回：排序状态
  - 数据：套餐排序确认

### 服务器管理

#### 服务器组
- `GET /api/v2/{admin_path}/server/group/fetch`
  - 用途：获取服务器组
  - 返回：服务器组列表
  - 数据：组配置和权限

- `POST /api/v2/{admin_path}/server/group/save`
  - 用途：创建/更新服务器组
  - 返回：组保存状态
  - 数据：组创建/更新确认

- `POST /api/v2/{admin_path}/server/group/drop`
  - 用途：删除服务器组
  - 返回：删除状态
  - 数据：组删除确认

#### 服务器路由
- `GET /api/v2/{admin_path}/server/route/fetch`
  - 用途：获取服务器路由
  - 返回：服务器路由列表
  - 数据：路由配置和规则

- `POST /api/v2/{admin_path}/server/route/save`
  - 用途：创建/更新服务器路由
  - 返回：路由保存状态
  - 数据：路由创建/更新确认

- `POST /api/v2/{admin_path}/server/route/drop`
  - 用途：删除服务器路由
  - 返回：删除状态
  - 数据：路由删除确认

#### 服务器管理
- `GET /api/v2/{admin_path}/server/manage/getNodes`
  - 用途：获取服务器节点
  - 返回：服务器节点列表
  - 数据：节点详情、状态、配置

- `POST /api/v2/{admin_path}/server/manage/update`
  - 用途：更新服务器
  - 返回：更新状态
  - 数据：服务器更新确认

- `POST /api/v2/{admin_path}/server/manage/save`
  - 用途：创建服务器
  - 返回：创建状态
  - 数据：服务器创建确认

- `POST /api/v2/{admin_path}/server/manage/drop`
  - 用途：删除服务器
  - 返回：删除状态
  - 数据：服务器删除确认

- `POST /api/v2/{admin_path}/server/manage/copy`
  - 用途：复制服务器配置
  - 返回：复制状态
  - 数据：服务器复制确认

- `POST /api/v2/{admin_path}/server/manage/sort`
  - 用途：重新排序服务器
  - 返回：排序状态
  - 数据：服务器排序确认

### 订单管理
- `GET/POST /api/v2/{admin_path}/order/fetch`
  - 用途：获取带过滤的订单
  - 返回：分页的订单列表
  - 数据：订单详情、用户信息、支付状态

- `POST /api/v2/{admin_path}/order/update`
  - 用途：更新订单
  - 返回：更新状态
  - 数据：订单更新确认

- `POST /api/v2/{admin_path}/order/assign`
  - 用途：将订单分配到套餐
  - 返回：分配状态
  - 数据：订单分配确认

- `POST /api/v2/{admin_path}/order/paid`
  - 用途：标记订单为已支付
  - 返回：支付状态
  - 数据：支付确认

- `POST /api/v2/{admin_path}/order/cancel`
  - 用途：取消订单
  - 返回：取消状态
  - 数据：订单取消确认

- `POST /api/v2/{admin_path}/order/detail`
  - 用途：获取订单详情
  - 返回：详细的订单信息
  - 数据：完整的订单信息

### 用户管理
- `GET/POST /api/v2/{admin_path}/user/fetch`
  - 用途：获取带过滤和分页的用户
  - 返回：分页的用户列表
  - 数据：用户详情、订阅信息、使用统计

- `POST /api/v2/{admin_path}/user/update`
  - 用途：更新用户
  - 返回：更新状态
  - 数据：用户更新确认

- `GET /api/v2/{admin_path}/user/getUserInfoById`
  - 用途：获取特定用户信息
  - 返回：用户详情
  - 数据：完整的用户信息

- `POST /api/v2/{admin_path}/user/generate`
  - 用途：生成用户账户
  - 返回：生成状态
  - 数据：新用户账户详情

- `POST /api/v2/{admin_path}/user/dumpCSV`
  - 用途：将用户导出为 CSV
  - 返回：CSV 导出
  - 数据：CSV 格式的用户数据

- `POST /api/v2/{admin_path}/user/sendMail`
  - 用途：向用户发送电子邮件
  - 返回：邮件发送状态
  - 数据：邮件送达确认

- `POST /api/v2/{admin_path}/user/ban`
  - 用途：封禁用户
  - 返回：封禁状态
  - 数据：用户封禁确认

- `POST /api/v2/{admin_path}/user/resetSecret`
  - 用途：重置用户密钥
  - 返回：重置状态
  - 数据：密钥重置确认

- `POST /api/v2/{admin_path}/user/setInviteUser`
  - 用途：设置邀请关系
  - 返回：设置状态
  - 数据：邀请关系确认

- `POST /api/v2/{admin_path}/user/destroy`
  - 用途：删除用户
  - 返回：删除状态
  - 数据：用户删除确认

### 统计与分析
- `GET /api/v2/{admin_path}/stat/getOverride`
  - 用途：获取系统概览
  - 返回：系统统计概览
  - 数据：关键指标、收入、用户数量

- `GET /api/v2/{admin_path}/stat/getStats`
  - 用途：获取详细统计
  - 返回：综合统计
  - 数据：收入、使用量、增长指标

- `GET /api/v2/{admin_path}/stat/getServerLastRank`
  - 用途：获取服务器性能排名
  - 返回：服务器排名数据
  - 数据：服务器性能指标

- `GET /api/v2/{admin_path}/stat/getServerYesterdayRank`
  - 用途：获取昨天的服务器排名
  - 返回：历史服务器数据
  - 数据：前一天的服务器指标

- `GET /api/v2/{admin_path}/stat/getOrder`
  - 用途：获取订单统计
  - 返回：订单分析
  - 数据：订单趋势、收入数据

- `GET/POST /api/v2/{admin_path}/stat/getStatUser`
  - 用途：获取用户统计
  - 返回：用户分析
  - 数据：用户增长、活动指标

- `GET /api/v2/{admin_path}/stat/getRanking`
  - 用途：获取排名数据
  - 返回：各种排名
  - 数据：用户、服务器、收入排名

- `GET /api/v2/{admin_path}/stat/getStatRecord`
  - 用途：获取统计记录
  - 返回：历史统计
  - 数据：历史数据记录

- `GET /api/v2/{admin_path}/stat/getTrafficRank`
  - 用途：获取流量排名
  - 返回：流量使用排名
  - 数据：用户/服务器的流量使用情况

### 通知管理
- `GET /api/v2/{admin_path}/notice/fetch`
  - 用途：获取系统通知
  - 返回：通知列表
  - 数据：通知内容、可见性、调度

- `POST /api/v2/{admin_path}/notice/save`
  - 用途：创建通知
  - 返回：创建状态
  - 数据：通知创建确认

- `POST /api/v2/{admin_path}/notice/update`
  - 用途：更新通知
  - 返回：更新状态
  - 数据：通知更新确认

- `POST /api/v2/{admin_path}/notice/drop`
  - 用途：删除通知
  - 返回：删除状态
  - 数据：通知删除确认

- `POST /api/v2/{admin_path}/notice/show`
  - 用途：切换通知可见性
  - 返回：可见性状态
  - 数据：通知可见性确认

- `POST /api/v2/{admin_path}/notice/sort`
  - 用途：重新排序通知
  - 返回：排序状态
  - 数据：通知排序确认

### 工单管理
- `GET/POST /api/v2/{admin_path}/ticket/fetch`
  - 用途：获取支持工单
  - 返回：分页的工单列表
  - 数据：工单详情、用户信息、状态

- `POST /api/v2/{admin_path}/ticket/reply`
  - 用途：回复工单
  - 返回：回复状态
  - 数据：工单回复确认

- `POST /api/v2/{admin_path}/ticket/close`
  - 用途：关闭工单
  - 返回：关闭状态
  - 数据：工单关闭确认

### 优惠券管理
- `GET/POST /api/v2/{admin_path}/coupon/fetch`
  - 用途：获取优惠券
  - 返回：优惠券列表
  - 数据：优惠券详情、使用统计

- `POST /api/v2/{admin_path}/coupon/generate`
  - 用途：生成优惠券
  - 返回：生成状态
  - 数据：新优惠券代码

- `POST /api/v2/{admin_path}/coupon/drop`
  - 用途：删除优惠券
  - 返回：删除状态
  - 数据：优惠券删除确认

- `POST /api/v2/{admin_path}/coupon/show`
  - 用途：切换优惠券可见性
  - 返回：可见性状态
  - 数据：优惠券可见性确认

- `POST /api/v2/{admin_path}/coupon/update`
  - 用途：更新优惠券
  - 返回：更新状态
  - 数据：优惠券更新确认

### 知识库管理
- `GET /api/v2/{admin_path}/knowledge/fetch`
  - 用途：获取知识文章
  - 返回：文章列表
  - 数据：文章内容、分类、可见性

- `GET /api/v2/{admin_path}/knowledge/getCategory`
  - 用途：获取知识分类
  - 返回：分类结构
  - 数据：分类层次结构和组织

- `POST /api/v2/{admin_path}/knowledge/save`
  - 用途：创建/更新文章
  - 返回：保存状态
  - 数据：文章保存确认

- `POST /api/v2/{admin_path}/knowledge/show`
  - 用途：切换文章可见性
  - 返回：可见性状态
  - 数据：文章可见性确认

- `POST /api/v2/{admin_path}/knowledge/drop`
  - 用途：删除文章
  - 返回：删除状态
  - 数据：文章删除确认

- `POST /api/v2/{admin_path}/knowledge/sort`
  - 用途：重新排序文章
  - 返回：排序状态
  - 数据：文章排序确认

### 支付管理
- `GET /api/v2/{admin_path}/payment/fetch`
  - 用途：获取支付方式
  - 返回：支付提供商列表
  - 数据：支付方式配置

- `GET /api/v2/{admin_path}/payment/getPaymentMethods`
  - 用途：获取可用支付方式
  - 返回：支付方式列表
  - 数据：可用支付选项

- `POST /api/v2/{admin_path}/payment/getPaymentForm`
  - 用途：获取支付表单配置
  - 返回：表单配置
  - 数据：支付表单设置

- `POST /api/v2/{admin_path}/payment/save`
  - 用途：保存支付方式
  - 返回：保存状态
  - 数据：支付方式保存确认

- `POST /api/v2/{admin_path}/payment/drop`
  - 用途：删除支付方式
  - 返回：删除状态
  - 数据：支付方式删除确认

- `POST /api/v2/{admin_path}/payment/show`
  - 用途：切换支付方式可见性
  - 返回：可见性状态
  - 数据：支付方式可见性确认

- `POST /api/v2/{admin_path}/payment/sort`
  - 用途：重新排序支付方式
  - 返回：排序状态
  - 数据：支付方式排序确认

### 系统管理
- `GET /api/v2/{admin_path}/system/getSystemStatus`
  - 用途：获取系统状态
  - 返回：系统健康信息
  - 数据：服务器状态、性能指标

- `GET /api/v2/{admin_path}/system/getQueueStats`
  - 用途：获取队列统计
  - 返回：队列性能数据
  - 数据：队列指标、作业统计

- `GET /api/v2/{admin_path}/system/getQueueWorkload`
  - 用途：获取队列工作量
  - 返回：当前队列工作量
  - 数据：队列负载和处理时间

- `GET /api/v2/{admin_path}/system/getQueueMasters`
  - 用途：获取队列主节点（Horizon）
  - 返回：队列主节点状态
  - 数据：Horizon 监督程序信息

- `GET /api/v2/{admin_path}/system/getSystemLog`
  - 用途：获取系统日志
  - 返回：系统日志条目
  - 数据：应用日志、错误、事件

- `GET /api/v2/{admin_path}/system/getHorizonFailedJobs`
  - 用途：获取失败的作业
  - 返回：失败作业列表
  - 数据：失败作业详情和错误

- `POST /api/v2/{admin_path}/system/clearSystemLog`
  - 用途：清除系统日志
  - 返回：清除状态
  - 数据：日志清除确认

- `GET /api/v2/{admin_path}/system/getLogClearStats`
  - 用途：获取日志清除统计
  - 返回：日志管理统计
  - 数据：日志存储和清除指标

### 主题管理
- `GET /api/v2/{admin_path}/theme/getThemes`
  - 用途：获取可用主题
  - 返回：主题列表
  - 数据：主题详情、配置

- `POST /api/v2/{admin_path}/theme/upload`
  - 用途：上传主题
  - 返回：上传状态
  - 数据：主题上传确认

- `POST /api/v2/{admin_path}/theme/delete`
  - 用途：删除主题
  - 返回：删除状态
  - 数据：主题删除确认

- `POST /api/v2/{admin_path}/theme/saveThemeConfig`
  - 用途：保存主题配置
  - 返回：保存状态
  - 数据：主题配置保存确认

- `POST /api/v2/{admin_path}/theme/getThemeConfig`
  - 用途：获取主题配置
  - 返回：主题配置
  - 数据：当前主题设置

### 插件管理
- `GET /api/v2/{admin_path}/plugin/getPlugins`
  - 用途：获取已安装插件
  - 返回：插件列表
  - 数据：插件详情、状态、配置

- `POST /api/v2/{admin_path}/plugin/upload`
  - 用途：上传插件
  - 返回：上传状态
  - 数据：插件上传确认

- `POST /api/v2/{admin_path}/plugin/delete`
  - 用途：删除插件
  - 返回：删除状态
  - 数据：插件删除确认

- `POST /api/v2/{admin_path}/plugin/install`
  - 用途：安装插件
  - 返回：安装状态
  - 数据：插件安装确认

- `POST /api/v2/{admin_path}/plugin/uninstall`
  - 用途：卸载插件
  - 返回：卸载状态
  - 数据：插件卸载确认

- `POST /api/v2/{admin_path}/plugin/enable`
  - 用途：启用插件
  - 返回：启用状态
  - 数据：插件启用确认

- `POST /api/v2/{admin_path}/plugin/disable`
  - 用途：禁用插件
  - 返回：禁用状态
  - 数据：插件禁用确认

- `GET /api/v2/{admin_path}/plugin/config`
  - 用途：获取插件配置
  - 返回：插件配置
  - 数据：插件设置

- `POST /api/v2/{admin_path}/plugin/config`
  - 用途：更新插件配置
  - 返回：更新状态
  - 数据：插件配置更新确认

### 流量重置管理
- `GET /api/v2/{admin_path}/traffic-reset/logs`
  - 用途：获取流量重置日志
  - 返回：重置日志条目
  - 数据：流量重置历史和详情

- `GET /api/v2/{admin_path}/traffic-reset/stats`
  - 用途：获取流量重置统计
  - 返回：重置统计
  - 数据：流量重置指标和趋势

- `GET /api/v2/{admin_path}/traffic-reset/user/{userId}/history`
  - 用途：获取用户流量重置历史
  - 返回：用户特定的重置历史
  - 数据：单个用户的重置记录

- `POST /api/v2/{admin_path}/traffic-reset/reset-user`
  - 用途：重置用户流量
  - 返回：重置状态
  - 数据：用户流量重置确认

## 📝 说明
- **管理员路径**: V2 管理员路由中的 `{admin_path}` 是根据 `secure_path` 或 `frontend_admin_path` 配置设置动态生成的。

- **身份验证中间件**:
  - `user`: 需要有效的用户身份验证
  - `admin`: 需要管理员权限
  - `staff`: 需要员工权限
  - `client`: 需要客户端令牌身份验证
  - `server`: 需要服务器身份验证

- **响应格式**: 大多数端点以标准化格式返回数据：
  ```json
  {
    "data": [...], // 实际响应数据
    "message": "成功消息",
    "status": true/false
  }