# Computational Modeling Advisor CLI - Requirements Document v2.0

## 1. Introduction
### 1.1 Purpose
Enhanced CLI tool that combines decision-tree logic with LLM analysis to recommend computational modeling approaches and generate comprehensive Microsoft Word reports.

## 2. Enhanced Requirements

### 2.1 LLM Integration
**FR7**: The system shall:
- Use OpenAI's API (GPT-4) to:
  - Refine user responses through natural language processing
  - Generate detailed explanations of recommended methods
  - Create narrative summaries for the final report
- Implement fallback logic when API is unavailable

**FR8**: Configuration shall allow:
- API key management
- Model selection (gpt-4, gpt-3.5-turbo)
- Temperature control for deterministic/stochastic outputs

### 2.2 Data Validation
**FR9**: The system shall implement JSONSchema validation:
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "recommendations": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "method": {"type": "string"},
          "confidence_score": {"type": "number", "minimum": 0, "maximum": 1},
          "pros": {"type": "array", "items": {"type": "string"}},
          "cons": {"type": "array", "items": {"type": "string"}},
          "implementation_steps": {"type": "array", "items": {"type": "string"}}
        },
        "required": ["method", "confidence_score"]
      }
    }
  },
  "required": ["recommendations"]
}
```

## 2.3 Report Generation
FR10: The system shall produce DOCX reports containing:
- Cover page with timestamp and session ID
- Executive summary (LLM-generated)
- Methodology comparison matrix
- Implementation roadmap

### References and further reading
FR11: Report styling shall include:
- Custom headers/footers
- Company/research group branding options
- Automatic table of contents
- Proper citation formatting

## 3. Technical Architecture
### 3.1 Component Diagram
- User Input → [Questionnaire Engine] → [LLM Processor] → [Validation Layer]
                                   ↘ [Decision Tree] ↗
                                   
[Validated Output] → [Report Generator] → DOCX File

## 3.2 Implementation Details
- OpenAI SDK: Structured prompts with few-shot learning examples
- python-docx: For report generation with:
- Dynamic paragraph insertion
- Table generation
- Image placeholder support (for future diagram integration)

jsonschema: For output validation before report generation

## 4. Enhanced Usage Example

```bash
$ model-advisor advanced --llm --report=full_report.docx
? Describe your modeling problem in natural language: 
> I need to predict urban traffic patterns with sparse sensor data...

[System engages in clarifying dialog via LLM]
```

### Generating report with 3 recommended approaches...
✔ Validated LLM output
✔ Generated 15-page DOCX report
✔ Saved session log to session_20240520.json

## 5. Security Considerations
- API keys shall never be stored in plaintext
- User inputs shall be anonymized before LLM processing
- Reports shall include watermark when containing sensitive information

## 6. New Non-Functional Requirements
### 6.1 LLM-Specific
NFR5: Latency <5s for LLM interactions

NFR6: Automatic retry for failed API calls (3 attempts)

## 6.2 Reporting
NFR7: DOCX generation time <10s for 20-page reports

NFR8: Support for templates in .dotx format

## 7. Updated Appendix
### 7.1 Required Libraries
- openai>=1.0
- python-docx>=0.8.11
- jsonschema>=4.0.0
- pydantic>=2.0 (for additional data validation)

## 7.2 Error Handling
- Schema validation errors shall produce human-readable diagnostics
- Failed report generation shall preserve intermediate JSON results

