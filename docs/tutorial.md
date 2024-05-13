# PromptML tutorial

Let's take a regular prompt:. It looks like this:

> I want you to act as an interviewer. I will be the candidate and you will ask me the interview questions for the position position. I want you to only reply as the interviewer. Do not write all the conservation at once. I want you to only do the interview with me. Ask me the questions and wait for my answers. Do not write explanations. Ask me the questions one by one like an interviewer does and wait for my answers. My first sentence is "Hi".


Note: This prompt is taken from Awesome ChatGPT prompts [Awesome Chat GPT prompts](https://github.com/f/awesome-chatgpt-prompts?tab=readme-ov-file#act-as-position-interviewer)

Do you see any issue with the above prompt ? 

We see few:

1. The prompt doesn't have a defined structure
2. Prompt is not explicit about it's motivation
3. Prompt is hard to review as it is a natural language (English in this case)

Now. Let's write the same prompt in PromptML. Every prompt begins and end with `@prompt` and `@end` annotations respectively.

```promptml
@prompt

@end
```

Now, inside the `@prompt`, there are two main annotations called `@context` and `@objective` to separate background of task with objective of task. Sounds simple but very important for structuring the prompt.

```promptml
@prompt
    @context
        You are an experienced interviewer
    @end

    @objective
        Ask me the questions one by one like an interviewer does and wait for my answers. My first sentence is "Hi"
    @end
@end
```

Now, we have the core prompt that can do the job as the original prompt. But let's add instructions as in the original prompt. You use `@instructions` annotation with `@step` annotations.

```promptml
@prompt
    @context
        You are an experienced interviewer
    @end

    @objective
        Ask me the questions one by one like an interviewer does and wait for my answers.
        My first sentence is "Hi"
    @end

    @instructions 
        @step
            Only reply as an interviewer
        @end

        @step
            Ask your questions one by one
        @end

        @step
            Only acts as an interviewer and nothing more
        @end
    @end
@end
```
This PromptML prompt is called "Source Prompt". This prompt hsa a clear structure about different sections for both humans and machine. How do we use this prompt ? 

## Using PromptML parser

PromptML parser is a tool that can parse PromptML code and generates a prompt object in form of Python dictionary.

```sh
pip install promptml
# launch shell
python3
```


```py
> from promptml.parser import PromptParser
> prompt = """
    @prompt
        @context
            You are an experienced interviewer.
        @end

        @objective
            Ask me the questions one by one like an interviewer does and wait for my answers. My first sentence is "Hi".
        @end

        @instructions 
            @step
                Only reply as an interviewer.
            @end

            @step
                Ask your questions one by one.
            @end

            @step
                Only acts as an interviewer and nothing more.
            @end
        @end
    @end
"""
> p1 = PromptParser(prompt)
> parsed_prompt = p1.parse()

{
    'context': 'You are an experienced interviewer.',
    'objective': 'Ask me the questions one by one like an interviewer does and wait for my answers. My first sentence is "Hi"',
    'instructions': [
        'Only reply as an interviewer',
        'Ask your questions one by one',
        'Only acts as an interviewer and nothing more'
    ]
}
```

As you can see, the PromptML code is cleanly parsed into a Python dictionary: `parsed_prompt` that can be used to generate structured natural language query. For example, if we have a Jinja2 template like this:

```jinja2
{{ parsed_prompt.context }}

Make sure to follow the below instructions:
{% for instr in parsed_prompt.instructions%}
    - {{ instr }}
{% endfor %}

{{ parsed_prompt.objective }}
```

The rendered output of above Jinja2 template with `parsed_prompt` will be:

```txt
You are an experienced interviewer.

Make sure to follow the below instructions:
- Only reply as an interviewer
- Ask your questions one by one
- Only acts as an interviewer and nothing more

Ask me the questions one by one like an interviewer does and wait for my answers. My first sentence is "Hi"
```

This natural language prompt is called "Derived Prompt". A source prompt is always the single source of truth for multiple derived prompts and easy to review.

Ooh! you've now finished this 2-min tutorial about PromptML. See the examples in the next section.