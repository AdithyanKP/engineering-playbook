# Chat Summarization at Scale

## Problem

A chat application contains a large number of messages including text, images, and other content.

Requirements:

* Store all messages as the source of truth.
* Allow users or owners to generate a summary of an entire conversation.
* Avoid sending all messages to the LLM every time a summary is requested.
* Keep LLM cost and latency manageable as conversations grow.

---

## Naive Approach

When a user requests a summary:

```text
All Messages
    ↓
LLM
    ↓
Summary
```

### Issues

* Expensive for large chats.
* High latency.
* Limited by model context window.
* Reprocesses the same messages repeatedly.

---

## Running Summary Approach

```text
Current Summary
+
New Messages
    ↓
LLM
    ↓
Updated Summary
```

### Benefits

* Simple implementation.
* Fast retrieval.

### Problems

* Summary may grow indefinitely.
* Information can be lost during repeated compression.
* Requires careful prompt design to keep summary size bounded.

---

## Chunk-Based Summarization

Messages are grouped into chunks.

Example:

```text
Messages 1-300
    ↓
Summary A

Messages 301-600
    ↓
Summary B

Messages 601-900
    ↓
Summary C
```

Store summaries separately from messages.

### Benefits

* Each message is summarized only once.
* Scales well for large conversations.
* Lower repeated processing cost.

### Tradeoffs

* Requires background processing.
* Multiple summary records must be managed.

---

## Summary Generation Flow

When a user requests a summary:

### Step 1

Fetch all existing chunk summaries.

```text
Summary A
Summary B
Summary C
```

### Step 2

Find unsummarized messages.

Example:

```text
901-950
```

Generate:

```text
Summary D
```

### Step 3

Combine summaries.

```text
Summary A
Summary B
Summary C
Summary D
```

### Step 4

Generate an overall summary.

```text
Chunk Summaries
    ↓
LLM
    ↓
Overall Chat Summary
```

---

## Hierarchical Summarization

For extremely large conversations:

```text
Messages
    ↓
Level 1 Summaries

Level 1 Summaries
    ↓
Level 2 Summaries

Level 2 Summaries
    ↓
Master Summary
```

This prevents loading hundreds or thousands of chunk summaries at once.

---

## Relationship with RAG

RAG solves a different problem.

Example:

```text
Why did we choose PostgreSQL?
```

RAG:

```text
Question
    ↓
Vector Search
    ↓
Relevant Messages
    ↓
LLM
```

This works for specific questions.

For:

```text
Summarize the entire conversation
```

RAG alone is insufficient because there is no specific retrieval query.

Hierarchical summarization is more appropriate.

---

## Cost Considerations

### Small Chunks

```text
50 Messages per Chunk
```

Pros:

* Small context per request.

Cons:

* Many LLM calls.

### Large Chunks

```text
300-500 Messages per Chunk
```

Pros:

* Fewer LLM calls.
* Simpler management.

Cons:

* Larger context windows.

Important principle:

> Each raw message should ideally be processed only once.

---

## Alternative Approaches

### On-Demand Summarization

```text
User Requests Summary
    ↓
Read Entire Chat
    ↓
LLM
```

Pros:

* No background cost.

Cons:

* High latency.
* Expensive for large chats.

### Precomputed Master Summary

Maintain a master summary periodically.

```text
Chunk Summaries
    ↓
Master Summary
```

Pros:

* Instant retrieval.

Cons:

* Additional background processing.

---

## Key Learnings

1. Large systems do not continuously send entire chat histories to LLMs.
2. Summaries should remain bounded in size.
3. RAG is useful for question answering, not full conversation summaries.
4. Hierarchical summarization is a common scalability pattern.
5. The main optimization is ensuring that each message is processed as few times as possible.
6. Engineering decisions should be evaluated using cost, latency, scalability, and complexity tradeoffs rather than implementation convenience.
