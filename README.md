# Making AI Agents Smarter

Agents are highly resourceful and intelligent, but their capabilities are limited by the instructions they receive. This project aims to document best practices, including specialized specifications for complex tasks, and make them accessible to agents. As AI models become more sophisticated and gain access to advanced tools that suit them, they may eventually develop the ability to organize themselves independently, reducing their reliance on micro-management. Until then, this initiative aims to compile a small directory of proven strategies that have been effective for them.

## Code To Non-Code Artifacts - A Paradigm Shift
In any large organization, significant projects originate from shared ideas in emails and chats. These ideas evolve into comprehensive documents that transform from simple pitches outlining potential future outcomes to detailed assessments of the current state and identified gaps. These documents serve as the foundation for project requirements, eventually leading to product architecture, design, and ultimately, code development.

In an era where code generation is relatively more affordable, the earlier documents become increasingly valuable and crucial. Their content carries a deeper and more enduring impact. This represents a major shift in software development (or any kind of workflow resulting in an artifact) where the ultimate end-product is incresingly becoming dispensible, necessitating a greater emphasis on the creation of detailed requirements and instructions rather than solely focusing on the code itself (equally applicable to any end-product, such as Figma, Slides, CAD designs, other visual or audible creative works, et al). This shift demands meticulous organization from project owners, whether human or agents.

## An Example - A Project Spec Template
A Project Spec is an attempt to standardize the template for a complex project. It’s designed for collaboration between humans and agents. The result is a more detailed and specific spec that can be applied to similar projects in its domain. This domain-specific spec can be further enhanced through discussions on vision, goals, requirements, existing agreements or constraints, and anything else that can improve collaboration and the quality of the end result. Most importantly, it includes key pieces for verifying the work and checking if the goal has been achieved.

Example:
```
Project Spec Template -(design for archiving the contents of a website)-> Site Archival Spec
```

(NOTE: The file in spec-for/site-archival.md was not generated via this method and existed prior to it).

## Intent
To develop a versatile format for documenting requirements, decisions or any other form of important discussion on a topic resulting in an artifact that will be shared for broader consumption. The format must be easily read and understood by humans and machines. It should be easily contributed to and versionable, with the ability to diff changes between versions.

## Examples
```
From a high level initial (business) plan - optionally with `Press Release` and `FAQs` sections
  -> Initial product requirenent document
    -> Feature selection and software architecture
      -> Interfaces, stub objects and unit tests creation (TDD)
        -> Code delivery
```

In the above case, each stage leads to artifacts. However, except code, each other artifact becomes a part of the original spec itself (that started with an intial (business) plan. Everything is versioned and all changes are logged.

The closest system like this in real life is how laws are created and modified. 

## Current State
This is in the very early stages of development. As the author of this project, my goal is to continually enhance this based on my experiences.

## Inspiration
- [OpenAI Model Spec](https://model-spec.openai.com/2025-09-12.html)
- [The New Code - Sean Grove, OpenAI](https://www.youtube.com/watch?v=8rABwKRsec4)
- [GitHub Spark](https://github.com/features/spark)
