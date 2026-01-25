# AIM Design Guide

## Overview
- This document describes the syntax, design principles, and usage guidelines for AIM (Action Impact Model) written in PlantUML.

---

## 1. Packages
### Package Overview
- Each package encompasses the following:
    - UI elements
    - User actions

### Stereotypes
- `<<Page>>`, `<<Modal>>`, `<<Component>>`, etc.
    - Additional stereotypes can be added or integrated as needed

---

## 2. Classes
### User Actions
- Describes what users can do on the screen or use cases
- Use `<<Action>>` stereotype

### UI Elements
- UI element stereotypes include, for example, the following, but can be added or integrated as needed:
    - `<<Component>>`, `<<Button>>`, `<<TextBox>>`, `<<PullDown>>`, `<<Img>>`, `<<Text>>`, etc.

```plantuml
package "Settings Page" <<Page>> {
  class "Change user ID" <<Button>>          ' UI element
  class "Update user ID" <<Action>>          ' Action
  class "Header" <<Component>>               ' Component
}
```

---

## 3. Arrows
- `"Action" --> "UI Element" : {impact}`
    - Indicates that executing `"Action"` affects `"UI Element"` with `{impact}` (modify `{modify}`, delete `{delete}`, add `{add}`, etc.)
    - The label part indicating impact (`{impact}`) may be omitted when self-evident (e.g., when executing "Change user ID" changes the display content of "User ID")
- `"Action" --> "UI Element" : {impact} {Child Element}`
    - When only child elements of a UI element (such as Overlay content) are affected, describe the child element name in the label
- `"Action" --> "UI Element" : if {condition} {impact} {Child Element}`
    - Indicates an impact under a certain condition `{condition}`

---

## 4. Mapping with UCOD
- Each package and class in AIM can be mutually converted with the sister model [UCOD](https://github.com/hansuoi/ucod)

| UCOD | AIM |
| ---- | --- |
| Page class | Page package |
| Modal class | Modal package |
| Component class | Component package |
| Overlay class | Arrow label |
| UI elements in class | UI element class |
| Actions in class | Action class |
| Transition arrows | (No correspondence) |
| (No correspondence) | Impact arrows |

---

## 5. AIM as a Design Model for Automated Test Code Based on Product Change Impact Scope
- AIM is useful for identifying the impact scope of product changes in agile development, incremental development, etc.
- It is particularly intended for use cases where generative AI agents are given AIM and product code differences to automatically identify the impact scope, automatically design test cases, and automatically implement automated test code.
    - In such cases, it is strongly recommended to also use [UCOD](https://github.com/hansuoi/ucod), a sister model of AIM, as input.

### Implementation Example
- Assumes POM (Page Object Model) implementation in Playwright with TypeScript

#### Page Object Implementation
- Can be implemented based on UCOD
    - Reference:
        - [UCOD Design Guide](https://github.com/hansuoi/ucod/blob/main/docs/ucod-design-guide.en.md)
        - [Sample prompt](https://github.com/hansuoi/ucod/blob/main/prompts/generate-pom.md)

#### Assertion File Implementation
- Assertions can be implemented based on the labels attached to AIM arrows

#### Test Code Implementation
- The arrow path from an action to a UI element itself becomes a test case
    - AIM arrows represent "(under certain conditions) a user action affects a UI element"
