# LangGraph Design Patterns: Orchestration & Evaluation

## What This Code Does

This code implements two agentic workflow patterns using LangGraph: the Orchestrator-Worker pattern for parallel task decomposition and execution, and the Reflection pattern for iterative output refinement through evaluation-feedback loops.

## Key Comparisons

This code implements and compares two design patterns:

1. **Orchestrator-Worker Pattern** - Dynamically decomposes an input into structured subtasks, executes them in parallel across multiple worker nodes, and synthesizes the results
2. **Reflection Pattern** - Generates an initial output, evaluates it against target criteria, and iteratively refines it using feedback until acceptance or iteration limit is reached

## Core Components

**Orchestrator-Worker Pattern**
- `orchestrator` node - Uses LLM with structured output to parse input and produce a list of work units
- `assign_workers` function - Creates parallel worker tasks using LangGraph's `Send()` API for fan-out execution
- Worker nodes - Process individual work units independently using LLM with specialized prompts
- `synthesizer` node - Aggregates all parallel worker outputs into a final result
- State field with `operator.add` - Automatically merges results from concurrent workers

**Reflection Pattern**
- `determine_target_grade` node - Analyzes input to establish a target quality criterion
- Generator node - Switches between two generation strategies: initial creation and feedback-aware revision
- Evaluator node - Assesses generated output using structured scoring and written feedback
- Routing function - Conditional logic comparing current grade to target grade with iteration limit
- Reflection loop - Conditional edge routes back to generator when evaluation fails, creating an iterative refinement cycle

**LangGraph Workflow Features**
- `StateGraph` with typed state schemas for shared context across nodes
- `add_conditional_edges` for dynamic routing and parallel fan-out
- `Send()` API for dynamic node invocation
- Mermaid diagram visualization of graph structure
- Iteration counters to prevent infinite loops
