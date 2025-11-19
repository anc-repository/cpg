# CPG 项目编译器前端知识入门指南

## 项目概述

Code Property Graph (CPG) 是一个多语言静态代码分析框架，它从源代码中提取图表示，用于安全分析和代码查询。

## 涉及的编译器前端知识

### 核心概念

| 知识领域 | 难度 | 在项目中的重要性 | 相关模块 |
|---------|------|----------------|---------|
| **词法分析 (Lexical Analysis)** | ⭐⭐ | 中等 | 由第三方解析器处理 |
| **语法分析 (Syntax Analysis)** | ⭐⭐⭐ | 高 | LanguageFrontend |
| **抽象语法树 (AST)** | ⭐⭐⭐⭐ | 非常高 | cpg-core/graph |
| **语义分析 (Semantic Analysis)** | ⭐⭐⭐⭐⭐ | 核心 | cpg-core/passes |
| **作用域与符号表** | ⭐⭐⭐⭐ | 非常高 | cpg-core/graph/scopes |
| **类型系统** | ⭐⭐⭐⭐ | 非常高 | cpg-core/graph/types |
| **数据流分析** | ⭐⭐⭐⭐⭐ | 核心 | DFGPass |
| **控制流分析** | ⭐⭐⭐⭐ | 非常高 | EvaluationOrderGraphPass |

## 学习路径

### 阶段一：编译原理基础（2-4周）

#### 1.1 必读书籍
- **《编译原理》（龙书）** - Alfred V. Aho et al.
  - 重点章节：
    - 第2章：简单的语法制导翻译器
    - 第5章：语法制导的翻译
    - 第6章：符号表
    - 第7章：运行时环境
- **《现代编译原理》** - Andrew W. Appel
  - 更实践导向，Kotlin/Java 背景适合阅读 Java 版本

#### 1.2 在线资源
- Stanford CS143: Compilers - https://web.stanford.edu/class/cs143/
- MIT 6.035: Computer Language Engineering
- Coursera: "Compilers" by Stanford University

#### 1.3 基础概念清单
- [ ] Token、Lexeme、词法分析器的作用
- [ ] 上下文无关文法（CFG）
- [ ] 递归下降解析
- [ ] AST vs Parse Tree 的区别
- [ ] 符号表的数据结构和查找规则
- [ ] 作用域和作用域链
- [ ] 静态类型检查的原理

### 阶段二：理解 CPG 架构（1-2周）

#### 2.1 项目文档阅读
```bash
# 克隆项目并查看文档
cd /home/user/cpg
cat README.md
cd docs/docs/CPG
# 阅读以下规范
- specs/dfg/           # 数据流图规范
- specs/eog/           # 执行顺序图规范
- specs/graph/         # 图模型规范
- impl/language/       # 语言前端实现说明
```

#### 2.2 关键概念映射

**CPG 概念 → 编译器概念**
| CPG 术语 | 编译器术语 | 说明 |
|---------|-----------|------|
| LanguageFrontend | Parser/Frontend | 语言特定的解析器 |
| Handler | Visitor Pattern | AST 遍历器 |
| Pass | Semantic Analysis Phase | 语义分析阶段 |
| Scope | Symbol Table | 作用域/符号表 |
| DFG | Data Flow Graph | 数据流图 |
| EOG | Control Flow Graph | 控制流图 |

#### 2.3 核心类阅读清单
```
优先级1（必读）：
1. cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/graph/Node.kt
2. cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/frontends/LanguageFrontend.kt
3. cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/frontends/Handler.kt
4. cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/graph/declarations/Declaration.kt
5. cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/graph/statements/Statement.kt

优先级2（重要）：
6. cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/passes/SymbolResolver.kt
7. cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/passes/TypeResolver.kt
8. cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/graph/scopes/Scope.kt
9. cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/graph/types/Type.kt

优先级3（深入理解）：
10. cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/passes/DFGPass.kt
11. cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/passes/EvaluationOrderGraphPass.kt
```

### 阶段三：动手实践（2-4周）

#### 3.1 环境搭建
```bash
# 1. 复制配置文件
cp gradle.properties.example gradle.properties

# 2. 编辑 gradle.properties，启用你熟悉的语言
# 例如只启用 Java 前端
enableJavaFrontend=true
enableCXXFrontend=false
enableGoFrontend=false
enablePythonFrontend=false

# 3. 构建项目
./gradlew build

# 4. 运行测试
./gradlew test
```

#### 3.2 实践项目建议

**项目1：简单 AST 遍历器**
- 目标：读取一个简单的 Java 程序，打印所有函数名
- 涉及知识：AST、Visitor 模式
- 参考：`cpg-core/src/test/kotlin` 下的测试用例

**项目2：作用域分析工具**
- 目标：分析一个 Java 类，输出每个变量的作用域
- 涉及知识：符号表、作用域链
- 参考：`SymbolResolver.kt` 和 `ScopeManager.kt`

**项目3：简单的数据流追踪**
- 目标：追踪某个变量从定义到使用的路径
- 涉及知识：数据流图、Use-Def 链
- 参考：`DFGPass.kt`

**项目4：添加自定义 Pass**
- 目标：实现一个检测未初始化变量的 Pass
- 涉及知识：完整的语义分析流程
- 参考：`cpg-core/src/main/kotlin/de/fraunhofer/aisec/cpg/passes/` 下的各种 Pass

#### 3.3 调试技巧
```kotlin
// 使用 Neo4j 可视化 CPG
// 1. 启动 Neo4j
docker run -p 7474:7474 -p 7687:7687 -d \
  -e NEO4J_AUTH=neo4j/password \
  -e NEO4JLABS_PLUGINS='["apoc"]' \
  neo4j:5

// 2. 运行 cpg-neo4j 模块
// 3. 在浏览器打开 http://localhost:7474
// 4. 执行 Cypher 查询查看 CPG
```

### 阶段四：深入特定领域（按需选择）

#### 4.1 数据流分析（静态污点分析）
- **推荐资源**：
  - "Principles of Program Analysis" - Nielson et al.
  - Lattice theory 和 fixed-point iteration
- **项目实践**：实现一个简单的污点分析器

#### 4.2 类型推断（对 Python/TypeScript 前端）
- **推荐资源**：
  - "Types and Programming Languages" - Benjamin Pierce
  - Hindley-Milner 类型推断算法
- **项目实践**：为动态类型语言添加类型提示

#### 4.3 跨过程分析（Inter-procedural Analysis）
- **推荐资源**：
  - Call graph construction
  - Context-sensitive analysis
- **项目实践**：实现函数调用链分析

#### 4.4 指针分析（C/C++ 前端）
- **推荐资源**：
  - Andersen's pointer analysis
  - Steensgaard's pointer analysis
- **项目实践**：实现简单的指向分析

## 具体学习建议

### 针对不同背景

#### 如果你是初学者
1. 先完成 Stanford CS143 前6周的课程
2. 用一个简单的语言（如 Lisp/Calculator）实现一个解释器
3. 再回来看 CPG 的 Java 前端代码

#### 如果你熟悉编译原理
1. 直接阅读 CPG 架构文档
2. 挑选一个你熟悉的语言前端（如 Java）深入阅读
3. 尝试运行并调试现有的测试用例
4. 实现一个自定义 Pass

#### 如果你想贡献代码
1. 查看 GitHub Issues 中标记为 "good first issue" 的问题
2. 阅读贡献指南（CONTRIBUTING.md）
3. 从修复小 bug 或改进文档开始
4. 逐步参与新功能开发

## 常见概念速查

### Handler 模式
CPG 中的 Handler 类似于访问者模式（Visitor Pattern）：
```kotlin
// 每个语言前端都有类似的 Handler
class ExpressionHandler : Handler<Expression> {
    fun handle(expr: ASTExpression): CPGExpression {
        // 将语言特定的表达式转换为 CPG 通用表示
    }
}
```

### Pass 系统
Pass 是对 CPG 进行增强的分析阶段：
```kotlin
abstract class Pass {
    abstract fun accept(component: Component)
    // 在这里实现你的分析逻辑
}
```

常见 Pass 顺序：
1. `TypeHierarchyResolver` - 解析类型继承关系
2. `SymbolResolver` - 解析符号引用
3. `TypeResolver` - 解析类型
4. `EvaluationOrderGraphPass` - 构建执行顺序图
5. `DFGPass` - 构建数据流图
6. `ControlFlowSensitiveDFGPass` - 控制流敏感的数据流分析

### 作用域体系
```
GlobalScope
  └─ NamespaceScope (package/namespace)
      └─ RecordScope (class/struct)
          └─ FunctionScope (method)
              └─ BlockScope (local block)
```

## 资源链接

### 项目资源
- 项目文档：https://fraunhofer-aisec.github.io/cpg/
- GitHub仓库：https://github.com/Fraunhofer-AISEC/cpg
- 学术论文：https://arxiv.org/abs/2203.08424

### 编译器学习资源
- 《Crafting Interpreters》：https://craftinginterpreters.com/ (免费在线)
- LLVM Tutorial：https://llvm.org/docs/tutorial/
- "Engineering a Compiler" by Cooper & Torczon

### 程序分析资源
- Static Program Analysis（免费课程）：https://cs.au.dk/~amoeller/spa/
- Datalog 和 Soufflé：https://souffle-lang.github.io/

## 测试你的理解

完成以下检查点来验证你的学习进度：

- [ ] 能解释 AST、CFG、DFG 的区别和联系
- [ ] 能画出一段简单代码的 CPG 表示
- [ ] 能阅读并理解 `SymbolResolver.kt` 的核心逻辑
- [ ] 能独立实现一个简单的自定义 Pass
- [ ] 能解释为什么需要多遍分析（multi-pass）
- [ ] 能区分语法错误和语义错误
- [ ] 理解 scope 和 symbol table 的关系
- [ ] 能解释 DFG 如何用于污点分析

## 获取帮助

- GitHub Discussions：提问和讨论
- 项目文档：查看详细的 API 说明
- 阅读测试用例：`cpg-*/src/test/` 目录下有大量示例
- 查看现有 Issue：很多问题已经被讨论过

## 总结

CPG 项目是一个工程导向的编译器前端框架，它：
- 不需要你从零实现词法分析器和语法分析器
- 专注于语义分析和程序分析
- 提供了可扩展的架构用于自定义分析

适合学习的原因：
- 真实的工业级代码库
- 支持多种语言，可以对比学习
- 活跃的社区和持续维护
- 清晰的模块化设计

祝学习顺利！
