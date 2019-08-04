# 数据结构

## 集合

### 基本介绍

1. Scala同时支持不可变集合和可变集合，不可变集合可以安全的并发访问
2. 两主要的包：
   - 不可变集合：`scala.collection.immutable`
   - 可变集合：`scala.collection.mutable`
3. Scala默认采用不可变集合，对于所有的集合类，Scala都同时提供了可变和不可变的版本
4. Scala的集合有三大类：序列（Seq）、集（Set）、映射（Map），所有的集合都扩展自Iterable特质，在Scala中集合有可变和不可变两种