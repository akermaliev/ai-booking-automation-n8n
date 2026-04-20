# Architecture

## Purpose

This system automates conversational booking operations with a human-in-the-loop approval step. It is designed for vacation rental scenarios where AI can collect and validate request data, while final payment verification and approval remain with an administrator.

## Why the Workflows Are Split

The solution is separated into three workflows to keep responsibilities clear and maintainable:

- **Intake workflow** handles guest conversation, availability checks, and structured request creation.
- **Notification workflow** records requests and alerts admin operators.
- **Approval workflow** executes operator decisions and downstream actions.

This separation reduces coupling, simplifies testing, and makes replacements easier when integrations change.

## Role of the AI Agent

The AI agent manages guest dialogue, asks for missing data, applies business constraints, and controls escalation timing. It does not confirm payment and does not finalize bookings independently.

## Role of Memory

Chat memory keeps conversation context across user messages so the agent can continue naturally, avoid re-asking completed fields, and collect missing details progressively.

## Role of Calendar Tools

Calendar tools provide property-level availability checks. The intake and approval workflows both use property-specific calendar routing, preventing calendar mix-ups and enforcing clear inventory handling.

## Role of Admin Notification

The notification workflow receives structured request payloads, logs them in a sheet, and sends Telegram action buttons. This creates a visible operational queue for admin review.

## Role of Approval Workflow

The approval workflow processes admin actions (approve/reject/details), updates request status, creates calendar events for approved bookings, and triggers user messaging through ManyChat.

## Separation of Concerns

- **Intake**: conversational intelligence and data collection
- **Notification**: operational visibility and queueing
- **Approval**: human decision execution and side effects

This architecture is effective for automation systems because it keeps AI interaction logic independent from irreversible business operations, improving reliability, observability, and reusability.
