
``` mermaid
flowchart TD
A[Christmas]
-->
id1{Some text}
-->
id2{Some text}
-->
B(Some text)
-->
C{CHAKSDLK}
-->
D[SASDD]


```
``` mermaid
stateDiagram-v2
        state fork_state <<fork>>
          [*] --> fork_state
          fork_state --> State2
          fork_state --> State3
    
          state join_state <<join>>
          State2 --> join_state
          State3 --> join_state
          join_state --> State4
          State4 --> [*]
```
``` mermaid
flowchart TD

```
``` mermaid
flowchart TB
A(GSM启动拉低EN脚500ms以上)==>B[MCU向GSM发送AT500ms一次共10次]==>C{模块回复}--ERROR-->A
C--OK-->D[通过AT+CPIN查询SIM卡状态等待回复]
```




``` mermaid
flowchart TB
A(GSM模块启动)-->B[每500ms发送]
```
``` mermaid
graph LR
A-- text -->B
```


