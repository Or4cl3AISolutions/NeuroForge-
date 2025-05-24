# NeuroForge PoC: Implementation Tasks

This document breaks down the Proof-of-Concept (PoC) for NeuroForge into actionable development tasks, categorized by development area. Each task includes a relative effort estimation (Small, Medium, Large) and a priority (High, Medium, Low).

## 1. Environment & Setup

*   **Task 1.1:** Initialize Git repository for the PoC.
    *   Effort: Small
    *   Priority: High
*   **Task 1.2:** Define basic project structure (folders for backend services, UI, scripts, documentation).
    *   Effort: Small
    *   Priority: High
*   **Task 1.3:** Secure API key for the chosen external LLM service (e.g., OpenAI, Google AI Studio).
    *   Effort: Small
    *   Priority: High
*   **Task 1.4:** Set up a Neo4j instance (local Docker container or cloud instance like AuraDB free tier) for the Knowledge Graph.
    *   Effort: Small
    *   Priority: High
*   **Task 1.5:** Choose backend framework for services (e.g., FastAPI for Python, Express.js for Node.js) and set up basic project(s).
    *   Effort: Medium
    *   Priority: High
*   **Task 1.6:** Decide on UI approach (Basic Web Page or CLI) and set up the initial environment (e.g., simple HTML file, or basic Node.js/Python script for CLI).
    *   Effort: Small
    *   Priority: High
*   **Task 1.7:** Create a shared configuration mechanism for services (e.g., environment variables, simple config file) for things like LLM API key, KG connection details.
    *   Effort: Small
    *   Priority: Medium

## 2. Knowledge Graph (KG) Setup

*   **Task 2.1:** Define Cypher queries or use Neo4j Browser to manually create 2-3 `Concept` nodes (e.g., "Machine Learning," "Neural Networks," "Python Basics") with `conceptId`, `name`, and `description` properties.
    *   Effort: Small
    *   Priority: High
*   **Task 2.2:** Define Cypher queries or use Neo4j Browser to manually create 2-3 `ContentModule` nodes (e.g., "Intro to ML Video," "Neural Networks Explained Article") with `moduleId`, `title`, and `type` properties.
    *   Effort: Small
    *   Priority: High
*   **Task 2.3:** Define Cypher queries or use Neo4j Browser to manually create `TEACHES_CONCEPT` relationships between the `ContentModule` nodes and their respective `Concept` nodes.
    *   Effort: Small
    *   Priority: High
*   **Task 2.4:** Document the simple KG schema and the specific data populated for the PoC.
    *   Effort: Small
    *   Priority: Medium

## 3. Backend Services Development

### 3.1 Knowledge Graph Service (KGS)

*   **Task 3.1.1:** Implement a basic HTTP server (e.g., using FastAPI/Flask in Python, Express in Node.js).
    *   Effort: Small
    *   Priority: High
*   **Task 3.1.2:** Implement Neo4j connection logic (driver setup, connection string from config).
    *   Effort: Small
    *   Priority: High
*   **Task 3.1.3:** Implement API endpoint `GET /concepts?name={concept_name}`:
    *   Receives `concept_name` as a query parameter.
    *   Queries Neo4j to find a `Concept` node by its `name` property.
    *   Returns concept details (`conceptId`, `name`, `description`) as JSON.
    *   Effort: Medium
    *   Priority: High
*   **Task 3.1.4:** Implement API endpoint `GET /concepts/{conceptId}/modules`:
    *   Receives `conceptId` as a path parameter.
    *   Queries Neo4j to find `ContentModule` nodes connected to the specified `Concept` via `TEACHES_CONCEPT` relationship.
    *   Returns a list of module details (`moduleId`, `title`, `type`) as JSON.
    *   Effort: Medium
    *   Priority: High
*   **Task 3.1.5:** Basic error handling for API endpoints (e.g., concept not found).
    *   Effort: Small
    *   Priority: Medium

### 3.2 AI Mentor Service (AIMS)

*   **Task 3.2.1:** Implement a basic HTTP server.
    *   Effort: Small
    *   Priority: High
*   **Task 3.2.2:** Implement logic to receive user query (e.g., "What is [Concept X]?") and mock `userId` via an API endpoint (e.g., `POST /mentor/query`).
    *   Effort: Medium
    *   Priority: High
*   **Task 3.2.3:** Implement HTTP client logic to call the Knowledge Graph Service's `GET /concepts?name={concept_name}` endpoint.
    *   Effort: Small
    *   Priority: High
*   **Task 3.2.4:** Implement prompt construction logic:
    *   Take the `description` from the KG Service response.
    *   Formulate a prompt for the LLM (e.g., "Explain the following concept to a beginner: [Concept X Description from KG].").
    *   Effort: Medium
    *   Priority: High
*   **Task 3.2.5:** Implement HTTP client logic to call the chosen external LLM API with the constructed prompt and API key.
    *   Effort: Medium
    *   Priority: High
*   **Task 3.2.6:** Implement HTTP client logic to call the Knowledge Graph Service's `GET /concepts/{conceptId}/modules` endpoint using the `conceptId` obtained from the initial KG call.
    *   Effort: Small
    *   Priority: High
*   **Task 3.2.7:** Implement logic to format the LLM response and the list of suggested modules into a structured JSON response for the UI.
    *   Effort: Medium
    *   Priority: High
*   **Task 3.2.8:** Implement HTTP client logic to send a simplified interaction log to the (mocked) Cognitive Profiling Service endpoint (`POST /profiling/interactions`).
    *   Effort: Small
    *   Priority: Medium
*   **Task 3.2.9:** Basic error handling (e.g., KG service unavailable, LLM API error).
    *   Effort: Small
    *   Priority: Medium

### 3.3 Mocked Services

*   **Task 3.3.1:** Create a Mock Cognitive Profiling Service:
    *   Implement a single HTTP endpoint `POST /profiling/interactions`.
    *   This endpoint should accept any JSON payload.
    *   It should log the received payload to the console (for verification).
    *   It should return a 202 Accepted status code with a simple JSON body (e.g., `{"message": "Interaction event accepted"}`).
    *   Effort: Small
    *   Priority: High
*   **Task 3.3.2:** Create a Mock Content Delivery Service:
    *   Implement a single HTTP endpoint `GET /modules/{moduleId}/content` (or similar, to be called by UI).
    *   This endpoint should accept a `moduleId`.
    *   It should return a predefined JSON response (e.g., `{"moduleId": "moduleId_from_request", "content_type": "placeholder", "data": "Placeholder for content of module [moduleId_from_request]"}` or `{"url": "link_to_youtube_video"}`).
    *   Effort: Small
    *   Priority: High

## 4. Frontend User Interface (UI) Development (Choose ONE path)

### Path A: Basic Web Page UI

*   **Task 4.A.1:** Create a simple HTML page (`index.html`) with:
    *   An input field for the user's question.
    *   A "Submit" button.
    *   A `div` area to display AI explanations.
    *   A `div` area to display content suggestions.
    *   A `div` area to display (mocked) content module content.
    *   Effort: Small
    *   Priority: High
*   **Task 4.A.2:** Write basic JavaScript (`script.js`) to:
    *   Handle the "Submit" button click.
    *   Get the user's question from the input field.
    *   Make an asynchronous POST request (e.g., using `fetch` API) to the AI Mentor Service's `/mentor/query` endpoint with the question.
    *   Effort: Medium
    *   Priority: High
*   **Task 4.A.3:** In JavaScript, handle the response from the AI Mentor Service:
    *   Display the AI-generated explanation in the designated `div`.
    *   Display the content suggestions (e.g., as clickable links or buttons) in another `div`.
    *   Effort: Medium
    *   Priority: High
*   **Task 4.A.4:** In JavaScript, handle clicks on content suggestion links/buttons:
    *   Make an asynchronous GET request to the (mocked) Content Delivery Service endpoint.
    *   Display the returned placeholder content or link in the content display `div`.
    *   (Optional PoC Step) Send an interaction log to the AI Mentor Service (or directly to mocked profiling service) for this action.
    *   Effort: Medium
    *   Priority: Medium

### Path B: Command-Line Interface (CLI) UI

*   **Task 4.B.1:** Create a basic CLI script (e.g., Python with `input()` and `requests`, or Node.js with `readline` and `axios`).
    *   Effort: Small
    *   Priority: High
*   **Task 4.B.2:** Implement logic to prompt the user to enter their question.
    *   Effort: Small
    *   Priority: High
*   **Task 4.B.3:** Make a POST request to the AI Mentor Service's `/mentor/query` endpoint with the user's question.
    *   Effort: Small
    *   Priority: High
*   **Task 4.B.4:** Handle the response from the AI Mentor Service:
    *   Print the AI-generated explanation to the console.
    *   Print the content suggestions, perhaps numbered for selection.
    *   Effort: Small
    *   Priority: High
*   **Task 4.B.5:** Implement logic to allow the user to select a suggested content module (e.g., by number).
    *   Make a GET request to the (mocked) Content Delivery Service endpoint.
    *   Print the returned placeholder content or link.
    *   (Optional PoC Step) Send an interaction log for this action.
    *   Effort: Medium
    *   Priority: Medium

## 5. Integration & Testing

*   **Task 5.1:** Test Knowledge Graph Service endpoints independently using a tool like `curl` or Postman.
    *   Effort: Small
    *   Priority: High
*   **Task 5.2:** Test AI Mentor Service endpoint, ensuring it correctly calls KG Service, LLM API, and mocked Cognitive Profiling Service. Verify prompt construction and response formatting.
    *   Effort: Medium
    *   Priority: High
*   **Task 5.3:** Test Mocked Services endpoints independently.
    *   Effort: Small
    *   Priority: High
*   **Task 5.4:** Perform end-to-end testing based on the User Story/Scenario:
    *   User asks a question via UI.
    *   Verify correct explanation is displayed.
    *   Verify relevant content suggestion is displayed.
    *   Verify clicking suggestion leads to mocked content.
    *   Check console logs of mocked services for interaction data.
    *   Effort: Medium
    *   Priority: High
*   **Task 5.5:** Identify and document any integration issues, bugs, or technical challenges encountered.
    *   Effort: Small
    *   Priority: High

## 6. Documentation (Minimal for PoC)

*   **Task 6.1:** Create a simple `README.md` for the PoC project outlining:
    *   Brief PoC description.
    *   How to set up and run the PoC (including LLM API key setup, starting services).
    *   List of implemented and mocked service endpoints.
    *   How to test the defined user scenario.
    *   Effort: Medium
    *   Priority: High
*   **Task 6.2:** Document any significant technical decisions made, scripts for manual KG population, and findings/challenges from the PoC.
    *   Effort: Small
    *   Priority: Medium

This task breakdown should provide a clear path for implementing the NeuroForge PoC. Priorities and effort estimations are relative and can be adjusted based on team expertise and specific choices made during development.Okay, I have created the `neuroforge_poc_implementation_tasks.md` document. It breaks down the NeuroForge Proof-of-Concept into actionable development tasks, categorized by development area (Environment & Setup, Knowledge Graph Setup, Backend Services Development including KGS, AIMS, and Mocked Services, Frontend UI Development with options for Web or CLI, Integration & Testing, and Minimal Documentation). Each task includes a suggested relative effort and priority.

This structure should provide a clear roadmap for the PoC implementation.
