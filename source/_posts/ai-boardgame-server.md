---
title: ai-boardgame-server 项目结构 (持续更新中)
date: 2024-12-03 22:45:47
tags:
  - Work
  - Backend
categories:
  - Work
top_img: /img/boardgame.jpg
cover: /img/boardgame.jpg
---

## 前言

这是第一次接触到上线产品级别的项目———— `ai-boardgame`。 在这个项目中，AI 将扮演 DM 的角色帮助玩家推进剧情，其具体功能包括根据玩家的输入创建新剧情，语音输出，剧情相关的背景图片生成等等。

后端项目基于 `Python` 和 `FastAPI`。FastAPI 是一个用于构建 API 的现代、高性能、基于 Python 3.6+ 的 Web 框架。它以其易用性、速度和功能强大的特性迅速受到开发者的喜爱，特别适合构建 RESTful API 或异步应用程序。

作为一个对后端只了解一些皮毛的实习生，这是一个很好的学习机会。本文将解析整个 `ai-boardgame-server` 的项目结构（后端），为之后的学习和工作打下基础。

## 完整的项目目录结构

```plaintext
<项目根目录>/
├── .devcontainer/
│   ├── devcontainer.json
│   ├── docker-compose.yml
│   ├── Dockerfile
│   ├── postCreateCommand.sh
├── .github/
│   ├── workflows/
│       ├── deploy.yml
│   ├── dependabot.yml
├── app/
│   ├── cache/
│       ├── game_cache.py
│   ├── game/
│       ├── context.py
│       ├── model.py
│       ├── session.py
│       ├── worker.py
│   ├── image
│       ├── image_generate_template.py
│   ├── prompt
│       ├── game_prompt.py
│   ├── repository
│       ├── database.py
│       ├── game_dao.py
│       ├── order_dao.py
│       ├── resource_dao.py
│   ├── router
│       ├── asr_router.py
│       ├── game_router.py
│       ├── order_router.py
│       ├── status_router.py
│   ├── schema
│       ├── asr_schema.py
│       ├── base.py
│       ├── game_schema.py
│       ├── order.py
│       ├── resource.py
│   ├── service
│       ├── asr_service.py
│       ├── llm_service.py
│       ├── tts_stream_service.py
│       ├── txt2img_service_flux_api.py
│       ├── txt2img_service.py
│   ├── util
│       ├── constants.py
│       ├── resource_utils.py
│   ├── app_logging.py
│   ├── dependencies.py
│   ├── exception_handlers.py
│   ├── lifetime.py
│   ├── main.py
│   ├── settiings.py
├── tests/
│   ├── test_stream_tts.py
│   ├── test_game_auto.py
├── .env
├── .gitignore
├── ai-boardgame-dev.env
├── docker-compose.yml
├── Dockerfile
├── manifest.metadata
├── nohup.out
├── README.md
├── requirements.txt
└── run.sh
```

## 具体说明（`app/`中的主要目录）

`app/` 目录是整个项目的主要目录，包含了许多子目录和文件。所有主要功能都在这个目录下实现。

### cache/

定义有关游戏缓存的函数。这些函数通过 Redis 来存储和管理游戏的各种状态和信息，确保游戏数据的持久性和一致性。通过这些函数，游戏的状态、历史记录、角色信息等都可以被有效地管理和更新。

### game/

- **context.py**: 定义了一个名为 `Context` 的类。 构造函数将 `self.websocket` 和 `self.gid` 这两个参数赋值给实例变量。这个类的设计用于在游戏应用中管理每个用户的 WebSocket 连接和相关的游戏信息。
- **model.py**:
  - 定义了 `MessageType` ，它是一个继承自 `str` 和 `Enum` 的枚举类，定义了四种消息类型：`AUDIO_MESSAGE`、`IMAGE_MESSAGE`、`TEXT_MESSAGE` 和 `EVENT_MESSAGE`。
  - 定义了 `ClientGameMessage` 和 `ServerGameMessage` 类，用于表示客户端和服务器发送的游戏消息。它们包含三个字段: `type`, `timestamp`, `data`。`to_dict` 方法用于将数据类实例转换为字典格式，便于序列化或传输。
- **session.py**:
  - 定义了一个名为 `Session` 的类，此类维护每个连接客户端的状态，包括其唯一标识符、音频缓冲区、配置和已处理音频文件的计数器。
    - **init**: 初始化 `Session` 对象，设置 websocket 连接和游戏 ID。
    - **run**: 异步运行会话，接受 WebSocket 连接，处理消息，管理连接状态。接收消息并将其包装为 `ClientGameMessage` 对象，然后放入 `client_events_queue`。处理 WebSocket 断开连接和其他异常。
    - **close**: 关闭 WebSocket 连接，释放资源。
- **worker.py**:
  - 定义了一个游戏服务器的工作者 `Worker` 类，负责处理游戏中的各种任务和事件。
    - **\_call_game_world_creator_agent_with_history**: 调用游戏世界创建代理，根据玩家输入的设定生成新的游戏世界。
    - **\_call_game_world_simulator_agent_with_history**: 调用游戏世界模拟器代理，用于生成新的游戏剧情。
    - **\_call_game_world_referee_agent_with_history**: 调用游戏世界裁判代理，用于判断游戏是否结束。
    - **\_generate_scene_image**: 根据当前回合剧情生成背景图片。

### image/

定义了两个图像生成器 `MiaohuaImageGenerator` 和 `FluxImageGenerator`。目前使用的模型为 `Flux`。

- **\_generate_request**: 根据输入参数生成图像请求。
- **generate**: 根据请求调用 API 生成图像。

### prompt/

定义了一个名为 `GamePromptFactory` 的提示词工厂类，用于生成和 LLM 交互的游戏提示词。它主要用于管理和生成游戏中的各种系统提示和用户消息。

- 定义各种提示词，包括生成游戏世界，模拟器，裁判。
- **build_game_world_simulator_agent_history**: 用于构建游戏世界模拟器的代理历史记录。它根据当前用户输入、游戏世界模拟器历史和随机影响生成新的游戏剧情。
- **build_game_world_referee_agent_history**: 用于判断游戏是否结束。它根据游戏世界模拟器历史和胜利条件决定游戏是否结束。

### repository/

- **database.py**: 用于连接 MongoDB 数据库，使用了`Motor`库来实现异步的 MongoDB 客户端。
- **game_dao.py**, **order_dao.py**, **resource_dao.py**: 用于管理相关信息的 CRUD（创建、读取、更新、删除）操作。

### router/

定义了各种 API，用于处理不同类型的请求。

一个简单的例子:

```python
@router.get("/keys/count")
async def get_redis_key_count():
    async with get_redis() as redis:
        key_count = await redis.dbsize()
        return key_count
```

### schema/

定义了数据模型，通常用于 API 的请求和响应数据验证。在 `FastAPI` 中，`Pydantic` 是一个用于数据验证和设置的库，它通过定义数据模型来确保数据的完整性和正确性。

### service/

应用程序的服务层，负责处理业务逻辑和与外部服务的交互。每个文件通常对应一个特定的功能模块。

- **asr_service.py**: 用于处理自动语音识别（ASR）相关功能。
- **llm_service.py**: 用于与 LLM 交互。
- **tts_stream_service.py**: 用于文本到语音的流式转换。
- **txt2img_service_flux_api.py**, **txt2img_service.py**: 用于文本生成图像。

### util/

- **constants.py**: 定义了各种常量。
- **resource_utils.py**: 用于处理文件上传和下载，主要功能包括将文件上传到 S3 存储、从 URL 下载图像，以及将图像上传到 Musisoul 服务。

## 运行项目

- 安装 VSCode 插件`Dev Containers`。
- 进入 Docker 容器：`Ctrl + Shift + P`，输入`Dev Containers: Reopen in Container`。
- 如果添加了新的依赖，需要运行以下指令来刷新依赖列表：

```bash
pip freeze > requirements.txt
```

- 运行项目：

```bash
./run.sh start
```

- 重新开始:

```bash
lsof -i:8080
kill -9 <PID>
./run.sh start
```
