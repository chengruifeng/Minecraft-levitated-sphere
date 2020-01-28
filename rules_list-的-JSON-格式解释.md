**该文件提供云端仓库的索引**

# JSON 内容示意

```json
{
  "list_url": {
    "app_list_raw_url": "https://raw.githubusercontent.com/xz-dev/UpgradeAll-rules/dev/rules/apps/",
    "hub_list_raw_url": "https://raw.githubusercontent.com/xz-dev/UpgradeAll-rules/dev/rules/hub/"
  },
  "app_list": [
    {
      "app_config_name": "UpgradeAll",
      "app_config_uuid": "f27f71e1-d7a1-4fd1-bbcc-9744380611a1",
      "app_config_file_name": "UpgradeAll"
    },
    {
      "app_config_name": "MiPushFramework",
      "app_config_uuid": "f7b2d588-38ae-47d7-a2e8-fb2d6c2e9d51",
      "app_config_file_name": "MiPushFramework"
    },
    {
      "app_config_name": "[Magisk_Module]Riru Core",
      "app_config_uuid": "bef9e572-32fd-11ea-a82d-f108c61ac67d",
      "app_config_file_name": "[Magisk_Module]Riru Core"
    }
  ],
  "hub_list": [
    {
      "hub_config_name": "GitHub",
      "hub_config_uuid": "fd9b2602-62c5-4d55-bd1e-0d6537714ca0",
      "hub_config_file_name": "GitHub"
    },
    {
      "hub_config_name": "Z直播",
      "hub_config_uuid": "28591e65-849f-4417-aead-9d9098520eed",
      "hub_config_file_name": "zlive"
    },
    {
      "hub_config_name": "酷安",
      "hub_config_uuid": "1c010cc9-cff8-4461-8993-a86cd190d377",
      "hub_config_file_name": "CoolApk"
    }
  ]
}
```

# 节点解释

## list_url

用于指定软件获取云端配置时使用的分支  
软件内部会根据其下的 app_list_raw_url 和 hub_list_raw_url，以 Github 的规则组合成 **raw 地址**

- 如果你希望直接提交配置/代码，不需要修改该字段
- 如果你希望 Fork 该配置仓库并使用自己的仓库，请修改配置以指向你自己的仓库

### app_list_raw_url

用于为软件提供获取 **App**（跟踪项）的配置存放地址

### hub_list_raw_url

用于为软件提供获取**软件源**的配置存放地址

## app_list

该节点下包含所有收录的跟踪项

### 跟踪项格式

```text
{
  "app_config_name": "",
  "app_config_uuid": "",
  "app_config_file_name": ""
}
```

- app_config_name: 跟踪项名称（一般应该与跟踪项配置文件内的名称保持一致）
- app_config_uuid: 跟踪项配置唯一识别码(UUID v4，使用 [Online UUID Generator](https://www.uuidgenerator.net) 生成)
- app_config_file_name: 跟踪项配置文件的文件名（软件会根据该文件名在 rules/apps 目录下寻找同名配置文件）

## hub_list

该节点下包含所有收录的软件源

### 软件源格式

```text
{
  "hub_config_name": "",
  "hub_config_uuid": "",
  "hub_config_file_name": ""
}
```

- hub_config_name: 软件源名称（一般应该与软件源配置文件内的名称保持一致）
- hub_config_uuid: 软件源配置唯一识别码(UUID v4，使用 [Online UUID Generator](https://www.uuidgenerator.net) 生成)
- hub_config_file_name: 软件源配置文件的文件名（软件会根据该文件名在 rules/hub 目录下寻找同名配置文件）
