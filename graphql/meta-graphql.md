# Meta Graph API — GraphQL Conceptual Schema

## Overview

Meta's Graph API is a graph-traversal API modeled around nodes (objects), edges (connections between objects), and fields (properties of objects). While it does not expose a strict GraphQL SDL endpoint, its design maps closely to GraphQL concepts: every resource is a typed node with a globally unique ID, edges return paginated connection types, and the API supports field selection via the `fields` query parameter in the same spirit as GraphQL field selection sets.

This schema file (`meta-schema.graphql`) provides a conceptual GraphQL SDL representation of the Meta Graph API surface, covering the Facebook Graph API, Instagram Graph API, WhatsApp Cloud API, Messenger Platform, Threads API, and Meta Marketing API.

## Source References

- Graph API Overview: <https://developers.facebook.com/docs/graph-api/overview>
- Graph API Reference: <https://developers.facebook.com/docs/graph-api/reference>
- Marketing API Reference: <https://developers.facebook.com/docs/marketing-api/reference>
- Instagram Graph API: <https://developers.facebook.com/docs/instagram-platform>
- WhatsApp Cloud API: <https://developers.facebook.com/docs/whatsapp/cloud-api>
- Messenger Platform: <https://developers.facebook.com/docs/messenger-platform>
- Threads API: <https://developers.facebook.com/docs/threads>
- GitHub (Facebook): <https://github.com/facebook>
- GitHub (Facebook Research): <https://github.com/facebookresearch>

## Design Notes

### Node Model

Every object in the Meta Graph API has a globally unique `id` field. The root query entry points are `/me` (authenticated user), `/{id}` (any node by ID), and named top-level edges such as `/search`.

### Pagination

All edge (connection) types use Meta's cursor-based pagination envelope:

```graphql
type PageInfo {
  hasNextPage: Boolean!
  hasPreviousPage: Boolean!
  startCursor: String
  endCursor: String
}
```

### Versioning

The API is versioned via URL path prefix (e.g., `/v23.0/`). The schema types below reflect the v20+ surface area.

### Access Tokens

All queries require either a User Access Token, Page Access Token, App Access Token, or System User Access Token depending on the node being accessed. The `viewer` concept in the schema below represents the authenticated entity.

## Type Categories

| Category | Types |
|---|---|
| Identity & Profiles | User, Profile, AppUser, Application |
| Pages & Places | Page, Place, Location, PageLocation, CoverPhoto, ProfilePicture |
| Content | Post, Story, Status, Link, Photo, Video, Album, ReelsVideo, LiveVideo, AudioRoom |
| Groups & Events | Group, Event, EventAttendee, Milestone, Offer, Fundraiser |
| Insights & Analytics | VideoInsights |
| Advertising | Ad, AdAccount, AdSet, Campaign, AdCreative, AdImage, AdVideo, Audience, CustomAudience, PixelData, OfflineConversions, LeadAd |
| Commerce | Catalog, CatalogProduct, CatalogProductSet, CommerceOrder |
| WhatsApp | WhatsAppAccount, WhatsAppMessage, WABATemplate, PhoneNumber |
| Instagram | InstagramAccount, IGMedia, IGComment, IGMention, IGHashtag |
| Threads | ThreadsUser, ThreadsMedia, ThreadsReply |
| Messenger | Messenger, MessengerThread, MessengerMessage, MessengerAttachment |
| Reviews | Review, Rating |
| Extended Reality | MetaQuest, MixedReality, Horizon |
| Platform | CommunityAPI, WorkplacePost |
| Shared / Scalars | ID, String, Int, Float, Boolean, DateTime, URL, JSON |

## File

See `meta-schema.graphql` for the full SDL.
