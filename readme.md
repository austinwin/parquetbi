# AGENTS.md

## Project mission

This repository contains an old QSearch codebase built with NiceGUI.  
That legacy code is reference material only.

The target direction is a **full rewrite** into a new project folder:

`qsearch.new`

The new implementation must be developed inside `qsearch.new` and must not depend on NiceGUI at runtime.

---

## Current architecture reality

Legacy app:
- Built with NiceGUI
- Main map-related legacy files include:
  - `qsearchmap.js`
  - `qsearchmap.py`
- UI interactions and core app flow are tightly coupled to NiceGUI

This coupling is considered technical debt and should not be carried into the new architecture.

---

## Target architecture

Create and maintain this structure inside the repository:

```text
qsearch.new/
  backend/
  frontend/
Backend

Required stack:

Python

FastAPI

Frontend

Required stack:

Next.js

Tailwind CSS

shadcn/ui or an equivalent code-owned component system

Environment constraints

Do not use Docker

Use a local Python virtual environment

Prefer Homebrew Python if Python setup is needed

Install Node.js locally

Use direct local development commands only

Non-goals

Do not:

keep NiceGUI as part of the new runtime architecture

patch the old app and call that the rewrite

mix frontend and backend responsibilities the way the old app did

introduce Docker

introduce microservices

introduce unnecessary infra complexity

add major new dependencies unless they clearly improve the target architecture

Working model

When assigned work in this repository, follow this default sequence unless the user explicitly overrides it.

Inspect the existing repository structure

Identify legacy NiceGUI dependencies and coupling points

Identify reusable domain logic, data flow, and map behavior concepts

Create or update qsearch.new/backend

Create or update qsearch.new/frontend

Implement the requested feature or migration step in the new architecture

Validate with relevant commands

Fix errors

Re-run validation

Repeat until the task is complete or a true blocker is reached

Do not stop at the first failure.

Legacy code handling rules

The old code is for reference only.

You may:

read old files

extract business rules

extract API/data flow ideas

extract map interaction concepts

extract naming or domain terminology

You should avoid:

copying NiceGUI-specific patterns into the new app

preserving old coupling between UI and backend logic

migrating dead code just because it already exists

dragging legacy abstractions into the rewrite without justification

When migrating anything from the old code, prefer:

small clean rewrites

explicit interfaces

typed request/response models

simple components

clear backend/frontend separation

Backend guidance

Use FastAPI as the primary backend framework.

Preferred backend defaults:

app/ package layout

Pydantic models for request/response schemas

clear separation between:

routers

services

repositories or data-access logic

schemas/models

environment settings file or config module

minimal, production-sane structure

Suggested backend shape:

backend/
  app/
    api/
    core/
    models/
    schemas/
    services/
    db/
    main.py
  tests/
  requirements.txt

If a better Python packaging structure is used, keep it simple and consistent.

Avoid:

bloated abstractions

premature plugin systems

fake enterprise layers with no practical value

Frontend guidance

Use Next.js as the frontend framework.

Required frontend defaults:

TypeScript

App Router

Tailwind CSS

shadcn/ui or equivalent code-owned component library

Preferred frontend structure:

route-driven app structure

reusable UI components

shared API client layer

clear separation between:

page/layout concerns

UI components

data-fetching logic

feature modules

Suggested frontend shape:

frontend/
  src/
    app/
    components/
    features/
    lib/
    hooks/
    types/

Avoid:

giant page files

business logic hidden inside UI components

duplicate API request code everywhere

styling chaos with no reusable patterns

UI rules

Favor:

clean enterprise-style UI

consistent spacing

consistent component variants

predictable form patterns

predictable search/filter/table patterns

Do not add random libraries when simple components are enough.

Default frontend choices should stay lightweight:

Next.js

Tailwind CSS

shadcn/ui

small targeted libraries only when justified

API boundary rules

The frontend and backend must remain clearly separated.

Rules:

frontend should call backend via explicit HTTP APIs

backend owns business rules, data logic, orchestration, and processing

frontend owns UI, routing, view state, and user interaction

do not hide core business logic in frontend helpers

do not duplicate validation logic carelessly across layers

If shared contracts are needed, prefer generated or clearly synchronized API types.

Validation rules

Codex should verify its work whenever possible.

After meaningful changes, run the relevant checks for the touched area.

Backend validation

Prefer running:

environment setup checks

dependency install checks

import checks

app startup checks

tests if present

lint/type checks if configured

Examples:

cd qsearch.new/backend
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python -m uvicorn app.main:app --reload

If there is a test suite, run it.

Frontend validation

Prefer running:

dependency install

lint

type-check if configured

build

local startup if needed

Examples:

cd qsearch.new/frontend
npm install
npm run lint
npm run build
npm run dev

If the package manager is already defined in the project, use that consistently.

Required iteration behavior

For implementation tasks, use this loop:

make a focused change

run validation

inspect failures carefully

fix root cause, not just symptoms

rerun validation

repeat until clean

Do not declare success while known errors remain.

Do not stop after scaffolding if the scaffold does not actually run.

Do not leave broken imports, broken startup, or obvious configuration issues unresolved unless blocked by something external.

Completion standard

A task is not complete unless the requested work is actually usable.

For the rewrite/scaffold direction, completion generally means:

qsearch.new/backend exists and runs

qsearch.new/frontend exists and runs

Tailwind is configured

component system is configured

local non-Docker setup is in place

frontend can call backend through a defined path

no obvious unresolved startup/build/import issues remain

If something is incomplete, state exactly what remains.

Blocker policy

Only stop when there is a real blocker.

A real blocker is something like:

missing credentials that are truly required

missing external service access

missing source files

contradictory requirements that prevent implementation

If blocked, report:

exact blocker

what was attempted

what failed

the smallest next input needed

Do not ask for confirmation for obvious engineering next steps.

Decision rules

Prefer:

simple

explicit

typed

testable

local-first development

maintainable folder structure

small clean modules

Avoid:

unnecessary abstractions

giant refactors with no validation

magic behavior

hidden coupling

rewriting unrelated parts of the repo unless needed for the task

Migration intent

The rewrite is not a visual reskin.
It is an architectural reset.

The purpose is to move from:

NiceGUI-driven coupling
to:

FastAPI backend

Next.js frontend

clean separation of concerns

Every meaningful implementation decision should support that direction.
