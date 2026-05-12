# Part 3 – Prompt Preparation

## Selected Pull Request

PR #1172 – Make RAG embedding configurable and add gpt-4-turbo in token_counter

---

# 3.1.1 Repository Context

MetaGPT is an AI framework that attempts to recreate a software company workflow via several different AI-driven positions, including the roles of Product Manager, Architect, Engineer, and QA. Unlike other multi-lingual frameworks, the project breaks down all tasks among several agents and integrates them to perform software engineering jobs. It is capable of generating requirements, designs, APIs, code, and testing process autonomically based on a single user's input.

Most repositories of MetaGPT cover topics of AI orchestration, RAGs, workflow automation, and integration of big models. Additionally, they allow users to integrate with various AI providers and create autonomous AI solutions able to operate complex processes step by step.
The targeted audience consists of AI researchers, developers, startups, automation engineers, and enterprises seeking to use autonomous software engineering processes in their operations. The framework may be helpful for creating AI-assistants, multi-agents collaboration solutions, and intelligent work pipelines.

Coordination management in a multi-AI environment is one of the key problems covered in the repository. With the growing importance of embedding systems, retrieval processes, and multiple model providers used in modern AI, the repository covers aspects of modularity and configurability of the integration process.
---

# 3.1.2 Pull Request Description

This PR enhances the embedding functionality of RAG within MetaGPT by adding configurability of the embedding provider as opposed to embedding providers being mainly OpenAI/Azure. Previously, for those wanting to make use of another embedding provider, they would have to either specify embedding models manually or configure the embedding provider's settings. This process wasn't easy or scalable due to the rigidity of the internal logic and configurations used to generate embeddings.

The PR enables support for several embedding providers including OpenAI, Azure, Gemini, and Ollama. Embedding configurations have been changed to enable dynamic configuration of the provider from configuration settings without having to alter the internal logic. Additionally, the PR also introduces the GPT-4-Turbo version to the token_counter module, which will calculate tokens for newer OpenAI LLM models.

As part of implementing this change, some internal files relating to embedding factories, embedding configuration schemas, async helpers, and RAG schemas have been modified. Generally, the aim of this PR is to enhance the modularity and extensibility of the embedding workflow and enable compatibility with various AI ecosystems.

Previously, embedding generation was tightly coupled with embedding providers' internal logic. Post-update, this workflow has become more flexible and scalable to accommodate other providers and models.
---

# 3.1.3 Acceptance Criteria

✓ When the embedding provider chosen is OpenAI embeddings from the config, the implementation will be successful in loading the appropriate embedding provider.

✓ The system should be able to load Azure, Gemini, or Ollama embedding providers when their respective configurations are supplied without changes to any code within the source code.

✓ The implementation will accurately identify GPT-4-Turbo within the token_counter module and compute its token counts correctly.

✓ The embedding factory must be capable of producing embedding providers from configuration values rather than depending entirely on hardcoding OpenAI or Azure.

✓ All embedding provider workflows currently using either OpenAI and Azure will not be affected by the changes and maintain backward compatibility.

✓ All invalid embedding configurations should generate error codes appropriately.

✓ The implementation should allow asynchronous embedding providers without affecting retrieval.
---

# 3.1.4 Edge Cases

1. Duplicate embedding generation when retriever configurations are passed multiple times during document indexing, causing unnecessary index rebuilding and increased memory usage.

2. Unsupported or incorrectly configured embedding providers such as Gemini or Ollama leading to initialization failures during runtime.

3. Missing embedding model definitions for configurable providers that require explicit model selection.

4. Incorrect token calculation behavior when GPT-4-Turbo or newly added models are passed into the token_counter module.

5. Backward compatibility issues where existing OpenAI or Azure embedding workflows may fail after introducing configurable embedding providers.

6. Asynchronous embedding requests failing or timing out while switching dynamically between multiple providers.

7. Retrieval inconsistencies caused by stale or reused embeddings when indexes are partially rebuilt.

8. Error handling scenarios where invalid retriever configurations may silently fail instead of raising meaningful exceptions.

9. Performance degradation when large document collections repeatedly regenerate embeddings instead of reusing cached vector representations.

10. Future embedding provider integrations introducing schema conflicts or unsupported parameter mappings inside the embedding factory system.

---

# 3.1.5 Initial Prompt

You are working on the MetaGPT repository, an open-source multi-agent AI framework that simulates a collaborative software company using AI-driven roles such as Product Manager, Architect, Engineer, and QA. The repository heavily relies on Retrieval-Augmented Generation (RAG), configurable embedding providers, asynchronous workflows, and token management systems.

The current implementation of the RAG embedding workflow depends mainly on OpenAI or Azure configurations defined inside the llm_config system. This creates limitations when integrating newer embedding providers and makes the architecture less flexible for future extensions.

Your task is to refactor the embedding workflow so embedding providers can be configured dynamically through configuration settings. The implementation should support OpenAI, Azure, Gemini, and Ollama providers without requiring direct modifications to internal source code. Update the embedding factory system to initialize providers dynamically based on configuration values.

The implementation must also extend token_counter functionality to support GPT-4-Turbo and ensure accurate token calculations for newer models. Update any required schema definitions, helper utilities, asynchronous workflows, and retriever-related logic necessary for proper integration.

The implementation should include proper validation, exception handling, and meaningful error reporting for unsupported providers, missing embedding models, invalid configurations, and token calculation failures.

Testing requirements:

Verify embedding initialization for OpenAI, Azure, Gemini, and Ollama.
Verify GPT-4-Turbo token counting behavior.
Validate asynchronous embedding workflows.
Test backward compatibility with existing OpenAI and Azure configurations.
Prevent duplicate embedding generation during retrieval workflows.
Ensure failed unit tests do not produce excessive logging output.
Validate retriever behavior during index rebuilding scenarios.
Add or update unit tests related to embedding configuration, token counting, and retrieval workflows.

The final implementation should improve modularity, maintainability, extensibility, testing stability, and retrieval efficiency while minimizing disruption to the current RAG architecture. Contributors should clearly document architectural decisions, testing strategies, configuration handling behavior, and edge-case considerations during implementation.
