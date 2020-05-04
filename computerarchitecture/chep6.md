# 06.Pipelining & Hazards

## 复习：流水线的基本思想

- More systematically:
  - Pipelining the execution of multiple instructions
- Idea:
  - `Divide the instruction processing cycle into distinct “stages” of processing`
  - Ensure there are enough hardware resources to process   one instruction in each stage
  - `Process a different instruction in each stage simultaneously`
    - Instructions consecutive in program order are processed in consecutive stages
- Benefit: Increases instruction processing throughput

## 示例：加法操作的处理












