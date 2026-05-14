# 核心数据类型

> 完整字段定义（类型、枚举、必填）见 `openapi.yaml`，本文件提供人类可读概览，
> 重点说明 AI 容易混淆的字段含义和注意事项。

---

## ImageObject — 图片上传格式

用于所有写接口的图片字段（`create_card` / `update_card`）。

```json
{
  "filename": "avatar.jpg",
  "content_type": "image/jpeg",
  "content": "/9j/4AAQ..."
}
```

- `content`：base64 编码，**不含** `data:image/jpeg;base64,` 前缀
- 单张字段（`cover`、`target_logo`）传对象；多张字段（`intro_images`、`work_wechat_images` 等）传数组
- `target_logo` 也可直接传 `search_target` 返回的 URL 字符串（复用已有 logo 时）

---

## CardInfo — 我的名片（`get_my_card` 返回）

| 字段 | 说明 |
|------|------|
| `card_slug` | 内部标识，不展示，传给 `update_card` 的 `slug` 路径参数 |
| `name` | 姓名 |
| `avatar` | 头像图片 URL |
| `position` | 职位 |
| `company` | 公司 |
| `mobile` | 手机号 |
| `wechat` | 微信号 |
| `email` | 邮箱 |
| `address` | 地址 |
| `intro` | 个人简介 |
| `introImages` | 介绍图 URL 列表 |
| `status` | 审核状态：`none` / `pending` / `approved` / `rejected` |
| `authExpiredTime` | 认证到期日，未生效时为空字符串 |
| `authFailReason` | 最近审核失败原因，未失败时为空字符串 |
| `showStatus` | 展示状态 |
| `viewCondition` | 查看门槛 |

---

## CardSearchItem — 名片搜索结果（`search_cards` 返回）

| 字段 | 说明 |
|------|------|
| `slug` | 内部标识，不展示，传给 `exchange_card` 的 `card_slug` 参数 |
| `name` | 姓名 |
| `cover` | 头像图片 URL |
| `position` | 职位 |
| `intro` | 简介 |
| `targetType` | 身份类型：1 供应商 / 2 品牌方 / 3 服务商 / 4 加盟商 |
| `targetName` | 关联品牌/机构名称 |
| `isAuth` | 是否已认证 |
| `isMine` | 是否是自己的名片 |
| `mobile` | **有值时**直接展示联系方式，无需投递；空字符串时需投递后才能获取 |
| `wechat` | 微信号，可见规则同 `mobile` |
| `sendStatus` | `0` 表示已可查看联系方式（已交换 / 已收到分享 / 对方已投递） |
| `tagList` | 标签列表，每项含 `label` 和 `type` |
| `latestVisitDay` | 最近活跃日期，可为 null |

---

## ReceivedRequest — 收到的名片交换请求（`get_received_cards` 返回）

| 字段 | 说明 |
|------|------|
| `request_id` | 内部标识，不展示，传给 `respond_to_request`；**空字符串**表示纯分享名片，不可调 `respond_to_request` |
| `status` | `pending` / `accepted` / `rejected` |
| `reason` | 对方附言 |
| `created_at` | 请求时间 |
| `from_user.name` | 发送方姓名 |
| `from_user.position` | 职位 |
| `from_user.company` | 公司 |
| `from_user.avatar` | 头像图片 URL |
| `from_user.mobile` | 手机号，收到投递时**始终返回**，直接展示 |
| `from_user.wechat` | 微信号，若对方填写则返回 |
| `from_user.email` | 邮箱，若对方填写则返回 |

---

## DeliveredRequest — 我投递的名片请求（`get_delivered_cards` 返回）

| 字段 | 说明 |
|------|------|
| `request_id` | 内部标识，不展示 |
| `status` | `pending` / `accepted` / `rejected` |
| `to_user.name` | 对方姓名 |
| `to_user.position` | 职位 |
| `to_user.company` | 公司 |
| `to_user.mobile` | `status=accepted` 时必然有值，直接展示；`pending` / `rejected` 时为 null |
| `to_user.wechat` | `status=accepted` 且对方填写时返回 |
| `to_user.email` | `status=accepted` 且对方填写时返回 |

---

## TargetItem — 可关联企业（`search_target` 返回）

| 字段 | 说明 |
|------|------|
| `targetSlug` | 内部标识，不展示，传给 `create_card` 的 `target_slug` 参数 |
| `targetType` | 企业类型：1 供应商 / 2 品牌方 / 3 服务商 / 4 加盟商 |
| `targetName` | 企业名称，展示给用户确认 |
| `targetLogo` | 企业 logo URL，可直接传给 `create_card` 的 `target_logo` 字段复用 |
