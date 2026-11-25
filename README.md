# 直播控制委托的多方确认与指令交付

使用合成账号和节目分支。逻辑秒点表示到达，media 为节目帧位置。运营端的说明注明哪段可应用。

## 输入消息

每个 JSONL 文件是一段独立输入记录。每行包括整数秒点 at、消息名称 kind 与对象 record。at 代表中间件收到消息的时刻，同一秒内按行序提交。业务时间和对应回执引用写在 record 中。

- **grant**　节目主管将 grant 分配给 person。program、branch、actions、span 和 until 为范围，parent 可指代上级委托。 until 为逻辑秒点上界，span 含起点不含末点。
- **submit**　person 提交 command、rev、grant、program、branch、action、span 和 params。
- **approve**　reviewer 出示自己的 review_grant，确认 command 的 rev。
- **position**　导播报告节目分支当前 media。
- **dispatch**　要求将指定 command/rev 交给当前执行端，由网关判断是否可派。
- **accept**　导播执行端报告指定 rev 已接受，receipt 唯一。
- **revoke**　主管撤回指定 grant。
- **outcome**　执行端按 receipt 回报 applied、stopped 或 unknown。applied 含实际 media。
- **restart**　仿真网关重启。
- **inspect**　按 command 查询。

## 示例

- revocation-after-executor-acceptance.jsonl
- approval-not-carried-to-edited-command.jsonl

## 读取演练资料

运行环境为 Python 3.11，只使用标准库。在根目录运行以下命令可核对文件格式及读取接口。

    python3 scenario_reader.py examples
    python3 -m unittest discover -v

资料读取器会按文件统计各类参与方消息的数量。演练记录只描述各参与方交给系统的输入。
