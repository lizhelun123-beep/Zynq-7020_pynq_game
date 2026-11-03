# DMA 布局与安全退出技能

本提交只保留 [pynq-dma-contract/skill.md](pynq-dma-contract/skill.md) 一项技能，满足指南 3.3.5.2。它来自采集/显示缓冲交接、提交异常和停止时保留所有权的经验，可用于其他图像或流式工程，不依赖本工程相机、物块、JSON 或板卡型号。

在本目录执行：

```text
python -B pynq-dma-contract/scripts/check_layout.py pynq-dma-contract/examples/layout.json
python -B pynq-dma-contract/examples/lifecycle.py
python -B -m unittest discover -s pynq-dma-contract/scripts -p "test_*.py" -v
```

已验证：18 项主机检查通过，覆盖重叠、跨度不足、复制 padding、部分提交异常、停止超时和错误状态不得释放。布局例子输出 PASS_STATIC_DESCRIPTION，生命周期例子输出 MOCK_LIFECYCLE_ONLY。仅验证静态描述与 CPU 模型。

来源观察：命令报错不代表设备没有开始读缓冲；PARK、帧指针变化和 flush 不能单独证明退休；停止请求不能替代实际停机确认。运行层另有7项故障注入检查，验证持续显示故障的期限和停止响应，见 [运行层检查](../src/project/pynq_vision/tests/test_display_retry.py)。

失效条件：未知地址域、缓存语义、SG/共享别名或阻塞设备 API 必须另作平台适配。模型通过不证明真实 DMA 已退休或停机。字段见 [布局格式](pynq-dma-contract/references/layout-schema.md)，接口边界见 [生命周期接口](pynq-dma-contract/references/pynq-lifecycle.md)。
