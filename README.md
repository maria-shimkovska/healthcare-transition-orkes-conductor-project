# Node Starter – Healthcare Agents & Conductor Workers

![Orchestrating LangChain Agents](Orchestrating-LangChain-Agents-for-Production-with-Orkes-Conductor_RS-Edit.jpg)

This project is a Node.js starter template that demonstrates how to build and run AI-powered agents for healthcare-related tasks, integrated with Orkes Conductor for workflow automation. It includes several specialized agents for:

- **Healthcare Provider Finder**: Finds English-speaking primary care doctors (GPs) in the Netherlands.
- **Medical Email Drafter**: Drafts professional, administrative emails for medical purposes (e.g., appointment requests, prescription refills).
- **Dutch Healthcare System Navigator**: Answers questions about the Dutch healthcare system, using up-to-date knowledge when needed.
- **Prescription Transition Manager**: Provides checklists and guidance for moving prescriptions internationally (e.g., to the Netherlands).

## Project Structure

- `package.json` – Project dependencies and scripts
- `ConductorWorkers/workers.js` – Main entry point; registers and runs all agent workers with Orkes Conductor
- `LangChainAgents/` – Contains the agent definitions:
  - `DoctorFinder.js`
  - `EmailDrafter.js`
  - `MedicalSystemNavigator.js`
  - `PrescriptionTransitionManager.js`

## Prerequisites

- **Node.js** (v18+ recommended)
- **npm** (comes with Node.js)
- Access to an [Orkes Conductor](https://orkes.io/) server (cloud or local)
- API keys for Orkes Conductor (set as environment variables)
- (Optional) OpenAI API key for advanced LLM features

## Setup

1. **Clone the repository** (if not already):
   ```zsh
   git clone healthcare-transition-orkes-conductor-project
   cd healthcare-transition-orkes-conductor-project
   ```

2. **Install dependencies:**
   ```zsh
   npm install
   ```

3. **Configure environment variables:**
   Create a `.env` file in the project root with the following:
   ```env
   CONDUCTOR_SERVER_URL=<your-conductor-server-url>
   CONDUCTOR_AUTH_KEY=<your-conductor-api-key>
   CONDUCTOR_AUTH_SECRET=<your-conductor-api-secret>
   OPENAI_API_KEY=<your-openai-api-key>
   ```

## Running the Workers

Start the agent workers (they will connect to Conductor and begin polling for tasks):

```zsh
node ConductorWorkers/workers.js
```

You should see a message like `Connected to Conductor ✅` if the connection is successful.

## How It Works

- Each agent is registered as a worker for a specific task type in Conductor.
- When a workflow in Conductor schedules a task (e.g., `healthcare_provider_finder`), the corresponding agent is invoked.
- Agents use OpenAI (via LangChain) and, where needed, call external APIs for up-to-date information.

## Customization

- To add or modify agents, edit the files in `LangChainAgents/`.
- To add new workers or change polling behavior, edit `ConductorWorkers/workers.js`.

## License

MIT (or specify your license here)

---

For questions or support, contact the project maintainer.
