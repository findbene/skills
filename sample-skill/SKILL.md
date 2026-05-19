---
name: sample-skill
description: "Sample text processor demonstrating basic skill structure and Python stdlib-only tooling. Triggers: 'use sample-skill', 'sample skill', 'sample-skill task'."
allowed-tools: Bash, Glob, Grep, Read
---

# Sample Text Processor

---

**Name**: sample-text-processor
**Tier**: BASIC
**Category**: Text Processing
**Dependencies**: None (Python Standard Library Only)
**Author**: Claude Skills Engineering Team
**Version**: 1.0.0
**Last Updated**: 2026-02-16

---

## Description

The Sample Text Processor is a simple skill designed to demonstrate the basic structure and functionality expected in the claude-skills ecosystem. This skill provides fundamental text processing capabilities including word counting, character analysis, and basic text transformations.

This skill serves as a reference implementation for BASIC tier requirements and can be used as a template for creating new skills. It demonstrates proper file structure, documentation standards, and implementation patterns that align with ecosystem best practices.

The skill processes text files and provides statistics and transformations in both human-readable and JSON formats, showcasing the dual output requirement for skills in the claude-skills repository.

## Features

### Core Functionality
- **Word Count Analysis**: Count total words, unique words, and word frequency
- **Character Statistics**: Analyze character count, line count, and special characters
- **Text Transformations**: Convert text to uppercase, lowercase, or title case
- **File Processing**: Process single text files or batch process directories
- **Dual Output Formats**: Generate results in both JSON and human-readable formats

### Technical Features
- Command-line interface with comprehensive argument parsing
- Error handling for common file and processing issues
- Progress reporting for batch operations
- Configurable output formatting and verbosity levels
- Cross-platform compatibility with standard library only dependencies

## Usage

### Basic Text Analysis
```bash
python text_processor.py analyze document.txt
python text_processor.py analyze document.txt --output results.json
```

### Text Transformation
```bash
python text_processor.py transform document.txt --mode uppercase
python text_processor.py transform document.txt --mode title --output transformed.txt
```

### Batch Processing
```bash
python text_processor.py batch text_files/ --output results/
python text_processor.py batch text_files/ --format json --output batch_results.json
```

## Examples

### Example 1: Basic Word Count
```bash
$ python text_processor.py analyze sample.txt
=== TEXT ANALYSIS RESULTS ===
File: sample.txt
Total words: 150
Unique words: 85
Total characters: 750
Lines: 12
Most frequent word: "the" (8 occurrences)
```

### Example 2: JSON Output
```bash
$ python text_processor.py analyze sample.txt --format json
{
  "file": "sample.txt",
  "statistics": {
    "total_words": 150,
    "unique_words": 85,
    "total_characters": 750,
    "lines": 12,
    "most_frequent": {
      "word": "the",
      "count": 8
    }
  }
}
```

### Example 3: Text Transformation
```bash
$ python text_processor.py transform sample.txt --mode title
Original: "hello world from the text processor"
Transformed: "Hello World From The Text Processor"
```

## Installation

This skill requires only Python 3.7 or later with the standard library. No external dependencies are required.

1. Clone or download the skill directory → verify: file content matches expected shape
2. Navigate to the scripts directory → verify: step output matches expected outcome
3. Run the text processor directly with Python → verify: command exit code 0

```bash
cd scripts/
python text_processor.py --help
```

## Configuration

The text processor supports various configuration options through command-line arguments:

- `--format`: Output format (json, text)
- `--verbose`: Enable verbose output and progress reporting
- `--output`: Specify output file or directory
- `--encoding`: Specify text file encoding (default: utf-8)

## Architecture

The skill follows a simple modular architecture:

- **TextProcessor Class**: Core processing logic and statistics calculation
- **OutputFormatter Class**: Handles dual output format generation
- **FileManager Class**: Manages file I/O operations and batch processing
- **CLI Interface**: Command-line argument parsing and user interaction

## Error Handling

The skill includes comprehensive error handling for:
- File not found or permission errors
- Invalid encoding or corrupted text files
- Memory limitations for very large files
- Output directory creation and write permissions
- Invalid command-line arguments and parameters

## Performance Considerations

- Efficient memory usage for large text files through streaming
- Optimized word counting using dictionary lookups
- Batch processing with progress reporting for large datasets
- Configurable encoding detection for international text

## Contributing

This skill serves as a reference implementation and contributions are welcome to demonstrate best practices:

1. Follow PEP 8 coding standards → verify: step output matches expected outcome
2. Include comprehensive docstrings → verify: step output matches expected outcome
3. Add test cases with sample data → verify: dependency resolves + import works
4. Update documentation for any new features → verify: step output matches expected outcome
5. Ensure backward compatibility → verify: step output matches expected outcome

## Limitations

As a BASIC tier skill, some advanced features are intentionally omitted:
- Complex text analysis (sentiment, language detection)
- Advanced file format support (PDF, Word documents)
- Database integration or external API calls
- Parallel processing for very large datasets

This skill demonstrates the essential structure and quality standards required for BASIC tier skills in the claude-skills ecosystem while remaining simple and focused on core functionality.

## When NOT to use

- Real text processing in production — this is a sample/reference skill only
- PDF / Word / Excel parsing — use `pdf`, `docx`, `xlsx` skills
- NLP analysis (sentiment, topic, entity) — use a dedicated NLP skill
- Large dataset processing — sample skill is single-threaded, single-file
- Skill-creation guidance — use `skill-creator` instead

## Red Flags

| Rationalization | Reality |
|---|---|
| "Reach for sample-skill for any text task" | This is a BASIC-tier reference implementation, not a production tool |
| "Add advanced features to make it production-ready" | Better to fork into a new skill — sample-skill must remain minimal as a reference |
| "Bring in third-party deps to extend it" | Skill mandates Python stdlib only — adding deps breaks the educational contract |
| "Skip docstrings since the code is simple" | Sample skill exists to demonstrate documentation standards — docstrings are mandatory |

## Output Contract

Finished output must contain:
- Pure Python stdlib code (no third-party imports)
- PEP 8 compliant formatting
- Comprehensive docstrings on every function/class
- Dual output (human-readable text + machine-readable JSON)
- Test cases with sample input data
- README documenting usage and limitations

## Examples

**Example 1 — Word count + character stats**
- Input: `sample_skill.process('sample.txt', mode='stats')`
- Action: Read file with `open()` → tokenize via `re` → count words, chars, lines → format both text and JSON outputs
- Output: Stats printed to stdout, JSON written to `sample.stats.json`

**Example 2 — Basic transform (uppercase + reverse)**
- Input: `sample_skill.transform('hello', operations=['upper', 'reverse'])`
- Action: Apply transformations in order using stdlib methods
- Output: Returns `"OLLEH"`, no third-party deps, fully testable
