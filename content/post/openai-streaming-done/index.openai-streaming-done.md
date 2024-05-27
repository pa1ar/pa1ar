---
title: How to Understand that OpenAI API Streaming Response is Done
description: If you need to process response from OpenAI API in streaming mode, most likely also you need to know when the stream has finished sending all the chunks. Here are two methods to determine when the streaming has completed.
date: 2024-05-15
image: cover_article_openai_finished.jpg
categories:
  - noise
tags:
  - dev
  - AI
---

## summary
Check if
- `content` is `None`
- `finish_reason` is `"stop"`  

This way you can determine when an OpenAI API response has finished streaming. You can use it to set up further processing (e.g. saving full response, displaying to the user, etc.).

## method 1 :: check if `content` is `None`
In each chunk, the content field will be `!None` except for the last chunk. By checking when the content chunk became `None`, you can identify the end of the stream.

```
from openai import OpenAI

client = OpenAI(api_key='your-api-key-here')

stream = client.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[{"role": "user", "content": "count to ten"}],
    stream=True,
)

for chunk in stream:
    if chunk.choices[0].delta.content is not None:
        print(chunk.choices[0].delta.content, end="")
    else:
        print("\ncontent in the last chunk is:", 
        chunk.choices[0].delta.content)
        print("stream finished")
```

Outputs:

```
1, 2, 3, 4, 5, 6, 7, 8, 9, 10.
Content in the last chunk is:  None
Stream finished
```

## method 2 :: check if `finish_reason` is `stop`
In the last chunk, the `finish_reason` value will be set to `"stop"`. This can be used as a signal that the stream has ended.

```
from openai import OpenAI

client = OpenAI(api_key='your-api-key-here')

stream = client.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[{"role": "user", "content": "count to ten"}],
    stream=True,
)

for chunk in stream:
    if chunk.choices[0].finish_reason is None:
        print(chunk.choices[0].delta.content, end="")
    else:
        print("\nfinish_reason in the last chunk is:", 
        chunk.choices[0].finish_reason)
        print("stream finished")
```

Outputs:

```
1, 2, 3, 4, 5, 6, 7, 8, 9, 10.
finish_reason in the last chunk is:  stop
Stream finished
```

## Context
> In my [tldr project](/p/sumr/) I am streaming the response to the user, but I also want to store it. So I needed a way to understand when the last chunk has arrived so that I can process the response further. I found two methods to determine when the streaming has completed and wanted to share them.

Feel free to share your thoughts/methods here in the comments or under [the post on 𝕏](https://x.com/pa1ar/status/1790749565584322590).