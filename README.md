---
title: Xboard API Documentation
description: Comprehensive API reference for Xboard VPN management system
source: https://github.com/cedar2025/Xboard/issues/553
---

# Xboard API Documentation

This document provides a comprehensive overview of all API endpoints available in the Xboard system, organized by access level.

## 📋 Table of Contents
- [🔗 API Base URLs](#-api-base-urls)
- [🔐 Authentication](#-authentication)
- [🌐 Guest APIs (Public)](#-guest-apis-public)
- [🔐 Authentication APIs (Passport)](#-authentication-apis-passport)
- [👤 User APIs](#-user-apis)
- [💻 Client APIs](#-client-apis)
- [🖥️ Server APIs](#%EF%B8%8F-server-apis)
- [👔 Staff APIs](#-staff-apis)
- [⚙️ Admin APIs](#%EF%B8%8F-admin-apis)

## 🔗 API Base URLs
| Version | Base URL     |
|---------|--------------|
| V1      | `/api/v1/`   |
| V2      | `/api/v2/`   |

## 🔐 Authentication
| Level    | Requirements                          |
|----------|---------------------------------------|
| Guest    | No authentication required           |
| User     | Requires user authentication          |
| Staff    | Requires staff privileges            |
| Admin    | Requires admin privileges            |
| Client   | Requires client authentication       |
| Server   | Requires server authentication       |

## 🌐 Guest APIs (Public - No Authentication Required)

### 📋 Plans
**Endpoint**: `GET /api/v1/guest/plan/fetch`
- **Purpose**: Get all available subscription plans for public viewing
- **Returns**: List of available plans with pricing, features, and limits
- **Data**: Plan ID, name, prices (monthly/quarterly/yearly), transfer limits, speed limits, device limits, capacity info

### ⚙️ Configuration
**Endpoint**: `GET /api/v1/guest/comm/config`
- **Purpose**: Get public configuration settings
- **Returns**: Public app configuration
- **Data**: ToS URL, email verification settings, invite requirements, reCAPTCHA settings, app description, app URL, logo

### 💳 Payment Webhooks
**Endpoint**: `GET/POST /api/v1/guest/payment/notify/{method}/{uuid}`
- **Purpose**: Handle payment notifications from payment providers
- **Returns**: Payment processing status
- **Data**: Payment confirmation and processing results

### 🤖 Telegram Webhooks
**Endpoint**: `POST /api/v1/guest/telegram/webhook`
- **Purpose**: Handle Telegram bot webhook events
- **Returns**: Webhook processing status
- **Data**: Telegram bot event processing results

## 🔐 Authentication APIs (Passport)

### V1 Authentication
**Endpoint**: `POST /api/v1/passport/auth/register`
- **Purpose**: User registration
- **Returns**: Registration success/failure
- **Data**: User account creation status

**Endpoint**: `POST /api/v1/passport/auth/login`
- **Purpose**: User login
- **Returns**: Authentication token and user info
- **Data**: JWT token, user details, session info

**Endpoint**: `GET /api/v1/passport/auth/token2Login`
- **Purpose**: Token-based login
- **Returns**: Login status
- **Data**: Login result with token validation

**Endpoint**: `GET /api/v1/passport/auth/getQuickLoginUrl`
- **Purpose**: Get quick login URL
- **Returns**: Quick login URL
- **Data**: Temporary login URL with expiration

**Endpoint**: `POST /api/v1/passport/auth/sendEmailVerify`
- **Purpose**: Send email verification
- **Returns**: Email sending status
- **Data**: Email verification request confirmation

**Endpoint**: `POST /api/v1/passport/auth/getCaptcha`
- **Purpose**: Get CAPTCHA challenge
- **Returns**: CAPTCHA image and data
- **Data**: CAPTCHA challenge and answer

**Endpoint**: `POST /api/v1/passport/auth/forget`
- **Purpose**: Password reset request
- **Returns**: Password reset initiation status
- **Data**: Reset token and instructions

**Endpoint**: `POST /api/v1/passport/auth/reset`
- **Purpose**: Password reset confirmation
- **Returns**: Password reset success/failure
- **Data**: Password update confirmation

### V2 Authentication
**Endpoint**: `POST /api/v2/passport/auth/login`
- **Purpose**: User login (V2)
- **Returns**: Authentication token and user info
- **Data**: JWT token, user details, session info

**Endpoint**: `GET /api/v2/passport/auth/check`
- **Purpose**: Check authentication status
- **Returns**: Authentication validity
- **Data**: Session status and user info

## 👤 User APIs

### 👤 User Profile
**Endpoint**: `GET /api/v1/user/profile/fetch`
- **Purpose**: Get user profile information
- **Returns**: Complete user profile data
- **Data**: User details, subscription info, settings

**Endpoint**: `POST /api/v1/user/profile/update`
- **Purpose**: Update user profile
- **Returns**: Profile update status
- **Data**: Updated profile confirmation

**Endpoint**: `POST /api/v1/user/profile/transfer`
- **Purpose**: Transfer account data
- **Returns**: Data transfer status
- **Data**: Transfer confirmation and details

**Endpoint**: `POST /api/v1/user/profile/resetSecurity`
- **Purpose**: Reset security settings
- **Returns**: Security reset status
- **Data**: Security settings confirmation

**Endpoint**: `POST /api/v1/user/profile/close`
- **Purpose**: Close user account
- **Returns**: Account closure status
- **Data**: Account deletion confirmation

### 📊 User Statistics
**Endpoint**: `GET /api/v1/user/stat/getTraffic`
- **Purpose**: Get traffic statistics
- **Returns**: Traffic usage data
- **Data**: Upload/download statistics, usage history

**Endpoint**: `GET /api/v1/user/stat/getOnline`
- **Purpose**: Get online status
- **Returns**: Online session information
- **Data**: Active connections, session details

**Endpoint**: `GET /api/v1/user/stat/getRate`
- **Purpose**: Get usage rates
- **Returns**: Usage rate statistics
- **Data**: Consumption rates, speed metrics

### 🛒 Orders
**Endpoint**: `GET /api/v1/user/order/fetch`
- **Purpose**: Get user orders
- **Returns**: Order history and details
- **Data**: Order list with status and payment info

**Endpoint**: `POST /api/v1/user/order/save`
- **Purpose**: Create new order
- **Returns**: Order creation status
- **Data**: New order confirmation and details

**Endpoint**: `POST /api/v1/user/order/checkout`
- **Purpose**: Process order checkout
- **Returns**: Checkout processing status
- **Data**: Payment instructions and order finalization

**Endpoint**: `GET /api/v1/user/order/detail`
- **Purpose**: Get order details
- **Returns**: Detailed order information
- **Data**: Complete order data with items and payments

**Endpoint**: `POST /api/v1/user/order/cancel`
- **Purpose**: Cancel order
- **Returns**: Order cancellation status
- **Data**: Cancellation confirmation

**Endpoint**: `GET /api/v1/user/order/getPaymentMethod`
- **Purpose**: Get available payment methods
- **Returns**: Available payment options
- **Data**: Payment method list and configuration

**Endpoint**: `GET /api/v1/user/checkLogin`
- **Purpose**: Check login status
- **Returns**: Login status and permissions
- **Data**: Login status, admin privileges

**Endpoint**: `GET /api/v1/user/getStat`
- **Purpose**: Get user statistics
- **Returns**: User statistics summary
- **Data**: Pending orders count, open tickets count, invited users count

**Endpoint**: `GET /api/v1/user/getSubscribe`
- **Purpose**: Get subscription information
- **Returns**: Subscription details and URL
- **Data**: Plan details, subscription URL, usage stats, reset schedule

**Endpoint**: `POST /api/v1/user/transfer`
- **Purpose**: Transfer commission to balance
- **Returns**: Transfer status
- **Data**: Transfer confirmation and updated balances

**Endpoint**: `POST /api/v1/user/getQuickLoginUrl`
- **Purpose**: Generate quick login URL
- **Returns**: Quick login URL
- **Data**: Temporary login URL

### 💻 Session Management





### 💻 Session Management
**Endpoint**: `GET /api/v1/user/getActiveSession`
- **Purpose**: Get active sessions
- **Returns**: List of active sessions
- **Data**: Session details, login times, IP addresses

**Endpoint**: `POST /api/v1/user/removeActiveSession`
- **Purpose**: Remove specific session
- **Returns**: Session removal status
- **Data**: Session termination confirmation
- **Returns**: Payment method options
- **Data**: Available payment processors and methods

### 📧 Notifications
**Endpoint**: `GET /api/v1/user/notify/fetch`
- **Purpose**: Get user notifications
- **Returns**: Notification list
- **Data**: Notifications with timestamps and content

**Endpoint**: `POST /api/v1/user/notify/read`
- **Purpose**: Mark notification as read
- **Returns**: Read status confirmation
- **Data**: Notification read confirmation

**Endpoint**: `POST /api/v1/user/notify/clear`
- **Purpose**: Clear notifications
- **Returns**: Clear operation status
- **Data**: Notification clearance confirmation

### 🔗 Invites
**Endpoint**: `GET /api/v1/user/invite/fetch`
- **Purpose**: Get invite information
- **Returns**: Invite details and history
- **Data**: Invite codes, usage statistics, rewards

**Endpoint**: `POST /api/v1/user/invite/save`
- **Purpose**: Create new invite
- **Returns**: Invite creation status
- **Data**: New invite code and details

### 🎫 Tickets
**Endpoint**: `GET /api/v1/user/ticket/fetch`
- **Purpose**: Get support tickets
- **Returns**: Ticket list and status
- **Data**: Support tickets with messages and status

**Endpoint**: `POST /api/v1/user/ticket/save`
- **Purpose**: Create new ticket
- **Returns**: Ticket creation status
- **Data**: New ticket confirmation and details

**Endpoint**: `POST /api/v1/user/ticket/update`
- **Purpose**: Update ticket
- **Returns**: Ticket update status
- **Data**: Ticket update confirmation

**Endpoint**: `POST /api/v1/user/ticket/close`
- **Purpose**: Close ticket
- **Returns**: Ticket closure status
- **Data**: Ticket closure confirmation

## 💻 Client APIs

### 📋 Client Configuration
**Endpoint**: `GET /api/v1/client/config/fetch`
- **Purpose**: Get client configuration
- **Returns**: Client settings and parameters
- **Data**: Configuration for VPN client applications

**Endpoint**: `POST /api/v1/client/config/update`
- **Purpose**: Update client configuration
- **Returns**: Configuration update status
- **Data**: Updated configuration confirmation

### 🔌 Client Connection
**Endpoint**: `POST /api/v1/client/service/connect`
- **Purpose**: Establish connection
- **Returns**: Connection establishment status
- **Data**: Connection parameters and session info

**Endpoint**: `POST /api/v1/client/service/disconnect`
- **Purpose**: Terminate connection
- **Returns**: Disconnection status
- **Data**: Session termination confirmation

**Endpoint**: `GET /api/v1/client/service/status`
- **Purpose**: Get connection status
- **Returns**: Current connection state
- **Data**: Connection status and statistics

### 📊 Client Statistics
**Endpoint**: `POST /api/v1/client/stat/report`
- **Purpose**: Report usage statistics
- **Returns**: Statistics reception status
- **Data**: Usage data acknowledgment

**Endpoint**: `GET /api/v1/client/stat/getUsage`
- **Purpose**: Get usage statistics
- **Returns**: Usage data summary
- **Data**: Traffic consumption and session history

## 🖥️ Server APIs

### 📊 Server Statistics
**Endpoint**: `POST /api/v1/server/stat/report`
- **Purpose**: Report server statistics
- **Returns**: Statistics reception status
- **Data**: Server performance data acknowledgment

**Endpoint**: `GET /api/v1/server/stat/getUsage`
- **Purpose**: Get server usage statistics
- **Returns**: Server usage data
- **Data**: Server load, traffic, and performance metrics

### 🔧 Server Management
**Endpoint**: `POST /api/v1/server/manage/update`
- **Purpose**: Update server configuration
- **Returns**: Configuration update status
- **Data**: Server update confirmation

**Endpoint**: `GET /api/v1/server/manage/getConfig`
- **Purpose**: Get server configuration
- **Returns**: Server settings and parameters
- **Data**: Complete server configuration details

## 👔 Staff APIs

### 📊 Dashboard Statistics
**Endpoint**: `GET /api/v1/staff/dashboard/getStat`
- **Purpose**: Get dashboard statistics
- **Returns**: System overview metrics
- **Data**: User counts, order statistics, revenue data

**Endpoint**: `GET /api/v1/staff/dashboard/getServerStatus`
- **Purpose**: Get server status overview
- **Returns**: Server health information
- **Data**: Server status, performance metrics, alerts

### 👥 User Management
**Endpoint**: `GET/POST /api/v1/staff/user/fetch`
- **Purpose**: Get users with filtering
- **Returns**: User list with details
- **Data**: User information with search and filter capabilities

**Endpoint**: `POST /api/v1/staff/user/update`
- **Purpose**: Update user information
- **Returns**: User update status
- **Data**: User modification confirmation

**Endpoint**: `POST /api/v1/staff/user/delete`
- **Purpose**: Delete user account
- **Returns**: User deletion status
- **Data**: Account removal confirmation

### 🛒 Order Management
**Endpoint**: `GET/POST /api/v1/staff/order/fetch`
- **Purpose**: Get orders with filtering
- **Returns**: Order list with details
- **Data**: Order information with search and pagination

**Endpoint**: `POST /api/v1/staff/order/update`
- **Purpose**: Update order details
- **Returns**: Order update status
- **Data**: Order modification confirmation

**Endpoint**: `POST /api/v1/staff/order/delete`
- **Purpose**: Delete order
- **Returns**: Order deletion status
- **Data**: Order removal confirmation

### 📧 Notification Management
**Endpoint**: `GET/POST /api/v1/staff/notify/fetch`
- **Purpose**: Get system notifications
- **Returns**: Notification list
- **Data**: System notifications with management capabilities

**Endpoint**: `POST /api/v1/staff/notify/send`
- **Purpose**: Send system notification
- **Returns**: Notification sending status
- **Data**: Message delivery confirmation

## ⚙️ Admin APIs

### ⚙️ System Configuration
**Endpoint**: `GET /api/v2/{admin_path}/config/fetch`
- **Purpose**: Get system configuration
- **Returns**: All system settings
- **Data**: Complete configuration parameters

**Endpoint**: `POST /api/v2/{admin_path}/config/save`
- **Purpose**: Save system configuration
- **Returns**: Save status
- **Data**: Configuration save confirmation

**Endpoint**: `POST /api/v2/{admin_path}/config/setTelegramWebhook`
- **Purpose**: Configure Telegram webhook
- **Returns**: Webhook setup status
- **Data**: Telegram integration status

**Endpoint**: `POST /api/v2/{admin_path}/config/testSendMail`
- **Purpose**: Test email configuration
- **Returns**: Email test results
- **Data**: Mail delivery test confirmation

**Endpoint**: `GET /api/v2/{admin_path}/config/getThemeConfig`
- **Purpose**: Get theme configuration
- **Returns**: Theme settings
- **Data**: Available themes and settings

### 📋 Plan Management
**Endpoint**: `GET /api/v2/{admin_path}/plan/fetch`
- **Purpose**: Get all plans
- **Returns**: Plan list with details
- **Data**: All plans with user counts, revenue, admin settings

**Endpoint**: `POST /api/v2/{admin_path}/plan/save`
- **Purpose**: Create/update plan
- **Returns**: Plan save status
- **Data**: Plan creation/update confirmation

**Endpoint**: `POST /api/v2/{admin_path}/plan/drop`
- **Purpose**: Delete plan
- **Returns**: Delete status
- **Data**: Plan deletion confirmation

**Endpoint**: `POST /api/v2/{admin_path}/plan/update`
- **Purpose**: Update plan details
- **Returns**: Update status
- **Data**: Plan update confirmation

**Endpoint**: `POST /api/v2/{admin_path}/plan/sort`
- **Purpose**: Reorder plans
- **Returns**: Sort status
- **Data**: Plan ordering confirmation

### 🖥️ Server Management
**Endpoint**: `GET /api/v2/{admin_path}/server/group/fetch`
- **Purpose**: Get server groups
- **Returns**: Group list
- **Data**: Server groups with configuration

**Endpoint**: `POST /api/v2/{admin_path}/server/group/save`
- **Purpose**: Create/update server group
- **Returns**: Group save status
- **Data**: Group creation/update confirmation

**Endpoint**: `POST /api/v2/{admin_path}/server/group/drop`
- **Purpose**: Delete server group
- **Returns**: Delete status
- **Data**: Group deletion confirmation

**Endpoint**: `GET /api/v2/{admin_path}/server/route/fetch`
- **Purpose**: Get server routes
- **Returns**: Route list
- **Data**: Route configuration and rules

**Endpoint**: `POST /api/v2/{admin_path}/server/route/save`
- **Purpose**: Create/update server route
- **Returns**: Route save status
- **Data**: Route creation/update confirmation

**Endpoint**: `POST /api/v2/{admin_path}/server/route/drop`
- **Purpose**: Delete server route
- **Returns**: Delete status
- **Data**: Route deletion confirmation

**Endpoint**: `GET /api/v2/{admin_path}/server/manage/fetch`
- **Purpose**: Get servers
- **Returns**: Server list
- **Data**: Node details, status, configuration

**Endpoint**: `POST /api/v2/{admin_path}/server/manage/update`
- **Purpose**: Update server
- **Returns**: Update status
- **Data**: Server update confirmation

**Endpoint**: `POST /api/v2/{admin_path}/server/manage/save`
- **Purpose**: Create server
- **Returns**: Create status
- **Data**: Server creation confirmation

**Endpoint**: `POST /api/v2/{admin_path}/server/manage/drop`
- **Purpose**: Delete server
- **Returns**: Delete status
- **Data**: Server deletion confirmation

**Endpoint**: `POST /api/v2/{admin_path}/server/manage/copy`
- **Purpose**: Copy server configuration
- **Returns**: Copy status
- **Data**: Server copy confirmation

**Endpoint**: `POST /api/v2/{admin_path}/server/manage/sort`
- **Purpose**: Reorder servers
- **Returns**: Sort status
- **Data**: Server ordering confirmation

**Endpoint**: `POST /api/v2/{admin_path}/server/manage/restart`
- **Purpose**: Restart server
- **Returns**: Restart status
- **Data**: Server restart confirmation

### 📦 Order Management
**Endpoint**: `GET/POST /api/v2/{admin_path}/order/fetch`
- **Purpose**: Get orders with filtering
- **Returns**: Order list with details
- **Data**: Order details, user information, payment status

**Endpoint**: `POST /api/v2/{admin_path}/order/update`
- **Purpose**: Update order
- **Returns**: Order update status
- **Data**: Order update confirmation

**Endpoint**: `POST /api/v2/{admin_path}/order/assign`
- **Purpose**: Assign order to plan
- **Returns**: Assignment status
- **Data**: Order assignment confirmation

**Endpoint**: `POST /api/v2/{admin_path}/order/paid`
- **Purpose**: Mark order as paid
- **Returns**: Payment confirmation
- **Data**: Payment confirmation

**Endpoint**: `POST /api/v2/{admin_path}/order/cancel`
- **Purpose**: Cancel order
- **Returns**: Cancellation status
- **Data**: Order cancellation confirmation

**Endpoint**: `POST /api/v2/{admin_path}/order/detail`
- **Purpose**: Get order details
- **Returns**: Detailed order information
- **Data**: Complete order data with items

### 👥 User Management
**Endpoint**: `GET/POST /api/v2/{admin_path}/user/fetch`
- **Purpose**: Get users with filtering and pagination
- **Returns**: User list with details
- **Data**: User details, subscription information, usage statistics

**Endpoint**: `POST /api/v2/{admin_path}/user/update`
- **Purpose**: Update user
- **Returns**: User update status
- **Data**: User modification confirmation

**Endpoint**: `POST /api/v2/{admin_path}/user/detail`
- **Purpose**: Get user details
- **Returns**: Complete user information
- **Data**: Full user profile and activity

**Endpoint**: `POST /api/v2/{admin_path}/user/generate`
- **Purpose**: Generate user account
- **Returns**: Account creation status
- **Data**: New user account details

**Endpoint**: `POST /api/v2/{admin_path}/user/dumpCSV`
- **Purpose**: Export users to CSV
- **Returns**: CSV export status
- **Data**: CSV-formatted user data

**Endpoint**: `POST /api/v2/{admin_path}/user/sendMail`
- **Purpose**: Send email to users
- **Returns**: Email delivery status
- **Data**: Email delivery confirmation

**Endpoint**: `POST /api/v2/{admin_path}/user/ban`
- **Purpose**: Ban user
- **Returns**: Ban status
- **Data**: User ban confirmation

**Endpoint**: `POST /api/v2/{admin_path}/user/resetSecret`
- **Purpose**: Reset user secret
- **Returns**: Reset status
- **Data**: Secret reset confirmation

### 💳 Payment Management
**Endpoint**: `POST /api/v2/{admin_path}/payment/save`
- **Purpose**: Save payment method
- **Returns**: Save status
- **Data**: Payment method save confirmation

**Endpoint**: `POST /api/v2/{admin_path}/payment/drop`
- **Purpose**: Delete payment method
- **Returns**: Deletion status
- **Data**: Payment method deletion confirmation

**Endpoint**: `POST /api/v2/{admin_path}/payment/show`
- **Purpose**: Toggle payment method visibility
- **Returns**: Visibility status
- **Data**: Payment method visibility confirmation

**Endpoint**: `POST /api/v2/{admin_path}/payment/sort`
- **Purpose**: Reorder payment methods
- **Returns**: Sort status
- **Data**: Payment method ordering confirmation

### 📊 System Management
**Endpoint**: `GET /api/v2/{admin_path}/system/getSystemStatus`
- **Purpose**: Get system status
- **Returns**: System health information
- **Data**: Server status, performance metrics

**Endpoint**: `GET /api/v2/{admin_path}/system/getQueueStats`
- **Purpose**: Get queue statistics
- **Returns**: Queue performance data
- **Data**: Queue metrics and performance indicators

**Endpoint**: `GET /api/v2/{admin_path}/system/getQueueWorkload`
- **Purpose**: Get queue workload
- **Returns**: Queue workload information
- **Data**: Queue load and capacity metrics

**Endpoint**: `GET /api/v2/{admin_path}/system/getRedisStatus`
- **Purpose**: Get Redis status
- **Returns**: Redis health information
- **Data**: Redis performance and connection status

**Endpoint**: `GET /api/v2/{admin_path}/system/getServerMonitor`
- **Purpose**: Get server monitoring data
- **Returns**: Server monitoring information
- **Data**: Server performance and resource usage

**Endpoint**: `POST /api/v2/{admin_path}/system/update`
- **Purpose**: Update system settings
- **Returns**: Update status
- **Data**: System settings update confirmation

**Endpoint**: `POST /api/v2/{admin_path}/system/restart`
- **Purpose**: Restart system services
- **Returns**: Restart status
- **Data**: Service restart confirmation

**Endpoint**: `POST /api/v2/{admin_path}/system/clearCache`
- **Purpose**: Clear system cache
- **Returns**: Cache clearance status
- **Data**: Cache clearance confirmation

### 📝 Log Management
**Endpoint**: `GET /api/v2/{admin_path}/log/fetch`
- **Purpose**: Get system logs
- **Returns**: Log entries
- **Data**: System log data with filtering

**Endpoint**: `POST /api/v2/{admin_path}/log/clear`
- **Purpose**: Clear logs
- **Returns**: Log clearance status
- **Data**: Log clearance confirmation

### 📧 Email Management
**Endpoint**: `GET /api/v2/{admin_path}/email/fetch`
- **Purpose**: Get email templates
- **Returns**: Email template list
- **Data**: Email templates with content

**Endpoint**: `POST /api/v2/{admin_path}/email/save`
- **Purpose**: Save email template
- **Returns**: Template save status
- **Data**: Template save confirmation

**Endpoint**: `POST /api/v2/{admin_path}/email/test`
- **Purpose**: Test email template
- **Returns**: Test results
- **Data**: Email test confirmation

### 🤖 Telegram Management
**Endpoint**: `GET /api/v2/{admin_path}/telegram/fetch`
- **Purpose**: Get Telegram bot settings
- **Returns**: Telegram configuration
- **Data**: Bot settings and status

**Endpoint**: `POST /api/v2/{admin_path}/telegram/save`
- **Purpose**: Save Telegram settings
- **Returns**: Settings save status
- **Data**: Telegram configuration save confirmation

**Endpoint**: `POST /api/v2/{admin_path}/telegram/test`
- **Purpose**: Test Telegram integration
- **Returns**: Test results
- **Data**: Telegram test confirmation

### 🔐 Security Management
**Endpoint**: `GET /api/v2/{admin_path}/security/fetch`
- **Purpose**: Get security settings
- **Returns**: Security configuration
- **Data**: Security policies and settings

**Endpoint**: `POST /api/v2/{admin_path}/security/save`
- **Purpose**: Save security settings
- **Returns**: Settings save status
- **Data**: Security configuration save confirmation

**Endpoint**: `POST /api/v2/{admin_path}/security/scan`
- **Purpose**: Run security scan
- **Returns**: Scan results
- **Data**: Security scan report

### 📊 Analytics & Reports
**Endpoint**: `GET /api/v2/{admin_path}/analytics/getRevenue`
- **Purpose**: Get revenue analytics
- **Returns**: Revenue statistics
- **Data**: Financial reports and metrics

**Endpoint**: `GET /api/v2/{admin_path}/analytics/getUserGrowth`
- **Purpose**: Get user growth analytics
- **Returns**: User growth statistics
- **Data**: User acquisition and retention data

**Endpoint**: `GET /api/v2/{admin_path}/analytics/getServerPerformance`
- **Purpose**: Get server performance analytics
- **Returns**: Server performance statistics
- **Data**: Server metrics and performance indicators

**Endpoint**: `GET /api/v2/{admin_path}/analytics/getTrafficUsage`
- **Purpose**: Get traffic usage analytics
- **Returns**: Traffic usage statistics
- **Data**: Network traffic and consumption data