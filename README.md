You are an AI agent assisting in building a fully automated DevOps + AI-based code-fixing pipeline.

The system is designed using:
- FastMCP server
- MCP-use client
- Python for all orchestration
- SonarQube for static analysis
- GitHub for source control
- Gemini LLM for intelligent code fixes

Your task is to help complete the implementation of the following flow:

1. When code is pushed and merged to `main`, trigger a GitHub Action pipeline.
2. If the pipeline is successful:
   - Trigger a SonarQube scan.
   - Fetch the list of violations using SonarQube API `/api/issues/search`.
   - Extract important details like:
     - `component`
     - `line`
     - `message`
     - `type`
3. Use the `mcp-use` Gemini-powered LLM to fix these issues in the code:
   - Clone the GitHub repo to a temp folder.
   - Pass relevant code snippets and SonarQube messages to the LLM.
   - The LLM should return an updated/fixed version of the code.

4. Apply additional formatting using:
   - `black`
   - `isort`

5. Commit the changes to a new branch:
   - Branch naming convention: `sonar_resolution_<build_no>`
   - Use `git` CLI from Python subprocess

6. Raise a Pull Request via the GitHub API using PyGitHub.

7. Monitor PR comments:
   - If the comment includes the keyword `update`, re-check SonarQube, re-run the fix pipeline, and update the PR.
   - Otherwise, respond appropriately in the PR conversation thread.

Important Notes:
- The FastMCP server defines `@mcp.tool()` functions to handle modular steps.
- The MCP client launches and coordinates tools, and sends prompts to the LLM agent.
- The GeminiLLMWrapper class wraps the Google Generative AI (Gemini 2.5) model and converts responses into LangChain messages.

Your job is to:
- Implement any missing logic.
- Ensure proper async handling (e.g., awaiting Gemini calls).
- Monitor and respond to PR comments continuously or periodically (e.g., via GitHub webhooks or polling).
- Refactor and modularize code if needed for readability.
- Validate environment variables and provide useful errors if anything is missing.

You should:
- Write idiomatic Python 3.11+ code.
- Use asyncio and subprocess efficiently.
- Include logging where useful.
- Avoid redundant API calls.
- Ensure minimal memory/cpu use in long-running loops.

Extra task:
- Optionally, integrate a dashboard or simple CLI for visualizing violations and PR statuses (bonus points 😎).

Build the ultimate DevOps + AI code-fixer 💪🔥
