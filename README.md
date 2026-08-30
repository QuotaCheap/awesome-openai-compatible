# Awesome OpenAI-Compatible

A curated list of OpenAI-compatible APIs, gateways, tools, SDK patterns, test harnesses, and production notes.

OpenAI-compatible APIs are becoming the common interface for model access. The hard part is not the interface. The hard part is choosing, testing, routing, observing, and controlling real production traffic.

## Contents

- [Gateways and API Layers](#gateways-and-api-layers)
- [Testing and Benchmarking](#testing-and-benchmarking)
- [Cost and Budget Control](#cost-and-budget-control)
- [SDK Examples](#sdk-examples)
- [Local and Self-Hosted Servers](#local-and-self-hosted-servers)
- [Agent Workflows](#agent-workflows)
- [Production Checklist](#production-checklist)
- [Contributing](#contributing)

## Gateways and API Layers

- [QuotaCheap](https://www.quota.cheap) - OpenAI-compatible API gateway with server-side upstream credentials, user API keys, quotas, logs, usage tracking, balances, and billing visibility.
- [LiteLLM](https://github.com/BerriAI/litellm) - Proxy and SDK for calling many LLM providers with an OpenAI-compatible interface.
- [OpenRouter](https://openrouter.ai) - Unified API for accessing multiple models through an OpenAI-compatible style interface.
- [Portkey](https://github.com/Portkey-AI/gateway) - AI gateway focused on routing, observability, and reliability.
- [XiuRouter](https://router.xiu.ai/) - Hosted multi-model API supporting OpenAI Responses and Chat Completions, Anthropic Messages, and Gemini GenerateContent, with scoped API keys, usage-based pricing, and request-level usage and cost records.

## Testing and Benchmarking

- [llm-gateway-benchmark](https://github.com/quotacheap/llm-gateway-benchmark) - Benchmark OpenAI-compatible gateways for latency, success rate, and repeatable scenarios.
- [openai-compatible-healthcheck](https://github.com/quotacheap/openai-compatible-healthcheck) - Proposed healthcheck pattern for endpoint compatibility, auth, chat completions, streaming, and error formats.

## Cost and Budget Control

- [ai-agent-budget-guard](https://github.com/quotacheap/ai-agent-budget-guard) - CLI guardrail that stops runaway AI agents before they burn a configured budget.
- Track prompt tokens, completion tokens, cache reads, request count, latency, and retries separately.
- Use per-user and per-task budgets for agentic workflows, not just one global monthly cap.

## SDK Examples

### Node.js

```js
import OpenAI from 'openai';

const client = new OpenAI({
  apiKey: process.env.QUOTACHEAP_API_KEY,
  baseURL: 'https://api.quota.cheap/v1',
});

const response = await client.chat.completions.create({
  model: 'gpt-5.4-mini',
  messages: [{ role: 'user', content: 'Hello from an OpenAI-compatible API.' }],
});

console.log(response.choices[0].message.content);
```

### Python

```python
from openai import OpenAI

client = OpenAI(
    api_key=os.environ['QUOTACHEAP_API_KEY'],
    base_url='https://api.quota.cheap/v1',
)

response = client.chat.completions.create(
    model='gpt-5.4-mini',
    messages=[{'role': 'user', 'content': 'Hello from an OpenAI-compatible API.'}],
)

print(response.choices[0].message.content)
```

## Local and Self-Hosted Servers

- [Ollama](https://github.com/ollama/ollama) - Local model runner with OpenAI-compatible endpoints for some workflows.
- [LocalAI](https://github.com/mudler/LocalAI) - Self-hosted OpenAI-compatible API for local models.
- [vLLM](https://github.com/vllm-project/vllm) - High-throughput inference server with OpenAI-compatible server support.

## Agent Workflows

OpenAI-compatible APIs are especially useful for agents because SDKs and tools can switch endpoints without rewriting the whole integration. Production agent workflows still need:

- budget guards
- request logs
- token usage tracking
- concurrency limits
- retry policies
- model routing rules
- human-review gates for risky actions

## Production Checklist

Before sending real traffic through any OpenAI-compatible endpoint:

- Confirm auth errors are clear.
- Confirm chat completions work for your SDK.
- Confirm streaming behavior if you use streaming.
- Measure p50 and p95 latency with your prompts.
- Check token usage fields in responses.
- Define request, user, project, and daily budget limits.
- Keep API keys out of client-side code.
- Log request IDs and failures.
- Avoid claiming exact model limits unless verified from official docs or API responses.

## Contributing

Pull requests are welcome for tools, docs, examples, and production lessons.

Rules:

- no affiliate spam
- no unverifiable pricing claims
- no fake benchmarks
- no secrets or private payloads
- explain why a resource is useful

## License

CC0-1.0
