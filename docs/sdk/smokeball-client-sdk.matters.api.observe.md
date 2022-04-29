<!-- Do not edit this file. It is automatically generated by API Documenter. -->

[Home](./index.md) &gt; [smokeball-client-sdk](./smokeball-client-sdk.md) &gt; [matters](./smokeball-client-sdk.matters.md) &gt; [Api](./smokeball-client-sdk.matters.api.md) &gt; [observe](./smokeball-client-sdk.matters.api.observe.md)

## matters.Api.observe() method

Creates a subscription for the matter associated to the current context or the specified matter id if provided.

Only one subscription will be made per session. Regardless of how many times this function is called, the last registered callback will be used.

<b>Signature:</b>

```typescript
observe(callback: (matter: Matter) => void, matterId?: string): void;
```

## Parameters

|  Parameter | Type | Description |
|  --- | --- | --- |
|  callback | (matter: [Matter](./smokeball-client-sdk.matters.matter.md)<!-- -->) =&gt; void | the function to execute when a change is made to matter(s) in Smokeball. |
|  matterId | string | <i>(Optional)</i> the matter to subscribe to. |

<b>Returns:</b>

void

## Example


```
sdk.matters.observe(matter => {
  // Perform syncing code here.
});
```
