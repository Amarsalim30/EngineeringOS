# Architecture

## Overview

## System Design

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Frontend  │────▶│   FastAPI   │────▶│  PostgreSQL  │
└─────────────┘     └─────────────┘     └─────────────┘
                          │
                          ▼
                    ┌─────────────┐
                    │ SAP Service │
                    │   Layer     │
                    └─────────────┘
```

## Components

## Data Flow

## Decisions

- [[ADR|Architecture Decision Records]]
