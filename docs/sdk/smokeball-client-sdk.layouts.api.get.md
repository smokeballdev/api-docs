<!-- Do not edit this file. It is automatically generated by API Documenter. -->

[Home](./index.md) &gt; [smokeball-client-sdk](./smokeball-client-sdk.md) &gt; [layouts](./smokeball-client-sdk.layouts.md) &gt; [Api](./smokeball-client-sdk.layouts.api.md) &gt; [get](./smokeball-client-sdk.layouts.api.get.md)

## layouts.Api.get() method

Gets the layout associated to the current context or the specified matter id if provided.

<b>Signature:</b>

```typescript
get(matterId?: string): Promise<LayoutMatter>;
```

## Parameters

|  Parameter | Type | Description |
|  --- | --- | --- |
|  matterId | string | <i>(Optional)</i> the layout to retrieve, use null for the matter in the current context. |

<b>Returns:</b>

Promise&lt;[LayoutMatter](./smokeball-client-sdk.layouts.layoutmatter.md)<!-- -->&gt;

the specified layout.

## Example


```
// Returns the layout with the specified matter id.
const matter = await sdk.layouts.get('e9b9084b-c9b4-4f3c-9f5a-4c83ed3ac265');
```
