# Tuttofy Documentation

## Overview

Tuttofy is an online AI avatar tutor platform that helps teachers, tutors, and subject-matter experts turn their persona, teaching style, and knowledge into interactive AI tutors.

In the current product version, Tuttofy uses Tavus as the avatar conversation provider. Tavus runs the live avatar experience, while Tuttofy continues to manage user identity, tutor settings, learning structure, materials, and student learning flows.

The core Tuttofy web app is built with Next.js. Clerk handles authentication and sessions, Neon stores the main application data, Upstash Redis supports caching or rate limiting needs, Cloudflare R2 is used for object storage such as learning materials or files, and Stripe is used as the payment provider and the foundation for teacher wallet/payout flows through Stripe Connect.

The internal admin portal or system lives in a separate application. This documentation repository focuses on product behavior for the core Tuttofy app used by tutors and learners.

This documentation repository is the internal source of truth for explaining how Tuttofy features work. It is intended to help align founders, product, design, and engineering.

## Who Tuttofy Serves

Tuttofy is designed for:

- Teachers who want to expand their teaching reach through AI avatar tutors
- Tutors who want to provide structured or on-demand learning experiences
- Knowledge experts who want to package their expertise into guided learning experiences
- Students or learners who want access to tutors with subject-specific capabilities

Tutors on the platform may cover subjects such as mathematics, English, history, coding, and other specialized knowledge domains.

## Current Platform Model

Each tutor in Tuttofy has a public profile, defined capabilities, a knowledge scope, learning materials, and conversation guardrails. However, the main learning experience now centers on `course`, not directly on a tutor in general.

Learners can browse available tutors, review tutor profile details such as `about the teacher`, `experience`, and `certificates`, then choose a relevant `course`. Before actually joining a course, learners must go through the `exploration` stage so Tuttofy can understand their learning goals and prepare more personalized context.

## Course-Based Learning Architecture

In Tuttofy, tutors create a `course` first, for example `Elementary Mathematics Grades 1-3`. Each course has specific learning goals, scope, guardrails, and learning paths.

Inside each course, learners always have two interaction paths:

- `Module Step Learning`
- `Course Free Conversation`

Both paths use the course context and the learner's personal context so conversations do not drift too far from the defined learning goals.

## Exploration Stage and Learning Path

Before joining a course, learners must enter the `exploration` stage.

During this stage, learners:

- Read the tutor profile and course details first
- Write their learning goal in a textarea
- Ask AI to generate a `personalized learning path`
- Review the learning path result as long-form text
- Download a `PDF` version
- Watch an AI video that explains the learning journey they will go through if they use the course
- Decide whether they want to `join this course`

This learning path is stored as the `user-course enrollment context`, so it is not only preview material but also becomes the official learning context for the user in that course.

## Learning Mode 1: Module Step Learning

`Module Step Learning` is the structured learning flow inside a course.

In this mode, the tutor prepares an ordered sequence of `module step` or `milestone` items from the beginning to the end. Learners move through the steps one by one based on the course curriculum prepared by the tutor.

Each `module step` may include:

- Avatar-based learning sessions
- Optional learning materials
- Downloadable files
- Progress tracking
- Completion status
- A summary after the step is completed

This mode is intended for learners who want a guided, sequential learning experience with a recap after each stage.

Example:

A mathematics tutor creates a course named `Elementary Mathematics Grades 1-3` with steps such as:

- Introduction to numbers and basic operations
- Addition and subtraction
- Basic multiplication
- Simple division
- Final practice

The learner completes the steps one by one, then receives a summary after each step is completed.

## Learning Mode 2: Course Free Conversation

`Course Free Conversation` is a flexible learning flow that still stays within the boundaries of a specific course.

In this mode, learners can start a conversation with the AI tutor avatar based on questions, exercises, or topics that are still relevant to the course they are taking.

The conversation is not globally free-form. Its discussion scope must remain within:

- The course goals and guardrails
- The user's personal learning path
- The user's progress in that course

This mode is useful for:

- Homework help that is still relevant to the course
- More personalized explanations
- Additional practice support
- Clarification of specific course topics
- Open discussion that remains guided

Example:

A student taking the `Elementary Mathematics Grades 1-3` course asks, `Can you help me practice simple multiplication?`

The AI tutor can answer, provide practice, adapt the explanation to the user's learning path, and keep the interaction within that course scope.

## Core Platform Concepts

### AI Tutor Personas

Tutors can create AI tutor personas that represent their identity, teaching approach, and learning style. This persona shapes how the avatar communicates with learners.

### Tutors, Teachers, and Experts

For documentation purposes, `tutor` is the main product term. A tutor may be a teacher, private tutor, coach, or subject-matter expert who delivers knowledge through Tuttofy.

### Course, Module Step, and Summary

`Course` represents the main container for the learning experience. `Module step` or `milestone` represents an individual learning step, avatar session, or checkpoint inside a course. After a step is completed, the system generates a `summary` as a recap of the learning result before the user continues to the next step.

### Learning Materials

Tutors can attach learning content and downloadable resources to support avatar sessions and the structured learning process.

### Guardrails and Knowledge Scope

Each learning experience must stay within approved subject boundaries. Guardrails define what the avatar may or may not do, while knowledge scope defines the domain and content boundaries the tutor can discuss. In the new model, the main guardrail applies at the `course` level, then is reinforced by the user's personal `learning path`.

## Documentation Goals

This repository is used to document Tuttofy features consistently so each feature page can eventually explain:

- What the feature is
- Why the feature exists
- Who uses the feature
- How the feature works
- The main user flow
- Business rules
- Important data
- Edge cases
- Related features
- Additional product or technical notes

## Documentation Conventions

The current documentation foundation follows these conventions:

- English documentation lives in `docs/en`
- Indonesian documentation lives in `docs/id`
- Each feature should ideally be documented in one Markdown page
- English and Indonesian pages should mirror each other structurally
- Feature file names should use a stable kebab-case format
- Feature pages should follow the shared template in `templates/feature-template.md`
- Every feature or system page must include at least one Mermaid diagram
- Mermaid diagrams are a priority, not optional decoration, because visual explanations should help readers understand the document faster
- Use `flowchart` for product flows, navigation, and system boundaries
- Use `sequenceDiagram` for actor-to-system interactions such as sign-in, API exchanges, or verification flows
- If a document covers both process behavior and interactions, include both a flowchart and sequence diagram when helpful
- Keep Mermaid labels concise so diagrams are easy to read in Markdown renderers
- Technical notes should remain concise unless a later phase expands them

## Available Documents

- [Tech Stack](./tech-stack.md)
- [Authentication](./authentication.md)
- [Onboarding](./onboarding.md)
- [Family Account](./family-account.md)
- [Course Discovery and Join](./course-discovery-and-join.md)
- [Course Learning Experience](./course-learning-experience.md)
- [Teacher Personalization](./teacher-personalization.md)
- [Teacher Wallet](./teacher-wallet.md)

## Feature Documentation Plan

This first phase does not fully document every feature yet. Planned feature documentation areas include:

- Authentication
- Onboarding
- Family account
- User profile
- Teacher profile
- Teacher personalization
- Teacher wallet
- Course discovery
- Create course
- Create module step
- Upload learning material
- Student learning progress
- Course free conversation
- Avatar conversation session
- Payment or subscription
- Guardrails and knowledge scope
- Download material
- Admin management

Full payment or subscription documentation still needs to be added after the business model is finalized, but the teacher wallet and payout foundation is now documented in `Teacher Wallet`.
