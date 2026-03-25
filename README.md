# agentkit - Agent Framework

> Agent framework with skill system, tool registry, and memory management using hexagonal architecture.

## Overview

agentkit is a framework for building AI agents with:
- **Skill System**: Modular, composable skills
- **Tool Registry**: Extensible tool integration
- **Memory Management**: Short-term and long-term memory
- **Hexagonal Architecture**: Clean separation of concerns

## Architecture

```
agentkit/
├── src/
│   ├── domain/           # Pure domain (no external deps)
│   │   ├── agents/       # Agent, AgentConfig
│   │   ├── skills/       # Skill, SkillRegistry
│   │   ├── tools/        # Tool, ToolResult
│   │   ├── memory/       # Memory, MemoryStore
│   │   └── ports/        # Domain interfaces
│   │
│   ├── application/      # Use cases
│   │   ├── agent/       # Agent execution
│   │   ├── skills/      # Skill management
│   │   └── tools/       # Tool management
│   │
│   ├── adapters/        # Implementations
│   │   ├── llm/        # LLM adapters
│   │   ├── memory/     # Memory backends
│   │   └── tools/      # Tool implementations
│   │
│   └── infrastructure/  # Cross-cutting
│
├── skills/             # Built-in skills
└── examples/           # Example agents
```

## xDD Methodologies Applied

| Category | Methodology |
|----------|-------------|
| Development | TDD, BDD, DDD, ATDD, SDD |
| Design | SOLID, DRY, KISS, YAGNI |
| Architecture | Clean, Hexagonal, CQRS |

## Installation

```bash
cargo add agentkit
```

## Quick Start

```rust
use agentkit::prelude::*;
use agentkit::domain::agents::Agent;

struct MyAgent;

#[async_trait]
impl Agent for MyAgent {
    async fn run(&self, ctx: &Context) -> Result<Output> {
        // Agent logic here
        Ok(Output::text("Task completed"))
    }
}

let agent = MyAgent;
agent.run(&context).await?;
```

## Skills

Skills are the building blocks of agents:

```rust
use agentkit::domain::skills::{Skill, SkillResult};

struct WebSearchSkill;

#[async_trait]
impl Skill for WebSearchSkill {
    fn name(&self) -> &str { "web_search" }
    
    async fn execute(&self, params: Value) -> Result<SkillResult> {
        // Search implementation
        Ok(SkillResult::success(json!({ "results": [] })))
    }
}
```

## Tools

Tools extend agent capabilities:

```rust
use agentkit::domain::tools::{Tool, ToolCall};

struct CalculatorTool;

#[async_trait]
impl Tool for CalculatorTool {
    fn name(&self) -> &str { "calculator" }
    
    async fn call(&self, call: ToolCall) -> Result<Value> {
        let expr = call.params.get("expression").unwrap();
        Ok(json!({ "result": evaluate(expr) }))
    }
}
```

## Memory

Two-tier memory system:

```rust
use agentkit::domain::memory::{Memory, MemoryStore};

// Short-term: Conversation context
let short_term = ShortTermMemory::new(limit = 10);

// Long-term: Persistent knowledge
let long_term = LongTermMemory::new(store);
```

## Documentation

- [API Documentation](https://docs.rs/agentkit)
- [Book](https://agentkit.dev/book)
- [Examples](./examples/)

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines.

## License

MIT OR Apache-2.0
