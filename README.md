# [WIP] Project LLMeta

## Project Overview

Project LLMeta is a lightweight WebXR metaverse experience that allows users to interact in a shared 3D space with AI-powered communication assistance.

## System Architecture

```mermaid
graph LR
    subgraph "User"
        UserBrowser["🌐 User's Browser"]
    end

    subgraph "Frontend"
        Client["<b>Client (Next.js)</b>"]
    end

    subgraph "Backend Services"
        Colyseus["<b>Multiplayer Server</b><br/>(Colyseus)"]
        LiveKit["<b>Media Server</b><br/>(LiveKit SFU)"]
        Supabase["<b>Auth & DB</b><br/>(Supabase)"]
        AIService["<b>AI Service</b><br/>(FastAPI, Whisper, LLM, Image Gen)"]
    end

    subgraph "AI Agents (Scalable)"
        AIAgentPool["🤖 <b>AI Agent Pool</b><br/>(1 Agent per User)"]
    end

    UserBrowser --> Client

    Client -- "Auth (JWT)" --> Supabase
    Client -- "Join with JWT" --> Colyseus
    Client -- "Join with JWT" --> LiveKit

    Colyseus -- "Public State (Images, etc.)" <--> Client
    Colyseus -- "Private Messages (Summaries, etc.)" --> Client
    LiveKit -- "Audio/Video Stream" <--> Client

    %% AI Agent Flow
    AIAgentPool -- "Connects as Client" --> LiveKit
    AIAgentPool -- "Connects as Client" --> Colyseus

    LiveKit -- "Nearby Audio Streams" --> AIAgentPool
    AIAgentPool -- "API Request (STT, Summary, Img Prompt)" --> AIService
    AIService -- "API Response (Text, Image URL)" --> AIAgentPool

    AIAgentPool -- "Update Public State (e.g., Shared Image URL)" --> Colyseus
    AIAgentPool -- "Send Private Message (e.g., Summary Text)" --> Colyseus
```
