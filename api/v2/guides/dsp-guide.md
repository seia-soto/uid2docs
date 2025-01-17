[UID2 Overview](../../../README.md) > [Getting Started](../../README.md) > [v2](../README.md) > [Integration Guides](README.md) > DSP Integration Guide

# DSP Integration Guide

This guide is for DSPs who transact on UID2s in the bid stream and includes the following sections:

* [Integration Steps](#integration-steps)
* [FAQs](#faqs)

## Integration Steps 

The following describes the integration workflow for DSP to support UID2 as part of RTB, which consists of two major steps:
1. [Honor user opt-outs](#honor-user-opt-outs)
2. [Decrypt UID2 tokens to use in RTB](#decrypt-uid2-tokens-for-rtb-use)

![DSP Flow](https://mermaid.ink/svg/eyJjb2RlIjoiICBzZXF1ZW5jZURpYWdyYW1cbiAgICBwYXJ0aWNpcGFudCBVIGFzIFVzZXJcbiAgICBwYXJ0aWNpcGFudCBTU1BcbiAgICBwYXJ0aWNpcGFudCBEU1BcbiAgICBwYXJ0aWNpcGFudCBVSUQyIGFzIFVJRDIgU2VydmljZVxuICAgIHBhcnRpY2lwYW50IFRDIGFzIFRyYW5zcGFyZW5jeSAmIENvbnNlbnQgUG9ydGFsXG4gICAgTm90ZSBvdmVyIFUsVEM6IDEuIEhvbm9yIHVzZXIgb3B0LW91dHMuXG4gICAgVS0-PlRDOiAxLWEuIFVzZXIgb3B0cyBvdXQuXG4gICAgYWN0aXZhdGUgVENcbiAgICBUQy0-PlVJRDI6IDEtYi4gVUlEMiBzZXJ2aWNlIHJlY2VpdmVzIG9wdC1vdXQuXG4gICAgZGVhY3RpdmF0ZSBUQ1xuICAgIGFjdGl2YXRlIFVJRDJcbiAgICBVSUQyLT4-RFNQOiAxLWMuIERTUCByZWNlaXZlcyBvcHQtb3V0LlxuICAgIGRlYWN0aXZhdGUgVUlEMlxuICAgIE5vdGUgb3ZlciBVLFRDOiAyLiBEZWNyeXB0IFVJRDIgdG9rZW5zIHRvIHVzZSBpbiBSVEIuXG4gICAgU1NQLS0-PkRTUDogVGhlIFNTUCBjYWxscyBhIERTUCBmb3IgYmlkLlxuICAgIERTUC0-PkRTUDogMi1hLiBEZWNyeXB0IFVJRDIgdG9rZW5zLlxuICAgIERTUC0-PkRTUDogMi1iLiBFeGVjdXRlIGJpZGRpbmcgbG9naWMsIGhvbm9yaW5nIHVzZXIgb3B0LW91dHMuXG4iLCJtZXJtYWlkIjp7InRoZW1lIjoiZm9yZXN0In0sInVwZGF0ZUVkaXRvciI6ZmFsc2V9)

### Honor User Opt-Outs

To receive and honor user opt-outs from the UID2 service, DSPs establish a pre-configured interface and provides it to the UID2 service during onboarding. The UID2 service sends the user's UID2 and an opt-out timestamp to the pre-determined interface. Examples of interfaces include webhooks and API endpoints.

The UID2 service will send the following data within seconds of a user's opt-out, which the DSP records and uses the bidding logic defined in [Decrypt UID2 Tokens for RTB Use](#decrypt-uid2-tokens-for-rtb-use).

| Parameter | Description |
| :--- | :--- |
| `identity` | The UID2 for the user who opted out. |
| `timestamp` | The time when the user opted out. |


The following example  illustrates a webhook configured to receive the UID2 and a corresponding timestamp:

```html
https://dsp.example.com/optout?user=%%identity%%&optouttime=%%timestamp%%
```
#### Bidding Opt-Out Logic

Use the logic below during bidding (2-b) to honor a user's opt-out.

Leverage the provided [RTB SDK](../sdks/dsp-client-v1-overview.md) to decrypt incoming UID2 tokens. The response contains the UID2 and time the UID2 was created, represented in the psuedocode below as `established_timestamp`. DSPs are required to check the most recent opt-out timestamp for a UID2, represented in the pseudocode below as `optout_timestamp`. 

The following diagram illustrates opt-out logic.

![DSP Opt-Out Check](https://mermaid.ink/svg/eyJjb2RlIjoiZ3JhcGggTFJcbkFbRGVjcnlwdCBVSUQyIFRva2VuXSAtLT4gQltSZXRyaWV2ZSBPcHQtb3V0IGZvciBVSUQyXVxuICAgIEIgLS0-IEN7Q2hlY2sgT3B0LW91dH1cbiAgICBDIC0tPiB8T3B0ZWQgT3V0fCBEW0JpZCB3aXRob3V0IFVJRDJdXG4gICAgQyAtLT4gfE5vdCBPcHRlZCBPdXR8IEVbQmlkIHdpdGggVUlEMl1cbiIsIm1lcm1haWQiOnsidGhlbWUiOiJmb3Jlc3QifSwidXBkYXRlRWRpdG9yIjpmYWxzZX0)

If the `established_timestamp` value is less than the `optout_timestamp` value, the user opted out and the UID2 should not be used for RTB. In these cases, it is up to the DSP whether they would like to send an alternate ID for bidding or not bid.

The logic for the <b>check opt-out</b> step is the following:

```java
if (established_timestamp < optout_timestamp) {
  // Opted out
}
```

### Decrypt UID2 Tokens for RTB Use

| Step | SDK | Description |
| :--- | :--- | :--- |
| 2-a | [RTB SDK](../sdks/dsp-client-v1-overview.md)  | Leverage the provided SDK to decrypt incoming UID2 tokens. The response contains the `UID2` and the UID2 creation time. |
| 2-b | | DSPs are required to honor opt-out protocol for UID2s. For details on configuring user opt-outs and honoring them during bidding, see [Honor user opt-outs](#honor-user-opt-outs). |

## FAQs
### How do I know which decryption key to apply to a UID2?
The provided [RTB SDK](../sdks/dsp-client-v1-overview.md) updates decryption keys automatically. Metadata supplied with the UID2 token discloses the IDs of the decryption keys to use. 

### Where do I get the decryption keys?
You can use the [RTB SDK](../sdks/dsp-client-v1-overview.md) library to communicate with the UID2 service and fetch the latest keys. To make sure that the keys remain up-to-date, it is recommended to fetch them periodically, for example, once every hour. 

### How do I know if/when the salt bucket has rotated?
The DSP is not privy to when the UID2 salt bucket rotates. This is similar to a DSP being unaware if users cleared their cookies. Salt bucket rotation has no significant impact on the DSP.  

### Should the DSP be concerned with latency?
The UID2 service does not introduce latency into the bidding process. Any latency experienced can be attributed to the network, not the UID2 service.

### How should the DSP maintain proper frequency capping with UID2?
The UID2 has the same chance as a cookie of becoming stale. Hence, the DSP can adapt the same infrastructure currently used for cookie or deviceID-based frequency capping for UID2. For details, see this [FAQ](../guides/advertiser-dataprovider-guide.md#how-do-i-know-when-to-refresh-the-uid2-due-to-salt-bucket-rotation) on salt bucket rotation. 

### Will all user opt-out traffic be sent to the DSP?
Yes, all opt-outs from the UID2 [Transparency and Control Portal](https://transparentadvertising.org/) hit the opt-out endpoint, which the DSP must configure to [honor user opt-outs](#honor-user-opt-outs).

### Is the DSP expected to handle opt-out signals only for the UID2s that they already store?
In some cases a DSP may receive a UID2 token for a newly-stored UID2 where the token is generated before the opt-out timestamp. The DSP is not allowed to bid on such tokens. It is therefore recommended to store all opt-out signals regardless of whether the corresponding UID2 is currently stored by the DSP or not. For details, see the diagram in [Bidding Opt-Out Logic](#bidding-opt-out-logic).

### How long should the DSP keep the opt-out list?
At least for 30 days.

### Is the UID of an opted-out user sent to the opt-out endpoint in an encrypted form?

No. It is sent as an unencrypted (raw) UID2.

### What request type do  opt-outs use? 

Typically GET requests, but different DSPs may use different types.

### How strict are the requirements for honoring opt-outs? 

Opt-outs must be always respected. It may take some time for an opt-out request to propagate through the system during which time it is expected that some bids may not honor the opt-out.

### How many decryption keys may be present in memory at any point?

There may be thousands of decryption keys present in the system at any given point.
