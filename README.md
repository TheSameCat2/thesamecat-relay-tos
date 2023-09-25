# About Our Relay
We are dedicated to promoting transparency and accountability in our operations. This document represents our best efforts at clarifying how we operate, what our relay offers, and what we consider acceptable use.
## Terms of Service
This service (and supporting services) are provided "as is", without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose and non-infringement.

By connecting to this relay, you agree:

- To not engage in spam
- To not flood
- To not expect content moderation
- To not misuse or abuse the relay service and other supporting services
- To not disseminate illegal content or material
- That requests to delete content you published cannot be guaranteed
- That this relay has no control over any content published in other relays
- That some services, such as but not limited to the privilege to publish content may require payment(s)
- That charge backs from payments may result in the termination of the privilege to use the service
- That the service might be revoked to you at the operator's sole discretion if found in violation of these terms
- That the terms of service may change at any time in the future without explicit notice
- To grant us the necessary rights to your content to provide the service to you and to other users for an unlimited time
- To use the service in compliance with all laws, rules, and regulations applicable to you
- To use the service in good faith and not seek to get the relay operator(s) in trouble
- That the service may throttle, rate limit or revoke your access to any content and/or your privilege to publish content for any reason
- That the content you publish to this relay will be further broadcast to any interested client and/or accepting relay
- To not infringe on the right of others to publish content to this relay as allowed by the terms of service
- That this service is not targeted, nor intended for use by, anyone under the legal age in their respective jurisdiction
- To be of legal age or have sufficient legal consent, permission and capacity to use this service
- That the service may be temporarily shutdown or permanently terminated at any time and without notice
- That the content published by you and other users may be removed at any time and without notice and for any reason
- To have your IP address and/or public key collected for the purpose of detecting abuse, spam or misuse
  
In addition you understand that:

- Nostr is a decentralized and distributed network of relays that relays data by users.
- Censorship resistance is practiced by posting to multiple relays and running your own private relay.
- The responsibility of filtering and moderating is the sole responsibility of the users and not of the relays.
- You may be inadvertently exposed to content that you might find triggering, disturbing, distasteful, immoral or against your views.
- The relay operator is not liable and has no involvement in the type, quality and legality of the content being produced by users of the relay.

## Nice legalese, but what will get me blacklisted?
We are a free speech relay, meaning we will not moderate content, regardless of how objectionable it may seem, unless it is:
* Posting, linking to, or promoting CSAM
* Intentionally performing technological actions that harm the relay (Don't be hackerman)

## Relay Configuration

We impose some limitations to events on our relay to protect the relay from abuse, and keep performance high.
```
  event:
    eventId:
      minLeadingZeroBits: 0
    kind:
      whitelist: []
      blacklist: [ 1065 ]
```
We prohibit posts using NIP-95 (Kind 1065) because:
* It places undue burden on our relay
* Because tools for moderating images on relays are inadequate to allow us to protect ourselves from legal action due to illegal (CSAM) content
* Because it offers no advantage we consider adequate to justify the difficulties mentioned above

```
    pubkey:
      minBalance: 0
      minLeadingZeroBits: 0
      whitelist: []
      blacklist: []
    createdAt:
      maxPositiveDelta: 900
      maxNegativeDelta: 0
```
We do not require proof of work, and we do not limit you from rebroadcasting events from the past in any way. Events are not accepted for the future though, outside of a reasonable buffer to allow for clocks being mis-set.

```
    content:
    - description: 64 KB for event kind ranges 0-10 and 40-49
      kinds:
      - - 0
        - 10
      - - 40
        - 49
      maxLength: 65536
    - description: 96 KB for event kind ranges 11-39 and 50-max
      kinds:
      - - 11
        - 39
      - - 50
        - 9007199254740991
      maxLength: 98304
```
We impose size limits on events to keep our relay fast.
* The length of an event may vary depending on language
	* Using latin characters, most characters are 1 byte, allowing for 65,536 characters in (for example) a Kind 1 post.
* We will not lower these limits. They are already very conservative. They may be raised in the future as needs arise.

```
rateLimits:
    - description: 6 events/min for event kinds 0, 3, 40 and 41
      kinds:
      - 0
      - 3
      - 40
      - 41
      period: 60000
      rate: 6
    - description: 12 events/min for event kinds 1, 2, 4 and 42
      kinds:
      - 1
      - 2
      - 4
      - 42
      period: 60000
      rate: 120
    - description: 30 events/min for event kind ranges 5-7 and 43-49
      kinds:
      - - 5
        - 7
      - - 43
        - 49
      period: 60000
      rate: 30
    - description: 24 events/min for replaceable events and parameterized replaceable
        events
      kinds:
      - - 10000
        - 19999
      - - 30000
        - 39999
      period: 60000
      rate: 24
	- description: 60 events/min for ephemeral events
      kinds:
      - - 20000
        - 29999
      period: 60000
      rate: 60
    - description: 10000 events/hour for all events
      period: 3600000
      rate: 720
```
Rate limits protect the relay from spam and abuse. We have raised some of these limits, since we are already using a whitelist to protect the relay from bots.

```
  client:
    subscription:
      maxSubscriptions: 10
      maxFilters: 10
```
We limit clients to maintaining 10 subscriptions at a time.

### Post Retention
We do not purge your posts at this time, nor do we intend to.
Should storage become an issue, we will notify people through Nostr multiple times at least one month in advance to allow for backups.

### IP Logging
Your IP address may appear in ephemeral logs during the normal operation of the relay. We do not store these logs, and their age is limited to the default log retention size for Docker containers (100MB).

We recommend using a VPN if your IP address is sensitive information.

We will never release your IP address to a third party unless ordered to do so by a United States Court Order.
