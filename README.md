# Anthropic.SDK

[![.NET](https://github.com/tghamm/Anthropic.SDK/actions/workflows/dotnet.yml/badge.svg)](https://github.com/tghamm/Anthropic.SDK/actions/workflows/dotnet.yml)

Anthropic.SDK is an unofficial C# client designed for interacting with the Claude AI API. This powerful interface simplifies the integration of the Claude AI into your C# applications.  It targets netstandard2.0, and .net6.0.

## Table of Contents

- [Installation](#installation)
- [API Keys](#api-keys)
- [IHttpClientFactory](#ihttpclientfactory)
- [Usage](#usage)
- [Examples](#examples)
  - [Non-Streaming Call](#non-streaming-call)
  - [Streaming Call](#streaming-call)
- [Contributing](#contributing)
- [License](#license)

## Installation

Install Anthropic.SDK via the [NuGet](https://www.nuget.org/) package manager:

```bash
PM> Install-Package Anthropic.SDK
```

## API Keys

You can load the API Key from an environment variable named `ANTHROPIC_API_KEY` by default. Alternatively, you can supply it as a string to the `AnthropicClient` constructor.

## IHttpClientFactory

The `AnthropicClient` can optionally take an `IHttpClientFactory`, which allows you to control elements such as retries and timeouts.

## Usage

To start using the Claude AI API, simply create an instance of the `AnthropicClient` class.

## Examples

### Non-Streaming Call

Here's an example of a non-streaming call to the Claude AI API:

```csharp
var client = new AnthropicClient();
var prompt = $"\n\nHuman:Write me a sonnet about Joe Biden.\n\nAssistant:";
var parameters = new SamplingParameters()
{
    // required    
    Model = "claude-1.3"
    Prompt = prompt,
    //optional
    MaxTokensToSample = 512,
    Temperature = 1,
    StopSequences = new[] { "\n\nHuman:" },
    Stream = false,
    TopK: 5
    TopP: 0.7
    Metadata = new [] {user_id = "13803d75-b4b5-4c3e-b2a2-6f21399b021b"}
};

var response = await client.Completions.GetClaudeCompletionAsync(parameters);
```

### Streaming Call

The following is an example of a streaming call to the Claude AI API:

```csharp
var client = new AnthropicClient();
var prompt = $"\n\nHuman:Write me a sonnet about Joe Biden.\n\nAssistant:";
var parameters = new SamplingParameters()
{
    MaxTokensToSample = 512,
    Prompt = prompt,
    Temperature = 0.0f,
    StopSequences = new[] { "\n\nHuman:" },
    Stream = true,
    Model = "claude-1.3"
};

await foreach (var res in client.Completions.StreamClaudeCompletionAsync(parameters))
{
    Console.Write(res.Completion);
}
```

## Contributing

Pull requests are welcome. If you're planning to make a major change, please open an issue first to discuss your proposed changes.

## License

This project is licensed under the [MIT](https://choosealicense.com/licenses/mit/) License.
