# List of PromptML Annotations
This document lists all available block annotations in the Prompt Markup Language.

All of these below block annotations will end with `@end` annotation.

| Name  | description | type
|------|-------------|------|
| <i>@prompt</i> | Parent of all other annotations | compound |
| <i>@instructions</i> | A group of instructions to follow | compound |
| <i>@vars</i> | Variable definition block | simple |
| <i>@metadata</i> | Custom properties about task | simple |
| <i>@context</i> | The context of a task | text |
| <i>@objective</i> | The end goal of a task | text |
| <i>@step</i> | A single instruction | text |
| <i>@constraints</i> | Prompt constraints block | text |
| <i>@length</i> | Length constraints | text |
| <i>@tone</i> | Tone constraint | text |
| <i>@difficulty</i> | Task difficulty level | text |
