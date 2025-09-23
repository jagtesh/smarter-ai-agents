# Standard Generative Specification
Standardized specification format for collaborative mixed AI and human use. Discuss goals, requirements, agreements and anything else that can meaningfully improve collaboration.

## Intent
To develop a versatile format for documenting requirements, decisions or any other form of important discussion on a topic resulting in an artifact that will be shared for broader consumption. The format must be easily read and understood by humans and machines. It should be easily contributed to and versionable, with the ability to diff changes between versions.

## Examples
From a high level initial (business) plan - optionally with `Press Release` and `FAQs` sections
  -> Initial product requirenent document
    -> Feature selection and software architecture
      -> Interfaces, stub objects and unit tests creation (TDD)
        -> Code delivery

In the above case, each stage leads to artifacts. However, except code, each other artifact becomes a part of the original spec itself (that started with an intial (business) plan. Everything is versioned and all changes are logged.

The closest system like this in real life is how laws are created and modified. 

## Current State
This is in the very early stages of development. As the author of this project, my goal is to continually enhance this based on my experiences.

## Inspiration
- [OpenAI Model Spec](https://model-spec.openai.com/2025-09-12.html)
- [The New Code - Sean Grove, OpenAI](https://www.youtube.com/watch?v=8rABwKRsec4)
- [GitHub Spark](https://github.com/features/spark)
