# Executive Summary: Custom AI Models at a Fraction of the Cost

## The idea
Large language models are remarkably good at writing code, debugging, solving problems,
summarising and drafting text. Out of the box, their answers can sometimes feel generic when a
task calls for a specific voice, format or domain. Retraining an entire model to close that gap
is expensive and demands specialist hardware.

**LoRA** offers a lighter alternative. The original model stays untouched, and we train a small
add-on, called an adapter, that teaches it the new behaviour. The adapter is only about 0.1% of
the model's size and consists of small extra weight matrices added to some of the model's linear layers.

## How organizations can use it
- **Consistent brand voice:** customer replies, reports and emails that follow the company's tone and structure.
- **Domain-specific tasks:** ticket triage, document summarisation, classification, and policy or knowledge Q&A.
- **Specialised language and terminology:** teaching the model the jargon, acronyms and conventions of a field or business, such as legal, finance, healthcare or engineering.
- **One model, many specialists:** each team or use case gets its own small adapter, which can be swapped in seconds.

## Why it matters
| Impact | Detail |
|---|---|
| **Lower cost** | Training a small fraction of the parameters takes far less GPU time and memory. |
| **Faster iteration** | Small adapters train quickly, so it is easy to test, correct and retrain. |
| **Easy to manage** | An adapter is a tiny fraction of the size of a full model, which runs to gigabytes, so it is simple to version, store and roll back. |
| **Platform-agnostic** | The approach is not tied to one technology stack. The same implementation can be adapted to Snowflake, and the concept scales with the hardware behind it. |

## What was built
A complete, tested pipeline that turns example data into a trained adapter. It covers data
preparation, the LoRA training engine (PyTorch Lightning), adapter saving, and tools for checking
a dataset before training. Everything is driven by configuration, so moving from a laptop to cloud
GPUs requires no code changes.

## Summary of results
- The pipeline ran end to end on a laptop: data in, model trained, adapter saved, then reloaded to generate text.
- Only **0.11%** of the model (540K of 494M parameters) was trained, which produced a **2 MB** adapter.
- Retraining was done locally on a bite-sized dataset and still produced a **measurable gain**: about **8% lower validation loss** (the standard measure of how well the model predicts the correct answer) on examples it had never seen. An encouraging early result, and a step in the right direction.
- **What changed:** the base model itself was left untouched. Only the 2 MB adapter was trained, and it nudged the model toward the target answers, so it now predicts the correct responses a little more confidently on examples it had never seen.
- The hands-on build also gave a working command of the full technique: preparing datasets, training the model and checking the results. The groundwork is in place, so the approach can now be applied to real use cases.

**Next:** train on a bigger dataset using a larger GPT-style model on a GPU, and get a brief sense of how the same workflow runs on Snowflake.

*The code is being worked on for some feature additions and will be made available soon.*
