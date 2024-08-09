[LLM nodes](https://microsoft.github.io/promptflow/how-to-guides/develop-a-dag-flow/develop-standard-flow.html) are defined in an [JINJA2 template file](https://jinja.palletsprojects.com/en/3.1.x/).

## Declare template

```txt
SYSTEM:
Your task is to classify a given url into one of the following types:
Movie, App, Academic, Channel, Profile, PDF or None based on the text content information.
The classification will be based on the url, the webpage text content summary, or both.

Here are a few examples:
{% for ex in examples %}

URL: {{ex.url}}
Text content: {{ex.text_content}}

OUTPUT:
{"category": "{{ex.category}}", "evidence": "{{ex.evidence}}"}

{% endfor %}

For a given URL : {{url}}, and text content: {{text_content}}.
Classify above url to complete the category and indicate evidence.

OUTPUT:
```
