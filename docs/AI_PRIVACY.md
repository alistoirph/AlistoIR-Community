# AI outbound privacy — 3.0.3

All four AIService providers (OpenAI, Anthropic, DeepSeek and configured local endpoints) pass model content through the same mandatory sanitizer. Provider authentication remains in the required HTTP headers; it is not included as evidence.

Structured credentials, including nested credential objects, are withheld. Known identity fields receive consistent random aliases within the service context. Email addresses, IP addresses, domains and common absolute paths are pseudonymized. Raw logs, notes and opaque payloads are withheld where identified. JSON snapshots embedded in legacy prompts are sanitized before truncation. Evidence references, MITRE technique IDs, rule IDs and hashes remain available for reasoning. Original records stay in AlistoIR; no alias map is sent to a provider or written to a log.

The model is instructed to report privacy-related evidence gaps and keep aliases out of actionable IOC recommendations. Invalid UTF-8, unsupported message content, serialization failures and excessive structured nesting block the request instead of sending the original payload. All providers receive sanitized content, including a provider labeled local whose URL may point outside the network.

This is data minimization, not a guarantee that arbitrary prose contains no sensitive information. Unrecognized personal or business names, unusual encodings and custom fields require review. If policy prohibits exporting any SOC data, use an internally hosted model with outbound network controls. Threat-intelligence API lookups are separate from LLM requests and are not covered by this boundary.

Validation uses synthetic fixtures only; no customer evidence or live provider requests are required. Model outputs retain aliases; analysts should use the linked local evidence for original values.
