+++
title = 'Dpdk Traces'
date = 2024-11-01T14:43:42+08:00
draft = false
+++

## DPDK Trace 示例

用头文件（mytp.h）定义 trace points：
```C
#include <rte_trace_point.h>

RTE_TRACE_POINT(
    app_trace_string,
    RTE_TRACE_POINT_ARGS(const char *str),
    rte_trace_point_emit_string(str);
)

RTE_TRACE_POINT(
    app_trace_u32,
    RTE_TRACE_POINT_ARGS(unsigned int id),
    rte_trace_point_emit_u32(id);
)
```

在一个单独的源文件里（mytp.c）注册 trace points:
```C
#include <rte_trace_point_register.h>
#include <mytp.h>

RTE_TRACE_POINT_REGISTER(app_trace_string, app.trace.string)
RTE_TRACE_POINT_REGISTER(app_trace_u32, app.trace.u32)
```

在需要的时候激活：
```C
	rte_trace_dump(stderr);
	app_trace_string("Hello from trace point");
	app_trace_u32(1);
```

根据测试结果(23.11)，注册代码需单独放在一个文件里，否则无法输出追踪信息，原因不明。
这里是一个相关问题的参考链接：[Unable to see traces from a user defined DPDK Tracepoint](https://mails.dpdk.org/archives/users/2023-October/007428.html)。