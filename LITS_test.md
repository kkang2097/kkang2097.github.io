After quite a bit of thought, here's what I think about testing existing and new abstractions in LlamaIndex.

## Definition: What is LlamaIndex?
As a SDK, LlamaIndex is a collection of clever abstractions built over specific prompts. 

# Requirements
Warning: Since prompts are so flexible, we can only almost-guarantee that LlamaIndex works under the "recommended settings". (Running LlamaIndex using the default prompts)

#### Do the prompts work?
Prompts serve as the foundation for our abstractions so we need to make them rock-solid. What does that mean? 
- Guaranteed Types Given a Prompt
  - Each prompt, when fed into a LLM and parsed, should be able to almost-guarantee a valid object that is castable to our DesiredType.
  - For example: parse( LLM completion given Prompt A ) -> (object or object[] is DesiredType)
  - LlamaIndex example: parse( LLM completion given MultiSelectionPrompt ) -> (object[] is Selection[])
- Decent Coverage across mainstream LLMs

#### Do the abstractions work?
Abstractions in LlamaIndex may seem confusing, but they can be boiled down to 4 different categories:
- Integrations
- Utility (manipulates data, no LLM or DB connection needed)
- Prompt wrappers (Abstractions that take prompt output and give you a nice format, LLM connection needed)
- Abstraction Engines (Handle interactions between multiple component abstractions)

Type 1 examples:
- VectorStores/DocumentStores
- Readers

Type 2 examples:
- Parser
- Node
- Embedding
- Response
- Textsplitter

Type 3 examples:
- Selector
- ResponseSynthesizer/Summarizer
- Question/Subquestion generator
- Retriever (only if it's LLM-based)

Type 4 examples:
- Chat Engine
- Query Engine

#### How do we test these abstractions?

Type 1:
If in-memory, write unit tests. If out of memory, write integration tests. **Unit Tests or Integration Tests**

Type 2: 
Regular unit tests will do. **Unit Tests**

Type 3:
These tests need to be two-sided. One side needs to test the default prompts for correctness. The other side will mock the llm.complete output and check that our data comes out in the correct format (just to check that the outputs are reasonable). **Benchmark Tests, Unit Tests**

Type 4:
Interfaces alone can't enforce correct behavior for these larger abstractions. One approach is to mock the LLM completions and the child abstractions. Then we enforce which functions get called using unit tests. Currently trying this out to see how much work this takes. **Unit Tests**

## Types of Tests We Need To Run
- Integration Tests
- Benchmark Tests
- Unit Tests

### Immediate Action Steps
- Write unit tests for Selector & RouterQueryEngine

### Future Action Steps
- Scale this out and make a robust standard for creating new LlamaIndex abstractions
