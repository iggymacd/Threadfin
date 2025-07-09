# AGENTS.md - Guidelines for AI Agents Working on Threadfin

Welcome, fellow agent! This document provides guidance for contributing to the Threadfin project, especially as it undergoes refactoring and enhancements to become enterprise-ready. Your adherence to these guidelines will ensure consistency, quality, and maintainability.

## 1. Core Principle: Documentation is Paramount

As Threadfin evolves, particularly towards enterprise readiness, **up-to-date and accurate documentation is as critical as the code itself.**

*   **ALWAYS Update Documentation:** If your code changes affect:
    *   User-facing behavior (UI, CLI, API responses, observable functionality).
    *   System configuration (new settings, changes to existing ones, environment variables).
    *   Deployment procedures (Docker, Kubernetes, binary).
    *   Security considerations.
    *   Build processes.
    *   Architecture or internal logic that a developer would need to understand.
    *   **You MUST update the relevant sections in the `/docs` directory.**
*   **Be Specific:** Don't just mention a change; explain *how* it affects the user or system. Provide examples where appropriate.
*   **Screenshots and Diagrams:** For UI changes, placeholder text for screenshots is acceptable initially, but flag it for human review to add actual visuals. For architectural changes, consider if a diagram needs updating or creating.
*   **Changelog:** For any user-facing change or significant bug fix, add an entry to `/docs/changelog.md` under the appropriate "Unreleased" section.
*   **Consistency:** Maintain consistency with existing documentation style and terminology. Refer to the `/docs/glossary.md`.
*   **If Unsure, Ask or Flag:** If you are unsure which documentation sections are impacted or how to best document a change, clearly state this in your commit message or PR description and flag it for human review.

**Failure to keep documentation synchronized with code changes will significantly hinder the project's goals.**

## 2. Golang Development Best Practices

*   **Effective Go:** Adhere to the principles outlined in [Effective Go](https://golang.org/doc/effective_go.html).
*   **Formatting:** All Go code MUST be formatted with `gofmt` or `goimports` before committing.
*   **Error Handling:**
    *   Handle errors explicitly. Do not ignore them (e.g., `_ = someFunc()`).
    *   Provide context to errors when returning them up the call stack (e.g., `fmt.Errorf("failed to do X: %w", err)`).
    *   Use `errors.Is()` and `errors.As()` for checking error types or wrapped errors.
*   **Concurrency:**
    *   Use goroutines and channels appropriately for concurrent operations.
    *   Be mindful of race conditions. Use the `-race` flag during testing (`go test -race ./...`).
    *   Ensure proper synchronization mechanisms (mutexes, waitgroups) are used when accessing shared resources.
*   **Testing:**
    *   Write unit tests for new functionality and bug fixes. Place tests in `_test.go` files.
    *   Aim for good test coverage.
    *   Use the standard `testing` package. Consider table-driven tests for multiple scenarios.
*   **Modularity:** Keep packages focused and well-defined. Avoid circular dependencies.
*   **Naming Conventions:** Follow Go's naming conventions (e.g., `camelCase` for local variables, `PascalCase` for exported identifiers).
*   **Comments:**
    *   Write clear, concise comments for exported functions, types, and complex logic.
    *   Use `// TODO:` or `// FIXME:` for known issues or areas needing future attention.
*   **Dependencies:**
    *   Manage dependencies using Go Modules (`go.mod`, `go.sum`).
    *   Keep dependencies updated, but test thoroughly after updates.
    *   Vendor dependencies using `go mod vendor` for reproducible builds.

## 3. Enterprise Readiness Focus

When refactoring or adding features, consider the following enterprise aspects:

*   **Configuration:**
    *   Prefer environment variables for configuring sensitive data or settings that vary by environment (development, staging, production).
    *   Ensure configuration loading is robust and provides clear error messages for misconfigurations.
*   **Security:**
    *   Follow secure coding practices (OWASP Top 10 principles where applicable).
    *   Input validation and output encoding are crucial.
    *   Be mindful of potential vulnerabilities (e.g., path traversal, SSRF if handling external URLs).
    *   Refer to `/docs/enterprise-guide/security.md` for existing security guidelines.
*   **Observability (Logging & Monitoring):**
    *   Implement structured logging (e.g., JSON format) with clear log levels.
    *   Provide meaningful metrics that can be exposed for monitoring systems (e.g., Prometheus). Think about what an operator would need to monitor.
    *   Refer to `/docs/enterprise-guide/monitoring-logging.md`.
*   **Performance & Scalability:**
    *   Write efficient code. Be mindful of resource usage (CPU, memory, network).
    *   Consider how features will perform under load.
*   **Reliability & Resilience:**
    *   Implement proper error handling and recovery mechanisms.
    *   Ensure the application can handle transient errors gracefully.
*   **API Design:**
    *   If modifying or adding API endpoints, aim for consistency, clear request/response formats (JSON), and proper HTTP status codes.
    *   Update `/docs/api-reference/index.md` and any detailed API endpoint documentation.

## 4. Frontend (TypeScript/JavaScript) Development

*   **Consistency:** Follow existing code style and patterns in the `ts/` directory.
*   **Compilation:** Always recompile TypeScript to JavaScript (`tsc -p ./ts/tsconfig.json`) after making changes.
*   **Asset Embedding:** Remember to run `./threadfin -dev` and rebuild the Go binary if your TypeScript changes need to be embedded into the main application (as per current build process).
*   **User Experience:** Changes to the UI should be intuitive and user-friendly.

## 5. Git and Contribution Workflow

*   Follow the guidelines in `/docs/developer-guide/contribution-guidelines.md`.
*   Create descriptive branch names.
*   Write clear, concise commit messages. Reference related issues.
*   Keep pull requests focused on a single feature or bug fix.
*   Ensure your branch is rebased on the latest `main` (or development branch) before submitting a PR.

## 6. Communication

*   If a task is ambiguous or you need clarification on requirements, **ask for human input** rather than making assumptions that could lead to incorrect implementations.
*   Provide clear updates on your progress and any challenges encountered.

## 7. Testing Your Changes

*   **Unit Tests:** Run `go test ./...` (and `go test -race ./...`).
*   **Build Verification:** Ensure the project builds successfully after your changes: `go build threadfin.go`.
*   **Manual/Integration Testing:**
    *   Run the Threadfin instance locally.
    *   Manually test the functionality you've changed.
    *   Consider common use cases and edge cases.
    *   If UI changes were made, test across different browsers (or at least the primary supported ones).
    *   Test with actual M3U/XMLTV sources if your changes impact playlist or EPG processing.

By following these guidelines, you will contribute effectively to making Threadfin a robust, enterprise-ready application. Thank you for your assistance!
