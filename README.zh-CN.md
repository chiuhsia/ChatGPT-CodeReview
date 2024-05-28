# 代码审查机器人

> 由 ChatGPT 提供支持的代码审查机器人

翻译版本：[英语](./README.md)\|[简体中文](./README.zh-CN.md)\|[繁体中文](./README.zh-TW.md)\|[韩国人](./README.ko.md)\|[日本人](./README.ja.md)

## 机器人使用情况

❗️⚠️`Due to cost considerations, BOT is only used for testing purposes and is currently deployed on AWS Lambda with ratelimit restrictions. Therefore, unstable situations are completely normal. It is recommended to deploy an app by yourself.`

### 安装

安装：[应用程序/cr-gpt](https://github.com/apps/cr-gpt);

### 配置

1.  转到您想要集成此机器人的存储库主页
2.  点击`settings`
3.  点击`actions`在下面`secrets and variables`
4.  改成`Variables`选项卡，创建一个新变量`OPENAI_API_KEY`使用您的开放 api 密钥的值（对于 Github Action 集成，请将其设置为 Secret）<img width="1465" alt="image" src="https://user-images.githubusercontent.com/13167934/218533628-3974b70f-c423-44b0-b096-d1ec2ace85ea.png">

### 开始使用

1.  当您创建新的 Pull 请求时，机器人会自动进行代码审查，审查信息将显示在 pr 时间线/文件更改部分。
2.  后`git push`更新pull request，cr bot将重新审核更改的文件

例子：

[ChatGPT-CodeReview/pull/21](https://github.com/anc95/ChatGPT-CodeReview/pull/21)

<img width="1052" alt="image" src="https://user-images.githubusercontent.com/13167934/218999459-812206e1-d8d2-4900-8ce8-19b5b6e1f5cb.png">

## 使用 Github 操作

[操作/chatgpt-codereviewer](https://github.com/marketplace/actions/chatgpt-codereviewer)

1.  添加`OPENAI_API_KEY`您的 github 操作秘密
2.  创造`.github/workflows/cr.yml`添加以下内容

```yml
name: Code Review

permissions:
  contents: read
  pull-requests: write

on:
  pull_request:
    types: [opened, reopened, synchronize]

jobs:
  test:
    # if: ${{ contains(github.event.*.labels.*.name, 'gpt review') }} # Optional; to run only when a label is attached
    runs-on: ubuntu-latest
    steps:
      - uses: anc95/ChatGPT-CodeReview@main
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
          # Optional
          LANGUAGE: Chinese
          OPENAI_API_ENDPOINT: https://api.openai.com/v1
          MODEL: gpt-3.5-turbo # https://platform.openai.com/docs/models
          PROMPT: # example: Please check if there are any confusions or irregularities in the following code diff:
          top_p: 1 # https://platform.openai.com/docs/api-reference/chat/create#chat/create-top_p
          temperature: 1 # https://platform.openai.com/docs/api-reference/chat/create#chat/create-temperature
          max_tokens: 10000
          MAX_PATCH_LENGTH: 10000 # if the patch/diff length is large than MAX_PATCH_LENGTH, will be ignored and won't review. By default, with no MAX_PATCH_LENGTH set, there is also no limit for the patch/diff length.
```

## 自托管

1.  克隆代码
2.  复制`.env.example`到`.env`，并填充环境变量
3.  安装 deps 并运行

```sh
npm i
npm i -g pm2
npm run build
pm2 start pm2.config.cjs
```

[机器人](https://probot.github.io/docs/development/)了解更多详情

## 开发者

### 设置

```sh
# Install dependencies
npm install

# Build code
npm run build

# Run the bot
npm run start
```

### 码头工人

```sh
# 1. Build container
docker build -t cr-bot .

# 2. Start container
docker run -e APP_ID=<app-id> -e PRIVATE_KEY=<pem-value> cr-bot
```

## 贡献

如果您对如何改进 cr-bot 有建议，或者想要报告错误，请提出问题！我们很乐意接受所有的贡献。

欲了解更多信息，请查看[贡献指南](CONTRIBUTING.md).

## 信用

这个项目的灵感来自[代码审查.gpt](https://github.com/sturdy-dev/codereview.gpt)

## 执照

[国际标准委员会](LICENSE)© 2023 ANC95
