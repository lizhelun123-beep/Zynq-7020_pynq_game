---
name: pynq-dma-contract
description: 检查 DMA/VDMA 布局、缓存同步、缓冲所有权与安全退出，提供静态检查和离线生命周期示例。
---
# DMA 布局与所有权

1. 核对通道类型、像素格式、width/height、有效行字节、stride、span、地址范围、缓冲数与 cache 属性。
2. 按槽明确 CPU/device 读写、提交、完成、退休和复用顺序。设备仍持有的槽不能读写、复用或释放。
3. 设备写完成后同步再由 CPU 读；CPU 写完同步后再交设备。flush/invalidate 和内存屏障不能证明 DMA 完成。
4. 按实际驱动确认停机条件。stop 请求、PARK 变化或计数安静不能替代退休或排空依据；不能确认时继续保留所有权。
5. 未知地址域、SG、共享别名或缓存语义需要专门建模，不靠改地址绕过检查。

从技能包根目录执行：

```text
python -B pynq-dma-contract/scripts/check_layout.py pynq-dma-contract/examples/layout.json
python -B pynq-dma-contract/examples/lifecycle.py
```

检查器输入和退出码见 [布局格式](references/layout-schema.md)，行复制、提交异常与有界停机接口见 [生命周期接口](references/pynq-lifecycle.md)。例子只操作合成布局或 CPU 字节数组；真实分配器、同步 API 和设备停机须按目标平台核准。
