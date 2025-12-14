# Code Review Assistant - GenAI Intern Assessment

**⚠️ FULL DISCLOSURE: This project is part of an assessment for a GenAI intern position. If you wish to have this taken down, please contact the author. All elements have been anonymized to ensure enterprise privacy.**

## Table of Contents
- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Architecture](#architecture)
- [Features](#features)
- [Technical Implementation](#technical-implementation)
- [System Flow](#system-flow)
- [Installation & Setup](#installation--setup)
- [Usage](#usage)
- [Output Format](#output-format)
- [Limitations](#limitations)
- [Next Steps](#next-steps)

## Overview

This Code Review Assistant is an AI-powered tool designed to automate the code review process using Large Language Models (LLMs). The system performs two sequential passes over developer code to detect issues and generate actionable tasks, streamlining the code review workflow and improving code quality.

Built using LangChain Expression Language, OpenAI models, and Pydantic for structured output parsing, this tool demonstrates advanced prompt engineering and parameter tuning capabilities for automated code analysis.

## Problem Statement

Traditional code reviews are time-consuming and can be inconsistent due to human oversight. This project addresses the need for:
- Automated detection of code issues (bugs, security vulnerabilities, performance problems)
- Generation of prioritized, actionable tasks for developers
- Consistent and thorough code analysis
- Structured output for integration with development workflows

## Architecture

The system follows a **Two-Pass Architecture**:

### Pass 1: Code Review
- **Input**: Code before changes, code after changes, brief description
- **Process**: LLM analyzes code for issues across multiple categories
- **Output**: Structured JSON with detected issues (file/line, severity, suggestions)

### Pass 2: TODO Planning
- **Input**: Issues from Pass 1 + current code
- **Process**: LLM converts issues into prioritized, actionable tasks
- **Output**: Structured JSON with prioritized tasks and implementation guidance

## Features

### Core Capabilities
- **Multi-category Issue Detection**: Identifies bug risks, security vulnerabilities, performance issues, readability concerns, and maintainability problems
- **Severity Classification**: Issues categorized as CRITICAL, HIGH, MEDIUM, or LOW priority
- **Actionable Suggestions**: Provides specific, implementable fixes for each identified issue
- **Priority-based Task Planning**: Converts issues into P0-P3 prioritized tasks with acceptance criteria
- **Structured Output**: JSON format compatible with CI/CD pipelines and issue trackers

### Technical Features
- **LangChain Integration**: Built using LangChain Expression Language for LLM orchestration
- **Pydantic Validation**: Ensures structured, validated output using Pydantic models
- **AST Validation**: Python syntax validation before processing
- **Interactive Frontend**: Simple UI for code input and result download
- **Parameter Tuning**: Optimized temperature settings for different analysis phases

## Technical Implementation

### Dependencies
- `langchain` - LLM orchestration framework
- `langchain-openai` - OpenAI integration for LangChain
- `openai` - OpenAI API client
- `pydantic` - Data validation and structured output
- `langsmith` - LLM application debugging and monitoring
- `ipywidgets` - Interactive UI components

### Models Used
- **Pass 1**: `gpt-4o-mini` (temperature=0.2) - Precise issue detection
- **Pass 2**: `gpt-3.5-turbo` (temperature=0.4) - Creative task planning

### Data Models

#### Code Issue Schema
```python
class CodeIssue(BaseModel):
    file: str           # File path
    line: int           # Line number
    category: str       # BUG_RISK, SECURITY, PERFORMANCE, etc.
    severity: str       # CRITICAL, HIGH, MEDIUM, LOW
    description: str    # Issue description
    suggestion: str     # Actionable fix suggestion
```

#### TODO Task Schema
```python
class TODOTask(BaseModel):
    priority: str               # P0, P1, P2, P3
    task_description: str       # Detailed task description
    related_issues: List[int]   # References to issue indices
    patch_hint: str             # Implementation guidance
    test_recommendation: str    # Testing strategy
```

## System Flow

```
[User Code Input] → [AST Validation] → [Pass 1: LLM Review] → [Pass 2: LLM TODO Planner] → [Structured Output]
```

1. **Input Validation**: AST parsing ensures syntactically valid Python code
2. **Pass 1 Analysis**: Comprehensive code review across multiple dimensions
3. **Pass 2 Planning**: Strategic task creation with prioritization
4. **Output Generation**: JSON files with issues and tasks for download

## Installation & Setup

This project is designed for Google Colab execution:

1. **Environment Setup**:
   ```python
   pip install langchain langchain-openai openai pydantic langsmith ipywidgets
   ```

2. **API Configuration**:
   - Set `OPENAI_API_KEY` environment variable
   - Use `gpt-4o-mini` for Pass 1, `gpt-3.5-turbo` for Pass 2

3. **Execution Environment**: Optimized for Google Colab runtime

## Usage

### Input Requirements
- **Code Before**: Original code (optional, defaults to placeholder)
- **Code After**: Modified code requiring review
- **Description**: Brief summary of changes made

### Interactive Interface
1. Paste code in "Code Before" and "Code After" text areas
2. Provide change description
3. Click "Analyze Code" button
4. Download generated JSON files for issues and tasks

### Output Files
- `issues.json`: Comprehensive list of detected code issues
- `tasks.json`: Prioritized TODO tasks with implementation guidance

## Output Format

### Issues JSON Structure
```json
{
  "issues": [
    {
      "file": "string",
      "line": integer,
      "category": "BUG_RISK|SECURITY|PERFORMANCE|READABILITY|MAINTAINABILITY",
      "severity": "CRITICAL|HIGH|MEDIUM|LOW",
      "description": "Issue description",
      "suggestion": "Actionable fix suggestion"
    }
  ]
}
```

### Tasks JSON Structure
```json
{
  "tasks": [
    {
      "priority": "P0|P1|P2|P3",
      "task_description": "Detailed task description",
      "related_issues": [issue_indices],
      "patch_hint": "Implementation guidance",
      "test_recommendation": "Testing strategy"
    }
  ]
}
```

## Limitations

### Known Constraints
- **Language Support**: Currently optimized for Python code analysis
- **Context Window**: Limited by LLM token constraints for very large files
- **False Positives**: May flag legitimate code patterns as issues
- **Complex Logic**: Struggles with highly complex business logic analysis
- **Integration**: Basic UI without advanced workflow integration

### Performance Considerations
- API costs scale with code size and analysis frequency
- Processing time increases with code complexity
- Rate limiting may affect batch processing capabilities

## Next Steps

### Enhancement Opportunities
1. **Multi-language Support**: Extend to JavaScript, Java, C++, etc.
2. **Framework Integration**: Direct integration with GitHub, GitLab, Bitbucket
3. **Custom Rules**: Configurable rule sets for organization-specific standards
4. **Real-time Analysis**: IDE plugin for live code analysis
5. **Metrics Dashboard**: Analytics for team code quality trends
6. **Security Scanning**: Enhanced security vulnerability detection
7. **Performance Profiling**: Runtime performance impact analysis

### Technical Improvements
- Advanced caching mechanisms
- Parallel processing for multiple files
- Custom embeddings for domain-specific analysis
- Fine-tuned models for specialized use cases

## Contact

This project was created as part of a GenAI intern assessment. If you have questions, concerns, or wish to have this repository taken down, please contact the author.

---

*Note: All sensitive information and proprietary elements have been anonymized to protect enterprise privacy while demonstrating the technical capabilities and architectural patterns implemented.*
