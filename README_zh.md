# Pot OpenAI OCR 插件

简体中文 | [English](README.md)

一款适用于 [Pot](https://github.com/pot-app/pot-desktop) 的 OCR（文字识别）插件，通过兼容 OpenAI Chat Completions 格式的 API 提取图片中的文字。

## 功能特点

- 支持识别 Pot 提供的所有语言。
- 可自由填写任何支持视觉输入的模型名称；留空时默认使用 `gpt-4o`。
- 支持 OpenAI 官方 API 和兼容 OpenAI 格式的自定义接口。
- 支持自定义 OCR Prompt，并可使用 `$lang` 语言占位符。
- 默认以高细节模式处理图片，并要求模型保留阅读顺序和换行。

## 安装

1. 下载或克隆本仓库。
2. 在 Pot 中打开 **偏好设置 → 服务设置 → 文字识别**。
3. 添加外部文字识别插件，选择本插件目录（如果下载的是发行版，也可以选择打包后的插件文件）。
4. 启用 **OpenAI OCR**，并按照下文完成配置。

> 不同版本的 Pot 可能使用不同的菜单名称。如果上述名称与实际界面不同，请在当前版本的插件管理界面中完成安装。

## 配置

| 配置项 | 必填 | 说明 |
| --- | --- | --- |
| **模型** | 否 | API 服务商提供的、支持视觉输入的模型。留空时默认使用 `gpt-4o`。 |
| **请求路径** | 否 | API 基础地址或 Chat Completions 完整地址。留空时使用 `https://api.openai.com`。 |
| **API Key** | 取决于服务商 | 以 Bearer Token 方式发送的 API 密钥；OpenAI 和大多数兼容服务商均要求填写。 |
| **自定义 Prompt** | 否 | 用于 OCR 的指令。留空时使用内置转录指令；其中的 `$lang` 会被替换为 Pot 当前选择的语言。 |

### 请求路径示例

插件会先规范化请求路径，再发送请求：

- `https://api.openai.com` → `https://api.openai.com/v1/chat/completions`
- `api.example.com/v1` → `https://api.example.com/v1/chat/completions`
- `https://api.example.com/v1/chat/completions` → 保持不变

接口需要支持包含图片输入的 OpenAI Chat Completions 请求，并在 `choices[0].message.content` 中返回识别结果。

## 自定义 Prompt

如果需要针对特定文档调整输出，可以填写 **自定义 Prompt**。Prompt 中可以使用 `$lang`，插件会将其替换为 Pot 当前选择的语言代码。例如：

```text
请按阅读顺序转录所有可见文字。预期语言为 $lang。只输出识别到的文字。
```

该配置留空时，插件会要求模型准确转录所有可见文字、保留原始阅读顺序和换行，并且不翻译、不解释，也不添加额外内容。

## 使用方法

1. 在 Pot 中选择 **OpenAI OCR** 作为文字识别服务。
2. 按照 Pot 常规的 OCR 操作方式截取屏幕区域或提供图片。
3. Pot 会显示所配置模型返回的文字。

## 隐私与安全

图片会被发送到 **请求路径** 中配置的 API 接口。处理敏感内容前，请先了解服务商的隐私和数据保留政策。请妥善保管 API Key，切勿将其提交到本仓库。

## 常见问题

- **认证失败：** 检查 API Key，并确认它有权访问所选模型。
- **模型报错：** 请使用支持通过 Chat Completions API 接收图片输入的模型。
- **404 或接口错误：** 检查请求路径。通常只需填写基础地址，插件会自动追加 `/v1/chat/completions`。
- **输出不符合预期：** 清空自定义 Prompt 以恢复默认设置，或在 Prompt 中明确要求只输出文字。

## 许可证

本项目采用 [MIT 许可证](LICENSE)。
