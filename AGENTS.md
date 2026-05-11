# Project Context

## Overview

This project is a full-stack map-based web application.

The application allows users to view locations on an interactive map, interact with markers, open contextual popups, and view detailed information about locations.

The project is being built as a modular monolith.

---

# Tech Stack

## Frontend
- SvelteKit
- TypeScript
- TailwindCSS
- Leaflet
- OpenStreetMap

## Backend
- Ktor
- Kotlin
- JWT Authentication

## Database
- MongoDB

---

# Frontend Architecture

## Principles
- Component-driven architecture
- Reusable UI components
- Isolated feature modules
- Minimal prop drilling
- Store-based shared state

## Folder Structure

src/
├── routes/
├── lib/
│   ├── components/
│   ├── stores/
│   ├── api/
│   ├── types/
│   └── utils/

---

# Map Architecture

## Responsibilities

### MapView
Responsible for:
- initializing Leaflet
- map lifecycle
- tile layers
- map events

Should NOT:
- fetch business data
- contain popup business logic
- contain modal logic

### Marker Components
Responsible for:
- rendering markers
- emitting selection events
- lightweight interactions

### Popup Components
Responsible for:
- small contextual information
- lightweight actions

### Detail Drawer / Modal
Responsible for:
- detailed location information
- images
- extended interactions

---

# State Management

Use Svelte stores for:
- selected marker
- selected place
- map zoom
- map center
- UI state

Avoid deeply nested props.

---

# API Architecture

Frontend should communicate with backend using:
- fetch wrappers
- centralized API layer

Avoid direct fetch calls scattered across components.

---

# UI/UX Guidelines

## Design Goals
- minimal
- map-first experience
- clean spacing
- responsive
- mobile-friendly

## Interaction Flow

User Flow:
Map
→ Marker
→ Popup
→ Detail Drawer

Avoid popup-inside-popup patterns.

---

# Coding Guidelines

## Components
- single responsibility
- avoid files larger than ~300 lines
- reusable where practical

## Naming
Use descriptive names:
- MapView
- PlaceMarker
- PlacePopup
- PlaceDrawer

Avoid generic names like:
- Component1
- UtilsHelper

---

# Development Workflow

Build features incrementally.

Preferred workflow:
1. Build isolated component
2. Test independently
3. Integrate into feature
4. Refactor if needed

Avoid generating large parts of the app at once.

---

# Initial Features

## Phase 1
- Fullscreen map
- Marker rendering
- Popup interaction
- Detail drawer
- Basic authentication

## Phase 2
- Search
- Clustering
- User-generated locations
- Saved places

## Phase 3
- Realtime updates
- Notifications
- Route planning