# CPG 代码阅读顺序

## 第一天：核心概念
1. `cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/graph/Node.kt` - 所有节点的基类
2. `cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/graph/declarations/Declaration.kt` - 声明节点
3. `cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/graph/statements/Statement.kt` - 语句节点

## 第二天：前端架构
4. `cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/frontends/LanguageFrontend.kt`
5. `cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/frontends/Handler.kt`
6. `cpg-language-java/src/main/kotlin/de/fraunhofer/aisec/cpg/frontends/java/JavaLanguageFrontend.kt`

## 第三天：语义分析
7. `cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/passes/Pass.kt` - Pass基类
8. `cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/passes/SymbolResolver.kt` - 符号解析
9. `cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/passes/TypeResolver.kt` - 类型解析

## 第四天：作用域和类型
10. `cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/graph/scopes/Scope.kt`
11. `cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/graph/types/Type.kt`

## 第五天：数据流和控制流
12. `cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/passes/EvaluationOrderGraphPass.kt`
13. `cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/passes/DFGPass.kt`

## 实践：运行示例
```bash
# 运行一个简单的测试
cd /home/user/cpg
./gradlew :cpg-language-java:test --tests "*JavaLanguageFrontendTest*"
```
