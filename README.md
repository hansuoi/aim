# AIM

## ABOUT
- AIM (Action Impact Model) is a modeling notation based on class diagrams, designed to visualize the impact scope of user actions and define test "targets".
- It is designed for documentation, impact analysis, and AI-assisted test design and implementation.

![AIM Sample](./samples/aim-sample.png)

---

## How to Describe AIM
- Each UI package represents a "Page", "Modal", "Component", etc., and contains the following classes:
    - UI elements: Interactive or visual elements contained in that package (e.g. buttons, text boxes)
    - User actions: Actions that users perform on that screen
- When a user action affects a UI element, the relationship is represented by an arrow (`-->`).
    - Conditions and details of the impact are expressed using labels
    - Labels may be omitted when the impact is self-evident (e.g., executing "Change user ID" changes the display content of "User ID")
- Each package and each class has a relationship that can be mutually converted with UCOD
    - Arrows cannot be converted
- [Design guide](./docs/aim-design-guide.en.md)

---

## Use Cases for AIM
- Impact scope analysis:
    - Organize which UI elements are affected by which user actions
    - Also useful for identifying improvement points in the system, such as whether too many UI elements are affected by a specific action, or whether a specific UI element is affected by too many actions
    - [Sample prompt](./prompts/analyze-impact.md)
- AI-assisted test case design:
    - By providing AIM to generative AI and covering the impact scope described in AIM, you can design a test suite
    - [Sample prompt](./prompts/generate-testcases-by-aim.md)
- AI-assisted automated test implementation:
    - By providing generative AI with AIM, increment information (product code differences, task tickets, etc.), and [UCOD](https://github.com/hansuoi/ucod) (a sister model of AIM), you can automatically generate automated tests using POM (Page Object Model) that target incremental parts of the product

---

## Directory Structure
```
.
├─ README.md                 # this file
├─ docs/
│  ├─ aim-design-guide.en.md
│  └─ aim-design-guide.ja.md
├─ prompts/
│  ├─ generate-testcases-by-aim.md
│  └─ analyze-impact.md
└─ samples/
   ├─ aim-sample.puml
   └─ ucod-sample-paired-with-aim.puml
```
