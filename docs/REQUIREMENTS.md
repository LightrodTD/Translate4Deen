# Translate4Deen — Requirements

Version: 0.3  
Date: 2026-09-14  
Status: Discussion draft; not an approved implementation baseline  
Product owner: Mehdi Syed  
Product name: Translate4Deen  
Repository: [LightrodTD/Translate4Deen](https://github.com/LightrodTD/Translate4Deen) (public)  
Repository location: `docs/REQUIREMENTS.md`

## 1. Purpose and document rules

Build a desktop application that turns a selected screen region into an Arabic–English reading experience and lets a user retain, organize, study, import, and export that material.

This document records the discussion so far. It is the proposed source of truth for product requirements in GitHub. It separates the user's decisions from assistant proposals; a suggestion in an earlier conversation is not automatically a commitment.

- **Confirmed:** explicitly requested or chosen by the user.
- **Proposed:** a recommended behavior or implementation safeguard awaiting review.
- **Open:** an unresolved choice that can change scope or architecture.
- **Deferred:** outside the proposed first release; may be reconsidered.

Requirement IDs remain stable when wording changes. Release priorities and acceptance criteria below are proposals until reviewed. Numeric quality, latency, and budget targets are intentionally unset until a baseline experiment.

## 2. Product goals

1. Make translating a screen region feel as direct as taking a screenshot.
2. Support individual hadiths and single/double-page reading.
3. Keep Arabic and English aligned at sentence or phrase level, including short diagram labels where feasible.
4. Support personal study through word/root analysis and a saved glossary.
5. Address classical and historical Arabic explicitly, while distinguishing translation, supporting evidence, and interpretation.
6. Keep running costs low for the developer and users.
7. Let the user select supported AI providers/models using their own credentials.
8. Preserve the ability to change AI execution and other architectural components later.

## 3. Confirmed decisions

| ID | Decision | Consequence |
| --- | --- | --- |
| DEC-001 | Actual desktop application, primarily macOS | Browser-only delivery does not meet the goal. |
| DEC-002 | Other desktop operating systems are desirable if practical | Avoid unnecessary platform coupling; release timing remains open. |
| DEC-003 | Single-user application; users affect only their own installation/data | No user-to-user communication, shared glossary, moderation, or collaboration features. |
| DEC-004 | Screenshot-style region selection followed by immediate translation | Capture must be part of the application workflow. |
| DEC-005 | Individual hadiths and one/two-page translations are primary inputs | These must be represented in evaluation samples. |
| DEC-006 | Arabic plus English, sentence or phrase alignment | Preserve the relationship between source and translated units. |
| DEC-007 | Save and organize readings; import and export | Persistent study material is central to the product. |
| DEC-008 | Root analysis and an editable personal glossary | Corrections must affect only the user's data; protection details remain open. |
| DEC-009 | Configurable translation appropriate to old Arabic texts | Translation style and historical/contextual treatment require explicit design. |
| DEC-010 | Users bring their own API credentials initially | Developer does not fund users' inference requests in the first architecture. |
| DEC-011 | Users choose among supported providers/models | Default suggestions must not lock users to one provider. |
| DEC-012 | Architecture should accommodate future changes | Define replaceable boundaries instead of binding UI/data to one API. |
| DEC-013 | Develop in GitHub and keep requirements with the project | Repository: `LightrodTD/Translate4Deen`, supplied by the user; currently public, with default branch `main`. |

## 4. Proposed first-release scope

### Core capabilities

- macOS region capture and a quick translation result.
- Individual report and single/double-page processing.
- Arabic transcription, aligned English, and source-image access.
- Correction of extracted Arabic and explicit retranslation.
- Local saved readings with basic organization and search.
- Word analysis and personal glossary entries with reversible edits.
- A small, tested set of provider integrations and model selection.
- A small set of translation profiles/settings.
- Image import, bounded PDF page import, readable export, and a portable backup.
- Useful behavior when offline, interrupted, or out of API quota.

### Proposed deferrals

- Fully local translation models, pending hardware/quality evaluation.
- Automated comparison across large historical corpora and parallel transmissions.
- Automatic verification of hadith attribution or authenticity.
- Large-book batch translation and background research agents.
- Automatic reconstruction of complex diagrams and marginal layouts.
- Cloud synchronization and app-funded billing.
- Dedicated third-party integrations such as Obsidian sync.

Windows/Linux timing remains open. Basic diagram phrase translation is part of the requested vision; advanced layout reconstruction is a separate scope choice.

## 5. Functional requirements and acceptance criteria

| ID | Requirement | Status | Proposed acceptance criterion |
| --- | --- | --- | --- |
| CAP-001 | Select a screen region and translate its Arabic directly | Confirmed | User starts capture, draws a region, and receives a result without manually saving/uploading a screenshot. |
| CAP-002 | Provide an app capture action and configurable global shortcut | Proposed | Both start the same flow; cancellation creates no API request. |
| CAP-003 | Handle permission denial, display scaling, and multiple monitors | Proposed | Tested configurations capture the selected region correctly; denied permission produces actionable guidance. |
| OCR-001 | Extract Arabic while retaining the original image | Confirmed capability; preservation rule proposed | Saved text can be inspected against the capture at readable zoom. |
| OCR-002 | Permit corrections without replacing original image/raw extraction | Proposed | User can inspect raw and corrected text and retranslate explicitly. |
| OCR-003 | Respect headings, report boundaries, reading order, and two-page order | Proposed | Sample pages have no silent missing, repeated, or interleaved blocks; user can correct detected order. |
| TRN-001 | Show Arabic and English aligned by sentence or phrase | Confirmed | Every displayed English unit links to its Arabic span; source order and text are preserved. |
| TRN-002 | Translate with passage context even when displaying short phrases | Proposed | A phrase view does not force isolated translation requests for every phrase. |
| TRN-003 | Offer configurable translation behavior | Confirmed | Settings are applied to new requests and recorded with the result. Exact controls: OPEN-005. |
| TRN-004 | Preserve uncertainty and separate commentary from translation | Proposed | Added explanations and alternatives are labeled; missing text is flagged instead of silently completed. |
| TRN-005 | Separate narrator chains, report body, headings, and notes where detected | Proposed | Collapsing a chain does not delete it or exclude it from a full export without an explicit choice. |
| TRN-006 | Preserve original results when models/settings change | Proposed | Retranslation creates an identifiable revision or alternative. Ordinary reopening makes no new API request. |
| LIB-001 | Save, organize, and reopen translated material | Confirmed | Saved content survives app restart and remains readable without network access. |
| LIB-002 | Store the personal reading library on the user's computer initially | Proposed | Core storage needs no developer-hosted account/database. |
| LIB-003 | Search Arabic/English and support basic grouping | Proposed | Search locates a known saved passage; Arabic search normalization does not change displayed source text. |
| LIB-004 | Associate book/page/report metadata without inventing sources | Proposed | Unknown fields stay unknown; suggested source matches are distinct from user-confirmed values. |
| LIB-005 | Save a whole page or selected reports linked to its capture | Proposed | Selecting one report retains a traceable connection to the source image and region. |
| GLS-001 | Analyze selected words and save entries to a personal glossary | Confirmed | Saved entry can be reopened with its originating passage. |
| GLS-002 | Distinguish surface form, lemma, root, morphology, sense, and preferred English | Proposed | Editing an English preference does not alter root letters. Unknown/ambiguous roots are representable. |
| GLS-003 | Permit personal corrections with protection against accidental changes | Confirmed goal; mechanism proposed | Original analysis and override remain distinguishable; previous value can be restored. |
| GLS-004 | Scope glossary preferences to applicable contexts | Proposed | Passage-specific edit does not silently redefine every derivative or rewrite existing readings. |
| IO-001 | Import and export user material | Confirmed | Supported inputs create normal reading items; exported readings open outside the app. Formats: OPEN-006. |
| IO-002 | Provide a versioned backup that preserves study data | Proposed | Restore recovers captures, text, alignment, metadata, notes, glossary, and revisions; credentials are excluded. |
| AI-001 | Let users configure their own provider credentials and model | Confirmed | Two configured supported models can be selected without changing library content. |
| AI-002 | Show only supported capabilities for a selected model | Proposed | A text-only model receives extracted text; unsupported image/structured-output actions are handled explicitly. |
| AI-003 | Keep requests, results, and error handling provider-independent internally | Confirmed architectural goal; boundary proposed | New adapter can be added without rewriting capture UI or saved reading schema. |
| AI-004 | Handle rate limits, unavailable models, invalid keys, and outages | Proposed | Capture is retained; retries are bounded; reset/retry timing is shown only when known. |
| AI-005 | Make provider or paid-model fallback explicit | Proposed | Failure cannot silently send content to another provider or incur a new paid route without prior user configuration. |
| AI-006 | Record usage and provide budget controls | Proposed | Known provider usage and app estimates are distinguished; the app does not claim to know account-wide remaining quota without evidence. |

## 6. Translation and historical-language policy

Historical suitability is a quality goal, not a promise that choosing an era proves the intended meaning of every passage.

Proposed rules:

- Translate the wording supplied in the capture. Label OCR recovery, supplied words, and explanatory additions.
- Preserve meaningful ambiguity instead of forcing an unsupported definitive interpretation.
- Distinguish root analysis from contextual meaning. A root-level semantic field does not automatically determine every derivative's English translation.
- Distinguish reported speaker/date, compilation date, edition date, and printed page. Unknown dates remain unknown.
- A source citation must point to evidence actually retrieved or supplied. Generated explanations are not verified evidence.
- Commentary and explanations attributed to the Ahl al-Bayt must identify the supporting report when offered as sourced material.
- Keep lexical evidence, transmitted explanations, and model inference visibly distinguishable.
- User-selected source preferences do not authorize silently changing the captured Arabic.

Proposed profiles for discussion: **Quick Read**, **Study**, **Literal**, and a later **Research** profile. Candidate controls include literal/readable output, ambiguity display, bracketed additions, transliteration, preserved religious terms, genre, approximate era, and glossary application. Names and defaults are not finalized.

## 7. User workflows to design

1. **First launch:** explain local storage and cloud processing; select provider; follow provider setup instructions; save key securely; choose supported model; run an explicitly initiated connection test.
2. **Quick capture:** start selection; capture; show progress and cancellation; display aligned result; copy, study, save, or close.
3. **Page study:** inspect layout; correct transcription/order; read aligned passages; analyze words; attach metadata; save page or selected report.
4. **Glossary correction:** inspect original analysis; edit the correct field; choose scope; preview impact; save reversible override.
5. **Quota exhausted:** preserve work; display the actual failure; allow later retry or explicit provider change; continue reading saved items.
6. **Import/export:** select supported files/pages; handle invalid files; import as normal readings; export selected material or a complete backup.

Open: temporary capture versus automatic capture history, floating window behavior, split-view direction, unsaved-close behavior, and the minimum number of interactions to save.

## 8. Data model requirements

These are logical records, not a committed database schema:

| Record | Information to retain |
| --- | --- |
| Capture | Original image/file reference, date, source regions, page order, optional duplicate fingerprint |
| Transcription | Raw extraction, corrected version, text spans, region mapping, extraction method, uncertainty |
| Translation | Arabic version reference, English units, alignment, settings, provider/model, timestamp, available usage |
| Reading item | Page/report grouping, title, collection/tags, notes, source metadata |
| Glossary entry | Surface form, lemma/root candidates, contextual sense, preferred English, scope, source passage, evidence |
| Revision | Prior value, new value, change type, timestamp, optional reason; same-user history only |
| Provider configuration | Provider identity, supported capabilities, model selection, reference to separately stored credentials |

Use stable internal IDs and versioned import/export formats. Persisting a model response alone is insufficient: the app must own its source/translation alignment and data format.

## 9. Nonfunctional and security requirements

All implementation details here are proposed safeguards unless already confirmed above.

- **Cost:** opening, organizing, searching, and exporting saved data makes no AI request. Analysis is requested when needed; bounded retries and reusable results prevent accidental repeated costs.
- **Responsiveness:** show progress and allow cancellation during long operations. Measure first-visible-result and completion latency separately. Targets remain OPEN-008.
- **Accuracy:** evaluate extraction and translation separately. Report omissions, negation errors, narrator/name errors, alignment faults, and unsupported additions. Fluent output alone is not sufficient.
- **Reliability:** interrupted saves/imports must not corrupt existing data. Database migrations and restore require recovery paths.
- **Credentials:** use the OS credential store where supported. Keys must not appear in logs, reading exports, backups, Git, or UI error messages.
- **Privacy:** show which provider receives images/text. Keep the personal glossary local and send only relevant entries. Define diagnostics/telemetry before adding them.
- **Input safety:** treat extracted text as document content, not executable instructions. Validate imported structures and model responses; render untrusted content safely.
- **Local integrity:** reversible glossary corrections and clear scope are required design goals. Password/biometric gates for edits remain undecided; local privacy does not remove the user's earlier request for strong protection.
- **Accessibility:** correct Arabic shaping and bidirectional layout, adjustable text size, keyboard navigation, and appropriate screen-reader labels.
- **Portability:** isolate screen capture, shortcuts, credentials, file access, and AI adapters from the reading/glossary domain.

## 10. Architecture boundaries — proposed, not a stack selection

1. Desktop presentation: capture controls, quick result, library, study view, settings.
2. Application/domain services: translation workflow, glossary rules, revisions, source alignment, save/export operations.
3. Extraction interface: interchangeable local OCR or remote image extraction.
4. AI provider interfaces: adapters for selected providers, with normalized results/errors and explicit capability differences.
5. Persistence interface: local database/files with versioned migrations and portable export.
6. OS integrations: capture permissions, display coordinates, shortcuts, and credential storage.

Initial execution path: desktop app → user's configured provider using their own key. A future local model or developer-operated gateway should fit behind the same domain-facing boundary. A hosted paid service would still require new billing/authentication work; an adapter does not eliminate that work.

No framework, programming language, database, OCR engine, model, or cloud platform has been selected. Tauri, Electron, SwiftUI, and Flutter were discussed as candidates only. Do not introduce microservices simply to make the architecture changeable.

## 11. Evaluation plan before selecting the stack/model

Build a small representative dataset, proposed initial size 12–20 captures:

- Short hadith; long hadith with narrator chain; dense one-page text; two-page spread.
- Vocalized and unvocalized Arabic; honorific symbols; headings and footnotes.
- Diagram labels, low-quality scans, cropped sentences, and ambiguous words.
- Different genres/eras where suitable examples are available.

The supplied screenshot headed **كتاب العقل والجهل**, printed page **٥**, containing seven numbered reports is the first example. Its book identity is not established by this image alone. Earlier assistant translations are illustrative, not reviewed ground truth.

Compare local OCR + text translation with direct image extraction/translation. Evaluate at least two affordable models where credentials permit. API evaluation requires configured keys; no paid experiment has been authorized by a budget yet.

| Dimension | Record |
| --- | --- |
| Extraction | Character errors under a documented normalization policy; omissions, order errors, names, negation, diacritics |
| Translation | Meaning preservation, alternatives, unsupported additions, terminology consistency, human corrections required |
| Alignment | Missing, duplicated, or mismatched source/English spans |
| Word analysis | Correct lemma/root candidates and explicit uncertainty against checked references |
| Experience | Time to first useful result, completion time, correction effort, failures |
| Cost | Actual input/output/reasoning usage when exposed, retries, total cost per acceptable result |

A knowledgeable Arabic reviewer or checked reference set is needed for credible accuracy claims; model agreement is not independent verification. Select numeric release gates after this baseline and before implementation acceptance.

## 12. Open decisions, in discussion order

| ID | Question to settle | Why now | Suggested starting point, not approved |
| --- | --- | --- | --- |
| OPEN-001 — Resolved | User supplied `LightrodTD/Translate4Deen` | Repository exists; current visibility is public | Use the supplied repository; do not change its visibility implicitly |
| OPEN-002 | What happens to captures before explicit Save? | Determines privacy, data lifecycle, and UI | Temporary result; explicit save; optionally recover interrupted work |
| OPEN-003 | Which macOS versions and Intel/Apple Silicon hardware? When other OSes? | Constrains framework and packaging | macOS first; gather actual hardware information |
| OPEN-004 | Floating result, workspace, or both? English left or stacked? | Defines the primary interaction | Quick floating result with expandable workspace; English left/Arabic right |
| OPEN-005 | Minimum profiles/settings and default fidelity policy? | Controls prompts, UI, and evaluation | Faithful readable translation with significant uncertainty flagged |
| OPEN-006 | Initial import/export formats and PDF limits? | Prevents unbounded document processing scope | PNG/JPEG; selected PDF pages; Markdown and portable backup; discuss PDF/Word export |
| OPEN-007 | Folders, tags, or both? Minimum source fields? | Defines saved-library behavior | Collections plus tags; optional book/volume/page/report number |
| OPEN-008 | Acceptable latency, cost, and error rates? | Makes success measurable | Set after a small benchmark; define evaluation spending budget first |
| OPEN-009 | How protected should glossary edits be? Required sources? | User explicitly requested strong protection | Advanced-edit toggle, scope preview, undo/history; discuss reauthentication |
| OPEN-010 | Initial providers/models and free-tier data-use policy? | Needed for evaluation and onboarding | Evaluate Gemini plus a second candidate; do not promise perpetual free quotas |
| OPEN-011 | Automatic historical evidence in v1 or later? | Largest likely scope expansion | Preserve provenance now; advanced retrieval later |
| OPEN-012 | Recovery, deletion, and backup behavior? | Avoids lost work | Explicit deletion; tested export/restore; discuss automatic backups |
| OPEN-013 | AI coding workflow, contribution policy, and licensing? | Defines repository practices and redistribution | Owner-reviewed small PRs; choose license before public distribution |

## 13. GitHub workflow and planned documentation

### Confirmed requirements-review workflow

The user requested stage-by-stage discussion, ongoing updates to this document, and all upcoming changes on a single branch for the user to merge later.

- Working branch: `docs/requirements-refinement`.
- Keep one draft pull request for the entire requirements refinement cycle; append commits after each set of decisions.
- Do not merge or enable auto-merge; the user will merge when ready.
- Update the affected requirement rows, acceptance criteria, open decisions, and change log together. Explicitly distinguish confirmed answers from suggestions and unresolved questions.
- Read the latest branch content before editing and preserve intervening user changes.
- PR #1 established the initial draft and was merged before this refinement cycle; its original branch was deleted. This working branch starts from the merged baseline.

### Discussion progress

| Stage | Topic | Status | Decisions to settle |
| --- | --- | --- | --- |
| 1 | Capture and saving | In discussion | Capture history/default retention, Save interaction, close/delete behavior, interrupted captures |
| 2 | Reading experience | Pending | Floating result/workspace, bilingual placement, alignment controls, corrections, selecting reports |
| 3 | Translation behavior | Pending | Profiles, narrator chains, honorifics, missing text, ambiguity, commentary |
| 4 | Glossary and root analysis | Pending | Word details, editable fields, override scope, protection, revisions, sources |
| 5 | Organization and portability | Pending | Collections/tags, metadata, search, import/export, PDF limits, backups |
| 6 | AI connections and costs | Pending | Initial integrations, key setup, model choice, limits, retries/fallback, usage display |
| 7 | Quality expectations | Pending | Latency/cost targets, material errors, historical evidence, evaluation protocol |
| 8 | Technical design | Pending | Hardware/OS support, frontend/backend, database, OCR, AI adapters, security, testing, distribution |

Stage 1 has no confirmed lifecycle decisions yet. Recent proposals included both explicit-save-only behavior and automatic local capture history with a separate curated library. Neither proposal has been selected; OPEN-002 remains open. Early hardware or evaluation questions may be raised before Stage 8 if needed to assess feasibility.

Proposed repository documents:

| Path | Purpose |
| --- | --- |
| `README.md` | Product overview, project status, setup entry point |
| `docs/REQUIREMENTS.md` | This living requirements document |
| `docs/ARCHITECTURE.md` | Stack and system design after evaluation |
| `docs/decisions/` | Short architecture decision records with alternatives and rationale |
| `docs/EVALUATION.md` | Dataset policy, runs, actual costs, findings, model versions |
| `docs/ROADMAP.md` | Milestones and issue links |

The repository contains a README and this requirements draft. The remaining documentation paths are proposed; they should be created when there is substantive content for them.

- Turn accepted requirement IDs into small GitHub issues with acceptance criteria.
- Reference those IDs in implementation PRs and update changed requirements with the code.
- Record major choices in decision records rather than burying rationale in chat.
- Add CI appropriate to the chosen stack: formatting, types/build, and meaningful tests of boundaries and persistence.
- Test Arabic layout manually with real captures, API error handling with deterministic fixtures, and export/restore with round-trip tests. Keep paid live evaluations explicit and bounded.
- Exclude credentials, private captures, personal databases, and provider responses containing user material from Git.
- Add redistributable test fixtures only after checking rights and privacy. Sample screenshots supplied in chat are not automatically public-repository assets.

## 14. Proposed milestones

| Milestone | Outcome | Exit condition |
| --- | --- | --- |
| M0 — Requirements baseline | Agree on core behavior and repository setup | Resolve blocking scope questions; user reviews and merges the requirements refinement PR |
| M1 — Extraction/translation experiment | Compare real samples before choosing engines | Record costs/errors/latency and choose initial candidate pipeline |
| M2 — UX and architecture | Clickable flow and documented stack decisions | Review capture, reading, glossary, saving, and quota behavior |
| M3 — Vertical slice | Capture → translate → save → reopen on macOS | Runs with a user key and preserves source alignment |
| M4 — Study features | Glossary, organization, import/export | Accepted core requirements and failure cases pass |
| M5 — Beta packaging | Installable app and onboarding | Fresh-machine install, permission flow, recovery, key safety, and quality gates verified |

## 15. Change log

| Version | Date | Change |
| --- | --- | --- |
| 0.1 | 2026-09-14 | Initial draft from the project discussion. Confirmed decisions separated from proposed behavior, scope, acceptance criteria, architecture boundaries, and open questions. GitHub repository not yet selected or created. |
| 0.2 | 2026-09-14 | Adopted Translate4Deen as the project name, recorded the supplied repository and its current public visibility, resolved OPEN-001, and prepared the draft for repository review. Product behavior remains under discussion. |
| 0.3 | 2026-09-14 | Recorded the user-directed single-branch, user-merged workflow and eight-stage discussion tracker. Began Stage 1; capture retention and saving behavior remain unresolved. |
