# Stop Notice: Missing Required Context

## Work item
Rebuild Product Listing Screen (Catalog Browse Page) — Risk tier: high

## Status
This run is halted before implementation. Per `workflow.yaml` stop conditions (`missing_required_context: true`) and `policy.yaml` (high tier requires human review and security review, and `auto_merge_allowed: false`), no file edits to the Catalog listing page have been made.

## Why
The hydrated context and target file map for this run contain only static front-end assets (Bootstrap/jQuery/Popper CSS and JS vendor files, WebForms compatibility shims, docker-compose files, PRODUCT.md, README.md). None of the following required artifacts — all explicitly in scope for this work item — were provided:

- The Catalog listing page markup/code-behind (`.aspx` / `.aspx.cs` / `.aspx.designer.cs`)
- `ICatalogService` / `ICatalogItemService` interfaces and implementations (EF6-backed and mock)
- `CatalogItem`, `CatalogBrand`, `CatalogType` model/entity definitions
- `Web.config` (governs mock vs. EF6/SQL Server selection)
- The Details/Edit/Delete pages linked from the listing page
- `AGENTS.md` and `repo-contract.yaml` (required reading before planning per `instructions.md`)
- Any `test/`/`Tests/` project content for the Catalog module

## Why the harness must not proceed anyway
- `instructions.md` requires reading `AGENTS.md` and `repo-contract.yaml` before planning — neither is present.
- `instructions.md` requires verifying import/export alignment for every import against real context — there is no real context for the Catalog data-access layer or models to verify against.
- `instructions.md` requires preserving compatibility contracts (existing exported helpers, public function signatures, API/dependency shapes) — these cannot be preserved if they are unknown.
- The acceptance criteria require preserving Details/Edit/Delete navigation and mock-vs-EF6 parity — neither can be verified without the corresponding files.
- Fabricating file paths, service signatures, or model shapes to produce a plan or edits would violate "must not... claim success without evidence" and would risk directly regressing production behavior on a high-risk-tier item that disallows auto-merge and requires security review.

## Required action
Re-hydrate the workspace/context to include the seven items enumerated in `RESEARCH.md`'s "Recommended implementation direction" section, then re-run planning and implementation. No PLAN.md, code changes, EVIDENCE.md, or PR body will be produced in this run beyond this stop notice, consistent with the `missing_required_context` stop condition.
