# TodoLister Development Tasklist

This tasklist is derived from the enhanced TodoLister PRD, structured for developers and LLMs to implement the application incrementally. It adheres to SOLID principles by ensuring modules have single responsibilities, are extensible without modification, use abstractions for dependencies, and promote segregated interfaces. The design is modular (e.g., separate modules for data, UI, events), with each PR introducing non-breaking changes (e.g., building on previous structures without altering existing code). Tasks are broken into epics (high-level groupings), PRs (mergeable feature branches), commits (atomic changes), and subtasks (detailed steps). Each item is tickable using markdown checkboxes for tracking progress.

Epic 0 focuses on preparatory documentation and setup to establish a solid foundation before coding begins.

## Epic 0: Preparation and Documentation
This epic handles all upfront documentation, repository setup, and planning to ensure the project is well-documented and ready for modular development without breaking changes.

### PR 0.1: Initialize Repository and Core Documentation
- [ ] **Branch**: setup/init
- [ ] **Commit 0.1.1**: Create repository structure.
  - [ ] Subtask: Initialize Git repository.
  - [ ] Subtask: Add .gitignore for common web files (e.g., node_modules, though no Node for MVP).
  - [ ] Subtask: Create empty folders: src/js (for modules), src/css, docs.
- [ ] **Commit 0.1.2**: Add README.md with project overview.
  - [ ] Subtask: Document project goals, tech stack (HTML/CSS/JS), and SOLID adherence.
  - [ ] Subtask: Include setup instructions (e.g., "Open index.html in browser").
  - [ ] Subtask: Outline modular architecture (e.g., AppController, DataModule).
- [ ] **Commit 0.1.3**: Add CONTRIBUTING.md for development guidelines.
  - [ ] Subtask: Explain SOLID principles application (e.g., "Each module must follow SRP").
  - [ ] Subtask: Describe PR process: Feature branches, no breaking changes, atomic commits.
  - [ ] Subtask: Note audience: Tasks suitable for human devs or LLMs (e.g., clear, stepwise subtasks).

### PR 0.2: Document Architecture and User Flows
- [ ] **Branch**: docs/architecture
- [ ] **Commit 0.2.1**: Create ARCHITECTURE.md.
  - [ ] Subtask: Describe modules: DataModule (SRP: data handling), UIModule (SRP: rendering), etc.
  - [ ] Subtask: Explain dependency inversion (e.g., AppController depends on interfaces).
  - [ ] Subtask: List assumptions (e.g., browser compatibility, no deps).
- [ ] **Commit 0.2.2**: Add user flow diagram in docs/user-flow.md.
  - [ ] Subtask: Embed or describe the Mermaid diagram from PRD.
  - [ ] Subtask: Add error paths (e.g., validation failures).
- [ ] **Commit 0.2.3**: Document success metrics and constraints.
  - [ ] Subtask: List metrics (e.g., handle 100 items).
  - [ ] Subtask: Note constraints (e.g., 1-24 hour build, modular for extensibility).

## Epic 1: Core Data Management
This epic implements data handling modularly, focusing on persistence and CRUD operations per SOLID (e.g., DataModule closed for modification but open for extension via adapters).

### PR 1.1: Implement DataModule Skeleton
- [ ] **Branch**: feature/data-skeleton
- [ ] **Commit 1.1.1**: Create data.js file.
  - [ ] Subtask: Define DataModule class with constructor initializing empty array.
  - [ ] Subtask: Add load() method stub using LocalStorage abstraction.
- [ ] **Commit 1.1.2**: Implement save() method.
  - [ ] Subtask: Stringify array and store in LocalStorage.
  - [ ] Subtask: Ensure no side effects (SRP: only persistence).

### PR 1.2: Add CRUD Operations to DataModule
- [ ] **Branch**: feature/data-crud
- [ ] **Commit 1.2.1**: Implement addItem(text).
  - [ ] Subtask: Validate (non-empty, <100 items).
  - [ ] Subtask: Push {text, complete: false} and call save().
- [ ] **Commit 1.2.2**: Implement toggleComplete(index).
  - [ ] Subtask: Flip complete flag if index valid.
  - [ ] Subtask: Call save() post-change.
- [ ] **Commit 1.2.3**: Implement deleteItem(index).
  - [ ] Subtask: Splice array if index valid.
  - [ ] Subtask: Call save() post-change.
- [ ] **Commit 1.2.4**: Add getItems() for abstraction.
  - [ ] Subtask: Return copy of array (immutable interface).

## Epic 2: User Interface Rendering
This epic builds the UI module separately, ensuring it's dependent on data abstractions without tight coupling.

### PR 2.1: Setup Basic HTML and CSS
- [ ] **Branch**: feature/ui-setup
- [ ] **Commit 2.1.1**: Create index.html.
  - [ ] Subtask: Add basic structure: <h1>, input, add button, <ul id="todoList">.
  - [ ] Subtask: Link styles.css and app.js (deferred loading).
- [ ] **Commit 2.1.2**: Create styles.css.
  - [ ] Subtask: Define base styles (e.g., flex for layout).
  - [ ] Subtask: Add .complete { text-decoration: line-through; }.

### PR 2.2: Implement UIModule
- [ ] **Branch**: feature/ui-module
- [ ] **Commit 2.2.1**: Create ui.js.
  - [ ] Subtask: Define UIModule class with renderList(items).
  - [ ] Subtask: Clear <ul> and loop to create <li> elements.
- [ ] **Commit 2.2.2**: Add item rendering details.
  - [ ] Subtask: Include checkbox (checked if complete), span for text, delete button.
  - [ ] Subtask: Apply classes for styling.

## Epic 3: Event Handling and Integration
This epic wires events modularly, using delegation for efficiency and ensuring no direct DOM manipulation outside UIModule.

### PR 3.1: Implement EventModule
- [ ] **Branch**: feature/event-module
- [ ] **Commit 3.1.1**: Create events.js.
  - [ ] Subtask: Define EventModule class with attachListeners(appController).
  - [ ] Subtask: Add click handler for add button.
- [ ] **Commit 3.1.2**: Implement delegation for list events.
  - [ ] Subtask: On <ul> click: If delete button, get index and call controller delete.
  - [ ] Subtask: On <ul> change: If checkbox, get index and call controller toggle.

### PR 3.2: Implement AppController and Integration
- [ ] **Branch**: feature/app-controller
- [ ] **Commit 3.2.1**: Create app.js.
  - [ ] Subtask: Define AppController class.
  - [ ] Subtask: Initialize DataModule, UIModule, EventModule.
- [ ] **Commit 3.2.2**: Add init() method.
  - [ ] Subtask: Load data, render initial list, attach events.
- [ ] **Commit 3.2.3**: Implement action methods (add, toggle, delete).
  - [ ] Subtask: Call data methods, then ui.renderList.
  - [ ] Subtask: Handle validations with alerts.

## Epic 4: Validation, Polish, and Testing
This epic adds a ValidationModule for segregated concerns and ensures the app is testable without breaking prior work.

### PR 4.1: Implement ValidationModule
- [ ] **Branch**: feature/validation
- [ ] **Commit 4.1.1**: Create validation.js.
  - [ ] Subtask: Define ValidationModule with methods like isValidAdd(text, length).
  - [ ] Subtask: Return errors or true.
- [ ] **Commit 4.1.2**: Integrate into DataModule.
  - [ ] Subtask: Inject ValidationModule as dependency (DIP).
  - [ ] Subtask: Use in addItem for checks.

### PR 4.2: Final Polish and Manual Testing
- [ ] **Branch**: feature/polish
- [ ] **Commit 4.2.1**: Add error handling UI.
  - [ ] Subtask: Show alerts for validation failures.
- [ ] **Commit 4.2.2**: Document testing in TEST.md.
  - [ ] Subtask: List manual tests (e.g., add 101 items fails).
  - [ ] Subtask: Suggest console-based unit tests for modules.
- [ ] **Commit 4.2.3**: Ensure responsiveness.
  - [ ] Subtask: Add media queries in CSS for mobile.
