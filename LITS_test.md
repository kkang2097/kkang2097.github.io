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
Abstractions in LlamaIndex may seem confusing, but they can be boiled down to three different types:
- Utility (manipulates data, no LLM connection needed)
- Prompt wrappers (Abstractions that format prompt output and give you a nice format)

Type 1 examples:
- Parser
- Node
- Embedding
- Response
- 
- L



#### How do we test nested abstractions?
