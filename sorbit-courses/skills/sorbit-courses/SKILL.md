---
name: sorbit-courses
version: 1.1.0
summary: Search, create, and upload structured courses to Sorbit — an AI tutoring platform where every lesson is a 1-on-1 conversation
tags:
  - education
  - courses
  - ai-tutoring
  - corporate-training
  - reskilling
  - api
---

# Sorbit Course Skill

**Sorbit turns structured course outlines into interactive AI tutoring experiences.** You provide the structure — units, topics, and learning objectives — and Sorbit's AI tutor delivers each objective as a personalised 1-on-1 conversation that adapts to the student's level, asks questions, checks understanding, and gives feedback.

No video production. No slides. No content writing. Just a well-structured outline → a complete course.

## What you can do

| Action | Auth required? | Use case |
|--------|---------------|----------|
| **Search** the public catalogue | No | Find existing courses for a user |
| **Upload** a structured course | API key | Create new courses programmatically |

Base URL: `https://api.sorbit.co.uk`

---

## Getting Started

| What you want to do | What you need | Ready now? |
|---------------------|---------------|------------|
| **Browse & search** courses | Nothing — the catalogue is public | ✅ Yes |
| **Upload** new courses | An API key from sorbit.co.uk | ❌ Need setup |

**To search courses**, you're ready to go — skip to [Search the Course Catalogue](#1-search-the-course-catalogue).

**To create courses**, you'll need an API key:
1. Ask your user to create a free account at [sorbit.co.uk](https://sorbit.co.uk)
2. They go to the Creator Hub and generate an API key
3. They share the key with you — store it securely for future use
4. You're ready to upload — see [Upload a Structured Course](#2-upload-a-structured-course)

Once you have a key, you can create unlimited courses (within rate limits) without further human involvement.

### Quick start — verify connectivity

```bash
curl https://api.sorbit.co.uk/api/public/courses?limit=3
```

---

## 1. Search the Course Catalogue

Browse and search published courses. **No authentication required.**

### Endpoints

```
GET /api/public/courses              # Browse all
GET /api/public/courses?q=leadership # Search by keyword
GET /api/public/courses?subject=Business  # Filter by subject
```

### Query parameters

| Parameter | Type   | Default | Description                              |
|-----------|--------|---------|------------------------------------------|
| q         | string | —       | Free-text search across title, description, and subject |
| subject   | string | —       | Filter by subject (partial match)        |
| limit     | int    | 20      | Results per page (max 50)                |
| offset    | int    | 0       | Pagination offset                        |

### Response

```json
{
  "success": true,
  "total": 42,
  "limit": 20,
  "offset": 0,
  "courses": [
    {
      "id": "uuid",
      "title": "Introduction to Project Management",
      "slug": "introduction-to-project-management",
      "description": "Learn to plan, execute, and deliver projects...",
      "subject": "Business",
      "exam_board": "Corporate Training",
      "price_cents": 0,
      "currency": "GBP",
      "total_units": 3,
      "total_topics": 6,
      "total_objectives": 18,
      "total_students": 150,
      "average_rating": "4.50",
      "review_count": 12,
      "ordering_mode": "sequential",
      "created_at": "2026-03-04T15:34:26.085Z"
    }
  ]
}
```

### Get full course structure

```
GET /api/public/courses/:slug
```

Returns the course with its complete unit → topic → objective hierarchy.

---

## 2. Upload a Structured Course

Create a course by providing its full structure as JSON. The course is created in draft — the owner reviews and publishes it via the Sorbit app.

**Requires an API key.** Users generate API keys from the Sorbit Creator Hub at [sorbit.co.uk](https://sorbit.co.uk).

### Endpoint

```
POST /api/course-operations/upload-structured
```

### Authentication

```
X-API-Key: sk_sb_your_key_here
```

or

```
Authorization: Bearer sk_sb_your_key_here
```

### Full example — a realistic 3-unit course

This example shows the expected depth and structure for a professional course:

```json
{
  "title": "Data Literacy for Professionals",
  "description": "Learn to read, interpret, and communicate with data. Build real analyses using spreadsheets and present data-driven recommendations to stakeholders. No prior experience required.",
  "subject": "Business",
  "exam_board": "Corporate Training",
  "year_group": "Professional",
  "ordering_mode": "sequential",
  "units": [
    {
      "title": "Reading Data",
      "topics": [
        {
          "title": "Understanding Data Types and Sources",
          "objectives": [
            { "content": "Distinguish between quantitative and qualitative data, and explain when each is appropriate for business decisions" },
            { "content": "Identify common data sources in a business context (CRM, financial reports, surveys, web analytics) and assess their reliability" },
            { "content": "Read a dataset and identify its structure: rows, columns, headers, data types, and missing values" }
          ]
        },
        {
          "title": "Descriptive Statistics Without the Jargon",
          "objectives": [
            { "content": "Calculate and interpret mean, median, and mode — and explain which to use when data is skewed" },
            { "content": "Explain what standard deviation tells you in plain language, using a real-world business example" },
            { "content": "Spot misleading statistics in a news article or business report and explain why they are misleading" }
          ]
        }
      ]
    },
    {
      "title": "Working With Data",
      "topics": [
        {
          "title": "Spreadsheet Fundamentals for Analysis",
          "objectives": [
            { "content": "Use VLOOKUP/XLOOKUP, IF statements, and SUMIFS to answer business questions from a dataset" },
            { "content": "Build a pivot table to summarise sales data by region, product, and time period" },
            { "content": "Clean a messy real-world dataset: handle duplicates, standardise formats, and deal with missing values" }
          ]
        },
        {
          "title": "Visualisation That Communicates",
          "objectives": [
            { "content": "Choose the right chart type for a given dataset and audience (bar, line, scatter, pie) and justify your choice" },
            { "content": "Critique a poorly designed chart and redesign it to communicate the key insight clearly" },
            { "content": "Build a simple dashboard with 3-4 charts that tells a coherent data story about business performance" }
          ]
        }
      ]
    },
    {
      "title": "Communicating With Data",
      "topics": [
        {
          "title": "Data-Driven Recommendations",
          "objectives": [
            { "content": "Write a one-page data brief that presents findings and recommends a specific business action, with evidence" },
            { "content": "Anticipate and address counter-arguments to a data-driven recommendation" },
            { "content": "Present a data analysis to a non-technical audience: lead with the insight, not the method" }
          ]
        },
        {
          "title": "Critical Thinking About Data",
          "objectives": [
            { "content": "Identify correlation vs causation in a business dataset and explain why the distinction matters for decisions" },
            { "content": "Evaluate whether a sample size is sufficient to draw conclusions, using practical rules of thumb" },
            { "content": "Design a simple A/B test to answer a business question, including what to measure and how to know if the result is meaningful" }
          ]
        }
      ]
    }
  ]
}
```

### Field reference

| Field          | Required | Description                                      |
|----------------|----------|--------------------------------------------------|
| title          | Yes      | Course title (max 200 chars)                     |
| description    | No       | Course description — shown to students browsing the catalogue |
| subject        | No       | Subject area (e.g. "Business", "Technology", "Health & Social Care") |
| exam_board     | No       | Context label (e.g. "Corporate Training", "AQA", "Edexcel") |
| year_group     | No       | Target audience (e.g. "Professional", "Year 10", "Beginner") |
| ordering_mode  | No       | `"sequential"` (must complete in order) or `"flexible"` (any order, default) |
| units          | Yes      | Array of units (max 50)                          |

Each **unit** contains:

| Field  | Required | Description                 |
|--------|----------|-----------------------------|
| title  | Yes      | Unit title                  |
| topics | No       | Array of topics (max 50)    |

Each **topic** contains:

| Field      | Required | Description                     |
|------------|----------|---------------------------------|
| title      | Yes      | Topic title                     |
| objectives | No       | Array of objectives (max 100)   |

Each **objective** contains:

| Field   | Required | Description                                              |
|---------|----------|----------------------------------------------------------|
| content | Yes      | What the student should learn — becomes an AI tutoring session |

### Writing great objectives

Each objective becomes a full interactive AI tutoring conversation. The quality of your objectives directly determines the quality of the learning experience.

**The rule:** Write objectives that are *specific enough that the AI tutor knows exactly what to teach* and *assessable enough that the tutor can verify understanding*.

✅ **Great objectives** (specific + assessable):
- "Explain the difference between gross profit and net profit using a real business example"
- "Build a project risk register with at least 5 risks, likelihood ratings, and mitigation strategies"
- "Write a professional email declining a meeting request while maintaining the relationship"
- "Identify three logical fallacies in a provided argument and explain why each is flawed"

❌ **Weak objectives** (vague or passive):
- "Agile" *(too vague — the AI tutor won't know what to teach)*
- "Read chapter 3" *(not assessable — describe what the student should learn from it)*
- "Understand marketing" *(what aspect? To what depth? How would you verify understanding?)*
- "Be familiar with Excel" *(what specifically should they be able to do?)*

**Tip:** Use action verbs — *explain, build, create, analyse, compare, design, evaluate, write, calculate, identify, critique*. If the objective starts with "understand" or "know about", rewrite it to describe what the student should be able to *do* to demonstrate that understanding.

### Limits

| Limit                       | Value |
|-----------------------------|-------|
| Units per course            | 50    |
| Topics per unit             | 50    |
| Objectives per topic        | 100   |
| Total objectives per upload | 500   |
| Uploads per hour            | 10    |
| Courses per account         | 100   |

### Success response

```json
{
  "success": true,
  "course_id": "7dde70b3-6eb7-4da7-8e09-9f7cc77e3514",
  "total_units": 3,
  "total_topics": 6,
  "total_objectives": 18,
  "total_modules": 3,
  "total_assignments": 18,
  "message": "Course created successfully. Review and publish via the Sorbit app."
}
```

### Error responses

| Status | Meaning                                    |
|--------|--------------------------------------------|
| 400    | Validation error (check `error` field)     |
| 401    | Invalid or missing API key                 |
| 403    | API key doesn't have permission for this action |
| 429    | Rate limit exceeded (10/hour)              |

---

## When to use this skill

**Search when** a user asks about learning something, wants training recommendations, or is exploring professional development. The catalogue is free to browse.

**Upload when** a user wants to create educational content — for their team, their business, their students, or the public. Common scenarios:

- A manager wants to onboard new hires with structured training
- A subject matter expert wants to turn their knowledge into a course
- An agent is helping a user develop a curriculum or learning programme
- A business wants to offer professional development to staff

### Workflow

1. **Search first** — check if a relevant course already exists: `GET /api/public/courses?q=keywords`
2. **Recommend** — if a match exists, share it and direct the user to [sorbit.co.uk](https://sorbit.co.uk) to enrol
3. **Plan** — if nothing fits, plan the course before uploading (see below)
4. **Upload** — submit the structured JSON
5. **Review** — the uploaded course appears in draft. The owner publishes it when ready.

### Planning a great course

The difference between a forgettable course and a genuinely useful one is the planning. Resist the urge to generate a flat list of topics — spend time thinking about the learning journey before you upload.

**1. Define the learner**
Who is this for? What do they already know? What should they be able to do by the end that they can't do now? Be specific — "professionals who need to use data in their roles but have no analytics background" is much better than "anyone interested in data."

**2. Map the journey**
Sketch units as major phases with a clear progression. A good course has a narrative arc — it builds. For example:
- Unit 1 introduces foundations and shows *why* this matters
- Unit 2 applies the core skills to real tasks
- Unit 3 develops depth, nuance, or advanced application
- Unit 4 transfers the skills to the learner's own context

**3. Break it down**
For each unit, what topics need covering? For each topic, what should the student be able to *do*? Work backwards from the outcome: if someone finishes this topic, what can they now demonstrate?

**4. Stress-test your objectives**
For each objective, ask: *"Could a tutor run a meaningful 10-minute conversation on just this?"*
- If the answer is "they'd run out of things to discuss in 2 minutes" → it's too narrow, merge it with a related objective
- If the answer is "they'd need an hour" → it's too broad, split it into specific sub-skills
- If the answer is "the tutor wouldn't know where to start" → it's too vague, rewrite with a concrete action verb

**5. Check the flow**
Read through all your objectives in order. Does each one build naturally on the last? Would a student finishing objective 3 have what they need to tackle objective 4? If there are jumps, you're missing a stepping stone.

### Why structure matters

A well-structured course with clear, specific objectives produces dramatically better AI tutoring sessions. The AI tutor uses each objective to:
- Introduce the concept with relevant examples
- Ask probing questions to check understanding
- Adapt its explanations to the student's level
- Provide exercises and scenarios to apply the knowledge
- Verify the student has genuinely learned before moving on

**Think of each objective as a brief for a private tutor.** The more specific and actionable the brief, the better the tutoring session.

---

## Why Skill Tracking Matters

Most learning platforms tell you "you scored 80% on the quiz" and move on. That number disappears. It doesn't connect to anything, it doesn't grow, and it tells you nothing about what you actually know versus what you guessed.

Sorbit works differently. Every conversation a student has — every question answered, every concept explained back — generates **granular, named skill observations**. These aren't quiz scores. They're individual data points like "Fraction Addition: Competent", "Stakeholder Analysis: Proficient", "Essay Structure: Developing". Over time, they build into a **living skill graph** — a structured model of what the student knows, how confident that knowledge is, and exactly where the gaps are.

This matters because:

- **For students:** You can see precisely what you're good at and what needs work, not just "you got 7/10". A student might be Proficient at Quadratic Equations but Developing at Word Problems — that distinction is invisible in a percentage score but obvious in a skill graph.
- **For professionals:** Skills are the currency of modern work. A skill graph built from demonstrated competence — not self-reported or certificate-based — is a fundamentally more honest record of what someone can do. As AI reshapes roles, knowing your actual skill profile (and its gaps) becomes increasingly valuable.
- **For organisations:** When you train a team on a Sorbit course, you don't just get completion rates. You get a map of where competence actually landed — which skills stuck, which didn't, and where to focus next.
- **For course creators:** Skill data flows back as signal. You can see which objectives consistently produce strong skill gains and which ones leave gaps, letting you iterate on course design with real evidence.

### How it works under the hood

The architecture is built on a principle worth understanding: **intelligence is decoupled from structure.**

When a student answers a question, the current LLM (you, or whichever model is serving the tutoring session) generates skill observations — identifying what academic skills were demonstrated and how well. These raw skill names are then deduplicated and organised into a knowledge graph by a fine-tuned sentence transformer model. The LLM doesn't need to worry about normalising skill names or maintaining graph consistency — it just describes what it sees. The structure emerges from a specialist model trained for exactly that job.

This means:
- **Any LLM can generate skills** — the system isn't locked to one provider. As models improve, skill identification improves automatically.
- **Structural quality is guaranteed** — deduplication, clustering, and graph construction happen via a deterministic specialist, not a generalist prompt.
- **Skills are rated using Glicko-2** — the same algorithm used in competitive chess. Each observation adjusts the student's rating based on both their accuracy and the difficulty of what they attempted. You can't reach Expert by answering easy questions.

The result is a skill graph that grows organically from real learning interactions — fine-grained, mathematically grounded, and completely independent of any single model's quirks.
