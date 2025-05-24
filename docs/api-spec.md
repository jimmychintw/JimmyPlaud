# API 規格書

## API 概述
### 版本資訊
- **API 版本**: v1.0.0
- **最後更新**: 2025-05-25
- **基礎 URL**: `https://api.example.com/v1`

### 認證方式
```
Authorization: Bearer {token}
```

### 通用回應格式
```json
{
  "success": true,
  "data": {},
  "message": "操作成功",
  "timestamp": "2025-05-25T12:00:00Z"
}
```

### 錯誤回應格式
```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "錯誤描述",
    "details": {}
  },
  "timestamp": "2025-05-25T12:00:00Z"
}
```

### HTTP 狀態碼
| 狀態碼 | 說明 |
|-------|------|
| 200 | 成功 |
| 201 | 建立成功 |
| 400 | 請求參數錯誤 |
| 401 | 未授權 |
| 403 | 禁止存取 |
| 404 | 資源不存在 |
| 500 | 伺服器錯誤 |

## API 端點

### 使用者管理

#### 1. 使用者註冊
```
POST /users/register
```

**請求參數**
```json
{
  "username": "string",
  "email": "string",
  "password": "string"
}
```

**回應範例**
```json
{
  "success": true,
  "data": {
    "userId": "12345",
    "username": "user123",
    "email": "user@example.com",
    "createdAt": "2025-05-25T12:00:00Z"
  },
  "message": "註冊成功"
}
```

#### 2. 使用者登入
```
POST /users/login
```

**請求參數**
```json
{
  "email": "string",
  "password": "string"
}
```

**回應範例**
```json
{
  "success": true,
  "data": {
    "token": "jwt_token_here",
    "refreshToken": "refresh_token_here",
    "expiresIn": 3600,
    "user": {
      "userId": "12345",
      "username": "user123",
      "email": "user@example.com"
    }
  },
  "message": "登入成功"
}
```

#### 3. 取得使用者資訊
```
GET /users/{userId}
```

**路徑參數**
- `userId` (string, required): 使用者 ID

**回應範例**
```json
{
  "success": true,
  "data": {
    "userId": "12345",
    "username": "user123",
    "email": "user@example.com",
    "profile": {
      "firstName": "John",
      "lastName": "Doe",
      "avatar": "https://example.com/avatar.jpg"
    },
    "createdAt": "2025-05-25T12:00:00Z",
    "updatedAt": "2025-05-25T12:00:00Z"
  }
}
```

#### 4. 更新使用者資訊
```
PUT /users/{userId}
```

**路徑參數**
- `userId` (string, required): 使用者 ID

**請求參數**
```json
{
  "username": "string",
  "profile": {
    "firstName": "string",
    "lastName": "string",
    "avatar": "string"
  }
}
```

### 資源管理

#### 1. 取得資源列表
```
GET /resources
```

**查詢參數**
- `page` (integer, optional): 頁碼，預設為 1
- `limit` (integer, optional): 每頁筆數，預設為 20
- `sort` (string, optional): 排序欄位
- `order` (string, optional): 排序方向 (asc/desc)
- `filter` (string, optional): 篩選條件

**回應範例**
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "1",
        "name": "資源名稱",
        "description": "資源描述",
        "status": "active",
        "createdAt": "2025-05-25T12:00:00Z"
      }
    ],
    "pagination": {
      "currentPage": 1,
      "totalPages": 10,
      "totalItems": 200,
      "itemsPerPage": 20
    }
  }
}
```

#### 2. 建立資源
```
POST /resources
```

**請求參數**
```json
{
  "name": "string",
  "description": "string",
  "type": "string",
  "metadata": {}
}
```

#### 3. 更新資源
```
PUT /resources/{resourceId}
```

**路徑參數**
- `resourceId` (string, required): 資源 ID

**請求參數**
```json
{
  "name": "string",
  "description": "string",
  "status": "string",
  "metadata": {}
}
```

#### 4. 刪除資源
```
DELETE /resources/{resourceId}
```

**路徑參數**
- `resourceId` (string, required): 資源 ID

### Webhook 設定

#### 1. 註冊 Webhook
```
POST /webhooks
```

**請求參數**
```json
{
  "url": "https://example.com/webhook",
  "events": ["user.created", "resource.updated"],
  "secret": "webhook_secret"
}
```

### 批次操作

#### 1. 批次建立
```
POST /batch/create
```

**請求參數**
```json
{
  "resources": [
    {
      "name": "資源1",
      "type": "type1"
    },
    {
      "name": "資源2",
      "type": "type2"
    }
  ]
}
```

## 資料模型

### User 物件
```typescript
interface User {
  userId: string;
  username: string;
  email: string;
  profile: UserProfile;
  status: 'active' | 'inactive' | 'suspended';
  createdAt: string;
  updatedAt: string;
}

interface UserProfile {
  firstName: string;
  lastName: string;
  avatar: string;
  phone?: string;
  address?: Address;
}
```

### Resource 物件
```typescript
interface Resource {
  id: string;
  name: string;
  description: string;
  type: string;
  status: 'active' | 'inactive' | 'deleted';
  metadata: Record<string, any>;
  createdBy: string;
  createdAt: string;
  updatedAt: string;
}
```

## 錯誤碼對照表

| 錯誤碼 | 說明 | HTTP 狀態碼 |
|-------|------|------------|
| AUTH_001 | 認證失敗 | 401 |
| AUTH_002 | Token 過期 | 401 |
| AUTH_003 | 權限不足 | 403 |
| USER_001 | 使用者不存在 | 404 |
| USER_002 | Email 已被使用 | 400 |
| RESOURCE_001 | 資源不存在 | 404 |
| RESOURCE_002 | 資源建立失敗 | 400 |
| VALIDATION_001 | 參數驗證失敗 | 400 |
| SERVER_001 | 內部伺服器錯誤 | 500 |

## Rate Limiting
- **限制**: 每分鐘 60 次請求
- **標頭資訊**:
  - `X-RateLimit-Limit`: 限制數量
  - `X-RateLimit-Remaining`: 剩餘次數
  - `X-RateLimit-Reset`: 重置時間

## 安全性考量
1. 所有 API 請求必須使用 HTTPS
2. 敏感資料必須加密傳輸
3. 實施 CORS 政策
4. 輸入驗證與消毒
5. SQL 注入防護
6. XSS 防護

## 版本控制
- API 版本透過 URL 路徑指定 (e.g., `/v1/`, `/v2/`)
- 舊版本支援期限：6 個月
- 版本棄用通知：提前 3 個月

## 測試環境
- **測試環境 URL**: `https://api-test.example.com/v1`
- **測試帳號**: 請聯繫開發團隊取得

## 聯絡資訊
- **技術支援**: support@example.com
- **API 狀態頁面**: https://status.example.com
- **開發者文件**: https://docs.example.com