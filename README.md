# Healthcare Agents & Conductor Workers

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
- Access to an [Orkes Conductor](https://orkes.io/) server (cloud or local). To get started quickly, create a free account for the Developer Edition at [https://developer.orkescloud.com/](https://developer.orkescloud.com/). After signing up, obtain your application credentials by navigating to **Access Control > Application** in the Orkes Cloud dashboard. Use these credentials to configure your connection to Conductor.
- API keys for Orkes Conductor (set as environment variables)
- [OpenAI API key](https://platform.openai.com/api-keys). Sign up or log in to your OpenAI account and generate an API key at this link.

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

You should see output like this if everything is working and the workers are polling for work:

```
Connected to Conductor ✅
INFO TaskWorker healthcare_provider_finder initialized with concurrency of 10 and poll interval of 300
INFO TaskWorker communication_drafter initialized with concurrency of 10 and poll interval of 300
INFO TaskWorker medical_system_navigator initialized with concurrency of 10 and poll interval of 300
INFO TaskWorker prescription_transition_manager initialized with concurrency of 10 and poll interval of 300
```

This means the workers are polling for work from Conductor.

## Registering Worker Tasks in Conductor

Before your workers can process tasks, you must register each worker task in Conductor. This tells Conductor that these workers exist and are available to handle work. Registration can be done through the Conductor UI or programmatically using the Conductor SDK.

**Important:** The name of each worker task in Conductor must exactly match the `taskDefName` specified in your worker files. This is how Conductor knows which worker to assign to which task.

**Examples:**
- In `ConductorWorkers/workers.js`, you will find workers like:
  - `taskDefName: 'healthcare_provider_finder'`
  - `taskDefName: 'communication_drafter'`
  - `taskDefName: 'medical_system_navigator'`
  - `taskDefName: 'prescription_transition_manager'`

When registering tasks in Conductor, use these exact names:
- `healthcare_provider_finder`
- `communication_drafter`
- `medical_system_navigator`
- `prescription_transition_manager`

You can register these tasks:
- **Via the Conductor UI:** Go to the Task Definitions section and add each task by name.
- **Via the Conductor SDK:** Use the SDK's task registration methods to define each task by name.

Once registered, your workers will be recognized by Conductor and will poll for work as soon as you start them.

## How It Works

- Each agent is registered as a worker for a specific task type in Conductor.
- When a workflow in Conductor schedules a task (e.g., `healthcare_provider_finder`), the corresponding agent is invoked.
- Agents use OpenAI (via LangChain) and, where needed, call external APIs for up-to-date information.

## Customization

- To add or modify agents, edit the files in `LangChainAgents/`.
- To add new workers or change polling behavior, edit `ConductorWorkers/workers.js`.

