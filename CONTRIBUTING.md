# Contributing to The Missing Layers

Thank you for considering a contribution. This project is in its early stage: a vision document seeking feedback, not a mature codebase seeking pull requests. The most valuable contributions right now are **ideas, criticism, and expertise**.

## What we need most

### Critical feedback on the whitepaper

The whitepaper has been through two rounds of adversarial review, but it has not yet been reviewed by someone who builds A2A agents for a living. If that's you, your perspective is the single most valuable thing this project can receive.

Open an [Issue](../../issues) with the label `whitepaper-feedback` and tell us:

- Where did you stop reading, and why?
- Which claims don't hold up in your experience?
- What existing work should we reference that we've missed?
- What would make this useful for your work?

### Domain expertise

The Agent Boundary Protocol (ABP) proposes domain-specific confidence thresholds (medical, financial, creative) that are currently placeholders. If you work in a regulated domain and can help calibrate these, or tell us why the approach is wrong, please open a Discussion.

### Open design questions

The whitepaper explicitly flags several questions it cannot answer alone:

- **Capability boundary:** Where does "what an agent can do" end and "how it does it" begin? This is the single most important open question
- **Sybil resistance:** How do we prevent operators from distributing credits across many agents to circumvent decay?
- **Diversity measurement:** How do we measure meaningful diversity (not just syntactic variety) in an agent ecosystem?
- **Formal verification:** The safety constraints need formal specification (TLA+, Alloy, or similar)

If any of these is in your area of expertise, we want to hear from you.

## How to contribute

### For feedback and discussion

1. **Issues** for specific feedback on the whitepaper, identified problems, or missing references
2. **Discussions** for open-ended questions, alternative approaches, or exploratory ideas

### For future specification work

As the project moves from vision document to Agent Boundary Protocol (ABP) specification, we will need:

- Protocol designers who can translate the three-tier model into A2A extension format
- Security researchers who can build a formal threat model
- Developers who can prototype a validator implementation

This work hasn't started yet. If you want to be involved when it does, open a Discussion or reach out to the author.

## What we don't need right now

- Code contributions (there is no codebase yet)
- Logo design or branding
- Marketing or social media help

## Code of conduct

Be direct, be honest, be constructive. The best contributions to this project so far have been uncomfortable questions that exposed design flaws. That's the culture we want to maintain.

## License

All contributions to this project are licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
