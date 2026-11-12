# SME Loan Origination Copilot: Architect deliverables (R2)

Group 1 capstone, FDE course. This is a design for an AI copilot that helps Northbridge Bank underwriters process SME loan applications. This repo holds the **Architect (R2)** deliverables: the architecture, the slide deck, the plan and a UX prototype. It contains no production code.

## The idea in one paragraph

An underwriter gets a complete, cited credit memo instead of a pile of PDFs. The system classifies and extracts documents, recomputes the numbers in code, matches findings to policy clauses, calls the existing ML scorecard and drafts the memo. **A human underwriter makes every decision.**

## Design rules

- The LLM never computes scores, amounts or eligibility. Code and the existing scorecard do.
- Only an underwriter can approve, reject or send back. This is enforced in code with an approval token, not by prompt.
- The system never communicates a credit decision to the customer.
- No gender, religion or PIN-code proxies. Every rejection reason maps to a policy clause through a reason-code registry.
- Synthetic data only. No personal data goes to external models or logs.

## Architecture at a glance

Eight layers: experience, gateway and input guardrails, orchestration (LangGraph with a Postgres checkpointer), agents (Intake, Document, Policy RAG, Credit memo, Action behind a human gate), knowledge (pgvector), decision tools (ML scorecard, rules, metric calculators), enterprise APIs through adapters, and governance and observability.

The target cloud is **Azure** (Azure OpenAI, Document Intelligence, Content Safety, Container Apps, PostgreSQL with pgvector, Blob Storage, Key Vault, Entra ID). AWS is documented as a fallback, with a comparison.

Indicative run cost is about ₹100 per application, or about ₹1.5 lakh a month at 1,500 applications. The estimate is ±40% and should be checked in the Azure Pricing Calculator.

## What is here

| Path | What it is |
|---|---|
| `R2_Deliverables/G1_Banking_Deck.pptx` | 25-slide deck on the UST template, including the Azure vs AWS comparison |
| `R2_Deliverables/G1_Banking_Architecture_v2.pdf` | 18-page architecture document: components, flows, security, cost, 7 ADRs, failure handling |
| `R2_Deliverables/G1_Banking_R2_Plan.pdf` | R2 plan: scope, timeline, dependencies, risks, Q&A prep |
| `R2_Deliverables/ux/` | Underwriter workbench prototype (`underwriter_workbench.html`) and screenshots |
| `R2_Deliverables/build/` | Scripts and diagram sources that regenerate the documents and deck |

`G1_Banking_Architecture.pdf` is the older AWS version. Use `_v2`.

## Rebuilding

Requires Python 3 with `python-pptx` and a PDF library, plus Microsoft Edge for rendering the diagrams.

```bash
cd R2_Deliverables/build
python build_docs.py   # architecture and plan PDFs
python build_deck.py   # deck (needs base_clean.pptx in the same folder)
```

`diagrams.html` holds the SVG diagrams. Open it with `#arch` or `#flow` to view each one.

## Open items

- Confirm the bank is on Azure with Entra ID, and that the needed Azure OpenAI models are available in Central India.
- Replace the "to fill" placeholders owned by R1, R3, R4 and R5.

## Key dates

- Review session: 20 Nov 2026
- Package due: 29 Nov 2026, 11:59 PM IST
- Presentation: 30 Nov 2026
