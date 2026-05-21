# [Project Name] - Inferred Product Spec

## 0. Document Status

- **Basis**: Current codebase inference
- **Overall confidence**: [High / Medium / Low]
- **Primary evidence**:
  - `[path/to/file]` - [why it matters]
  - `[path/to/file]` - [why it matters]

---

## 1. Product Summary

### 1.1 Current-State Positioning
- **Observed**: [What the repository clearly shows the product does]
- **Inferred**: [Best-supported plain-language summary, if needed]

### 1.2 Primary Users / Actors
- **Observed**: [Roles or actors clearly named in code or copy]
- **Inferred**: [Likely primary user if not named directly]

### 1.3 Core Job To Be Done
- [What the user is trying to accomplish in this product]

---

## 2. Surface Map

### 2.1 Entry Points
- **Surface**: [Page / route / command / endpoint]
  - **Purpose**: [What this surface is for]
  - **Evidence**: `[path/to/file]`

### 2.2 Navigation / Information Architecture
- [Primary sections, menus, tabs, or workflow stages]

---

## 3. Feature Requirements

### Feature 1: [Feature Name]
- **Observed behavior**: [What the product clearly does]
- **User value**: [Why this matters in product terms]
- **Inputs**: [User input, uploaded data, trigger]
- **Outputs**: [System result, saved record, generated artifact]
- **Evidence**:
  - `[path/to/file]`
  - `[path/to/file]`
- **Confidence**: [High / Medium / Low]

### Feature 2: [Feature Name]
- [Repeat]

[Continue for major shipped features]

---

## 4. User Flows

### 4.1 Primary Flow: [Flow Name]

1. **Entry**
   - User action: [What starts the flow]
   - System behavior: [What becomes visible or available]
   - Evidence: `[path/to/file]`
2. **Action**
   - User action: [Next step]
   - System behavior: [Response]
   - Evidence: `[path/to/file]`
3. **Completion**
   - User outcome: [What counts as success]
   - Stored / returned result: [Artifact, record, or state]
   - Evidence: `[path/to/file]`

### 4.2 Edge Cases / Constraints
- [Observed edge case or state handling]

---

## 5. Entities and Integrations

### 5.1 Core Entities
- **Entity**: [Name]
  - **Role in product**: [Why it exists]
  - **Evidence**: `[path/to/file]`

### 5.2 External Integrations
- **Integration**: [Service / API / platform]
  - **Product purpose**: [Why the product uses it]
  - **Evidence**: `[path/to/file]`

---

## 6. AI / Automation Signals

Include this section only when evidence supports it.

- **Capability**: [e.g. Text generation]
  - **Observed evidence**: `[path/to/file]`
  - **Product use**: [Where it appears in the product]
  - **Confidence**: [High / Medium / Low]

If the repo only contains dormant AI plumbing, move it to `Open Questions` or technical notes instead.

---

## 7. Open Questions

- [What the code cannot reliably tell us about audience, prioritization, intent, or dead code]

---

## 8. Summary for Handoff

- **Definitely shipped**: [Shortest reliable summary]
- **Probably true but inferred**: [Important caveated conclusions]
- **Needs human confirmation**: [Questions that code alone cannot settle]
