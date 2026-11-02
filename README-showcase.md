# Limitless Fantasy

**An AI-driven high-fantasy overhaul for Minecraft Bedrock Edition, where villagers hold real conversations.**

Right-click a villager, choose **Talk**, and type anything. There are no dialogue wheels and no canned lines. A locally hosted language model writes every reply in character, shaped by who the villager is, how they feel about you, and what they remember you doing.

![Talking to an elf villager](docs/images/conversation.png)

---

## Highlights

- **Conversational NPCs.** Free-text chat with every villager, powered by a local LLM. Nothing goes to a cloud API.
- **Memory and gossip.** Villagers remember what each player said, gave, or did. Each memory is rated 1–10 by importance and fades over time. Your deeds also spread to nearby villagers as gossip.
- **Relationships.** Friendship, reputation, romance, and respect scores change the tone of every reply and unlock new interactions.
- **Six peoples.** Elves, Dwarves, Humans, Vikings, Samurai, and Beast-Folk, each with its own villages, speech style, nobility, and quests.
- **Combat and magic.** 27 spells across nine elements, weapon mastery, race-specific armor, five roaming dragons, and boss-tier "Ascendants."
- **Living world.** Villager schedules, courts and succession, territory, and war between peoples.
- **Browser world map.** The backend serves a live map of explored terrain and territories at `localhost:3001`.

## Architecture

The mod has two parts that run side by side:

```
┌─────────────────────────────┐         ┌──────────────────────────────┐
│  Minecraft Bedrock server   │         │  Brain (Node.js)             │
│                             │  chat,  │                              │
│  behavior_pack/             │ events, │  • Builds each NPC's prompt  │
│   • World logic (Script API)│ ──────► │  • Memory store (memory.db)  │
│   • Villages, mobs, spells  │         │  • Relationship scores       │
│   • Schedules, quests, war  │ ◄────── │  • Local LLM for replies     │
│                             │ replies,│  • Web map server (:3001)    │
│  resource_pack/             │ effects │                              │
│   • Models, textures, UI    │         │                              │
└─────────────────────────────┘         └──────────────────────────────┘
```

- **Behavior pack** (JavaScript, `@minecraft/server` 2.0.0). Handles everything in the world: village generation, NPC behavior, combat, spells, items, and quests. It sends player messages and game events to the brain.
- **Brain** (Node.js). Handles conversation, memory, and feelings. For each message, it builds a prompt from the villager's personality, their people's speech style, relationship scores, relevant memories, and what just happened. Then it asks the local model for a reply and returns it along with any changes to friendship or reputation.
- **Resource pack.** Custom models, textures, and UI.

The world still runs if the brain is offline; villagers just can't answer free-text chat.

### How a villager's reply is built

1. The player types a message in the Talk menu.
2. The behavior pack sends the message, the villager's ID, and recent events to the brain.
3. The brain loads the villager's profile (name, people, job, three personality traits, family) and their memories of this player.
4. It assembles a prompt: identity, speech style, relationship tier, the most important memories, recent gossip, and the player's message.
5. The local LLM writes the reply. The brain also judges how the villager took the message and updates the relationship scores.
6. The reply and score changes go back to the game, and the exchange is saved as a new memory.

## Media

<!-- TODO: replace with your own files in docs/images/ -->
| | |
|---|---|
| ![Conversation](docs/images/conversation.png) | ![Village](docs/images/village.png) |
| ![Dragon](docs/images/dragon.png) | ![World map](docs/images/map.png) |

<!-- TODO: drag your demo .mp4 into the GitHub editor here -->

## Source code

This repository is a showcase. The source (behavior pack, resource pack, and brain) lives in a private repository and is available to recruiters and interviewers on request.

## Development

Development is AI-assisted: I design each system, write the specs and behavior rules, and direct AI coding tools to implement them. Then I test in-game, debug, and iterate. The full design is documented in a 120+ page player guide.

## Status

Active development. Current version: **0.0.382** (October 2026).

## Author

**Lucas Timbuc**, Computer Science, Wayne State University
[LinkedIn](https://www.linkedin.com/in/lucas-timbuc-a72325291)

---

*Not affiliated with Mojang or Microsoft. Minecraft is a trademark of Mojang Studios.*

© 2026 Lucas Timbuc. All rights reserved. 
