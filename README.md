# CareMatch Agent

An agentic AI system that automates care inquiry processing for elderly care platforms. Built as a technical case study demonstrating how AI agents can transform family-caregiver matching from a manual, time-intensive process into an intelligent, conversational experience.

## Problem

When families search for elderly care, the experience is often:

| Current State | Impact |
|:--------------|:-------|
| Manual intake calls take 20-30 minutes | Operations bottleneck |
| Matching requires experienced staff | Inconsistent quality, hard to scale |
| Families wait hours or days for options | Lost conversions, frustration |
| Peak times create backlogs | Missed opportunities |

Care platforms need to scale their intake capacity without sacrificing the empathy and personalization that families deserve during a difficult decision.

## Solution

CareMatch Agent is an autonomous AI system that:

1. **Conducts empathetic intake conversations** — gathering care requirements through natural dialogue
2. **Extracts structured requirements** — location, care type, languages, medical needs, schedule, budget
3. **Searches and ranks caregivers** — using weighted matching algorithms
4. **Explains recommendations** — transparency on why each caregiver fits
5. **Schedules consultations** — books calls or caregiver introductions
6. **Escalates edge cases** — routes complex situations to human specialists with full context

### Why Agentic?

Unlike traditional chatbots with fixed decision trees, CareMatch Agent uses a **tool-calling architecture** that allows it to:

- Dynamically decide which actions to take based on conversation context
- Execute multi-step workflows autonomously
- Maintain goal-oriented behavior across long conversations
- Gracefully handle unexpected inputs and edge cases

## Architecture

```mermaid
flowchart TB
    subgraph Frontend
        A[React Frontend<br>Chat UI • Caregiver Cards • Scheduling]
    end

    subgraph API
        B[Express.js API Gateway<br>/chat • /caregivers • /sessions]
    end

    subgraph Agent[Agent Orchestrator]
        C[LLM Core<br>Claude]
        D[Memory<br>Redis]
        E[Tool Registry<br>• searchCaregivers<br>• checkAvailability<br>• scheduleCall<br>• escalateToHuman<br>• sendSummaryEmail]
    end

    subgraph Data[Data Layer]
        F[PostgreSQL<br>Caregivers, Sessions]
        G[Redis<br>Cache, Sessions]
        H[External APIs<br>Calendar, Email]
    end

    A --> B
    B --> Agent
    Agent --> Data
```

## Features

### Core Agent Capabilities

- **Conversational Intake** — Natural dialogue that gathers requirements without feeling like a form
- **Intelligent Matching** — Weighted scoring based on languages, specializations, ratings, availability
- **Contextual Memory** — Maintains conversation history across sessions
- **Graceful Escalation** — Recognizes when human intervention is needed

### Tools

| Tool | Description |
|:-----|:------------|
| `searchCaregivers` | Queries database with filters, returns ranked matches with explanations |
| `checkAvailability` | Verifies real-time caregiver availability for specific dates |
| `scheduleCall` | Books consultation calls via calendar integration |
| `escalateToHuman` | Routes to specialists with full conversation context |
| `sendSummaryEmail` | Sends care requirements summary to family |

### Frontend

- Real-time message streaming (SSE)
- Caregiver profile cards with match explanations
- Integrated scheduling calendar
- Mobile-responsive design

## Tech Stack

| Layer | Technology |
|:------|:-----------|
| Frontend | React, TypeScript, Tailwind CSS |
| Backend | Node.js, Express.js, TypeScript |
| Agent | Anthropic Claude API, Tool Calling |
| Database | PostgreSQL, TypeORM |
| Cache/Sessions | Redis |
| Infrastructure | Docker, Docker Compose |
| External APIs | Google Calendar, SendGrid |

## Getting Started

### Prerequisites

- Node.js 20+
- Docker & Docker Compose
- Anthropic API key

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/carematch-agent.git
cd carematch-agent

# Copy environment variables
cp .env.example .env

# Add your API keys to .env
# ANTHROPIC_API_KEY=your_key_here

# Start services
docker-compose up -d

# Install dependencies
npm install

# Run database migrations
npm run db:migrate

# Seed sample data
npm run db:seed

# Start development server
npm run dev
```

### Environment Variables

# API Keys
ANTHROPIC_API_KEY=

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/carematch

# Redis
REDIS_URL=redis://localhost:6379

# External Services (optional)
GOOGLE_CALENDAR_API_KEY=
SENDGRID_API_KEY=

## Project Structure

- `src/`
  - `agent/`
    - `CareMatchAgent.ts` — Main agent orchestrator
    - `ToolRegistry.ts` — Tool management
    - `ConversationMemory.ts` — Session memory
    - `prompts/`
      - `system.ts` — System prompt
    - `tools/`
      - `searchCaregivers.ts`
      - `checkAvailability.ts`
      - `scheduleCall.ts`
      - `escalateToHuman.ts`
  - `api/`
    - `routes/`
      - `chat.ts`
      - `caregivers.ts`
      - `sessions.ts`
    - `middleware/`
  - `entities/`
    - `Caregiver.ts`
    - `ChatSession.ts`
    - `Family.ts`
  - `repositories/`
  - `services/`
- `frontend/`
  - `src/`
    - `components/`
      - `CareMatchChat.tsx`
      - `MessageBubble.tsx`
      - `CaregiverCard.tsx`
      - `ScheduleModal.tsx`
    - `hooks/`
      - `useAgentChat.ts`
    - `App.tsx`
  - `package.json`
- `docker-compose.yml`
- `package.json`
- `README.md`

### Demo Conversation

```
🤖 Agent:  Hello! I'm here to help you find the right caregiver for your 
           loved one. Could you tell me a bit about who needs care and 
           where they're located?

👤 Family: My mother lives in Berlin. She's 78 and has early-stage 
           dementia. We need someone during the day.

🤖 Agent:  Thank you for sharing that. Caring for a parent with dementia 
           is a journey, and finding the right support makes a real 
           difference.

           A few questions to find the best matches:
           1. Does your mother speak any languages besides German?
           2. What hours do you need coverage?

👤 Family: She speaks German and Polish. We need someone 9am-5pm, 
           Monday to Friday.

🤖 Agent:  Let me search for caregivers who match these needs...

           I found 4 excellent caregivers in Berlin:

           1. Anna K. ⭐ 4.9 (47 reviews)
              • 8 years experience, dementia specialist
              • Speaks German & Polish
              • €180/day | Available Dec 1
              
           2. Maria S. ⭐ 4.8 (32 reviews)
              • 5 years experience
              • Speaks German, Polish, Ukrainian
              • €165/day | Available immediately

           Would you like to schedule an introduction call with 
           either caregiver?
```

### Success Metrics

| Metric | Target |
|:-----|:------------|
| Inquiry → Recommendation | < 5 minutes |
| Requirement extraction accuracy | > 90% |
| Scheduling conversion rate | > 40% |
| Escalation rate | < 15% |
| User satisfaction (CSAT) | > 4.2/5 |

### Roadmap
- Multi-channel support — WhatsApp, voice (phone), website widget	
- Proactive follow-ups — Re-engage families who didn't schedule	
-	Caregiver-side agent — Help caregivers manage availability and respond	
-	Outcome learning — Improve matching based on successful placements	
-	Document processing — Parse care assessments and medical documents

### Design Decisions
Why Claude for the Agent Core?
- Superior instruction following for complex multi-turn conversations	
- Native tool calling with structured outputs	
- Strong empathy and tone control for sensitive healthcare context
- Reliable JSON schema adherence for tool parameters

### Why Separate Tools vs. RAG?
For this use case, explicit tool calling outperforms RAG because:
1.	Structured queries — Caregiver search needs precise filters, not semantic similarity	
2.	Real-time data — Availability must be checked live, not from embeddings	
3. Actions required — Scheduling and escalation need deterministic execution
4. Auditability — Tool calls create clear logs for compliance

### Why Redis for Memory?
- Fast read/write for real-time chat	
- TTL support for automatic session expiration	
- Pub/sub capability for future multi-instance scaling	
- Simple JSON storage for conversation history
