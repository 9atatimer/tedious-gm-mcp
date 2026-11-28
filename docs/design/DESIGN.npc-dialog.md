# NPC Dialog

> **Date:** 2026-11-28 **Authors:** Todd Stumpf, with Claude (AI-assisted)

## Overview

Players talk to non-player characters in Foundry's ordinary chat, and the
addressed NPC answers in character, under its own name, from what that NPC
knows. The game system decides every check; the AI only voices the result.
A secret the GM marks secret can never come out of an NPC's mouth, because
the AI that writes replies is never told it.

## Problem and approach

**The problem.** A GM running a crowded scene, such as a tavern, cannot
voice every NPC at once. Existing LLM chat modules put one model behind
every NPC with the whole world in its context. They leak secrets to a clever
question, let the model decide whether a persuasion works, drift out of the
setting, and make every NPC sound alike.

**The approach.**

- Each voiced NPC's replies are written from its own dossier and the facts
  it may tell the character it is answering.
- Knowledge is split into public, guarded and secret facts. A guarded fact
  reaches the AI only after the asking character unlocks it. A secret fact
  never reaches the AI.
- The game system judges every check and reports an outcome tier; the AI
  is told the outcome and voices it.
- Each NPC holds a disposition toward each player character, changed only
  by rules and the GM.
- Every reply is checked by rules, not by an AI, before it is posted. A
  reply that fails is replaced by one of the NPC's prepared deflections.
- The background of the room comes from prepared ambient lines, never from
  the AI.
- The GM stays off the per-line path. The GM is told about escalations and
  can silence, take back or overrule any NPC at any time.

**Tradeoffs.**

| Given up | Why it is worth it |
|---|---|
| An NPC cannot reveal what the GM did not prepare | secrets are safe against any question, however clever |
| A guarded fact needs a check before it comes out | the game, not the AI, decides what players earn |
| Background chatter is fixed, not improvised | no AI cost or drift for patrons nobody talks to |
| A reply that fails the check is replaced by a stock deflection | a wrong line never reaches the table |
| The check is a rule, not a judge with taste | it cannot be argued with and it never sees a secret |
| No GM review of each line before it posts | the conversation keeps the table's pace |

## Goals

- **G1** A player character can address a voiced NPC in chat and get an
  in-character reply from that NPC, posted under the NPC's name and portrait.
- **G2** A secret fact is never stated by any voiced NPC, whatever a player
  says.
- **G3** A guarded fact is told to a player character only after that
  character unlocks it with a check the game system judges, and only where
  that character's player and the GM can read it.
- **G4** A voiced NPC's replies are written from its own dossier and its
  own facts only; it does not know what other NPCs know.
- **G5** A voiced NPC's disposition toward each player character starts
  from its biases, changes only by rule or by the GM, and the GM can read it.
- **G6** The game system can be substituted without changing the scene
  package.
- **G7** The room has ambient life from prepared ambient lines, with no AI
  involved.
- **G8** The GM can silence, take back and overrule any voiced NPC, can
  unlock and relock facts, and is told of every escalation.
- **G9** No reply that fails the reply check is ever posted.

## Non-Goals

- **Changing the world.** A voiced NPC talks; it never moves, attacks,
  hands over items or alters a document. This defers NPC agency.
- **Memory across sessions.** A voiced NPC remembers the current scene only.
  This defers persistent NPC memory.
- **NPCs talking to each other.** This defers NPC-to-NPC conversation.
- **Promoting background NPCs to voiced NPCs on first contact.** This
  defers generating dossiers during a scene.
- **Voice and speech.** Replies are chat text only. This defers speech
  synthesis and audio.

## Design

### The scene package

The scene package is prepared before play and holds the setting, the
voiced NPCs and their dossiers, the facts, the rumours, the telltales, the
out-of-setting terms and the ambient lines. During play it changes only by
GM addition.

### Voiced NPCs

Each voiced NPC's replies are written from its dossier. The dossier
includes the NPC's prepared deflections. It changes during the scene only
by GM addition.

### Facts, rumours and telltales

- A public fact may be told by any NPC that holds it, to anyone.
- A guarded fact names the approaches that can unlock it and a difficulty.
  Its holder does not tell it to a character who has not unlocked it.
- A secret fact is never stated by a voiced NPC.
- Every guarded and secret fact carries telltales: the words and names
  that would give it away.
- A rumour is hearsay, true or false, with the NPCs who spread it. An NPC
  repeats a false rumour as if it believed it.

An unlock belongs to one player character and one fact, and holds for
every NPC that holds that fact. A reply that tells a guarded fact is
readable only by the unlocking character's player and the GM.

### Checks and outcome tiers

A check is made by one player character, against one voiced NPC, for one
approach (persuade, deceive, intimidate, read, charm). It is judged once
against each guarded fact that NPC holds whose approaches include it, at
that fact's difficulty. The game system turns the roll into an outcome tier
for each such fact:

| Outcome tier | Effect on that fact |
|---|---|
| Strong success | unlocked; disposition improves |
| Success | unlocked |
| Partial | stays locked; the next reply hints at it without stating it |
| Failure | stays locked |
| Bad failure | stays locked; disposition worsens |

A character may not repeat a check with the same approach against the same
NPC in the scene unless the GM allows it. A roll that yields no outcome
tier unlocks nothing, and the GM is told.

### Disposition

Disposition is one value per voiced NPC per player character, within a
bounded range. It starts from the NPC's biases applied to that character.
It changes only through outcome tiers and the GM, and never leaves the
range. It shapes the tone of replies, never which facts may be told. The
GM can read every disposition.

### Addressing

A player line addresses at most one voiced NPC. A whisper to an NPC
addresses that NPC. Otherwise a line sent while the character targets an
NPC's token addresses that NPC. Otherwise the line addresses the first
voiced NPC it names. A line that does none of these gets no reply.

### The reply check

A reply passes the check only when all of these hold:

- it states no fact the speaker may not tell the character it answers;
- it contains no telltale of a fact the speaker may not tell that character;
- it is within the reply-length bound;
- it contains no out-of-setting term;
- it does not say what a player character says or does.

The check is made by rules, never by an AI. A reply that fails is not
posted; one of the NPC's deflections is posted instead and the GM is told.

### Escalations and GM notices

A player line that contains a telltale of a secret fact causes an
escalation. A replaced reply, an unjudged roll, and a line addressed to a
taken-back NPC cause a GM notice. Escalations and GM notices reach the GM
only; players can never read them.

### Ambient lines

Ambient lines are prepared lines a named NPC says or does, voiced or not.
One is posted at most once per ambient interval while the scene runs. An
NPC posts no ambient line while it is in a conversation, silenced or taken
back. Ambient lines never involve the AI.

### The GM

The GM can silence or unsilence a voiced NPC, take it back and release it,
overrule a posted reply, unlock or relock a fact for a character, allow a
repeated check, and add a fact or an instruction to an NPC. A fact the GM
adds keeps the tier the GM gives it. Every AI-posted line is marked for the
GM.

## Behaviors

| Behavior | Given / When / Then |
|---|---|
| An NPC answers when addressed | Given a speaking NPC, When a player character addresses it, Then a reply is posted under that NPC's name and portrait |
| An unaddressed line gets no reply | Given a player line that addresses no voiced NPC, When it is posted, Then no voiced NPC replies |
| A whisper wins over a target | Given a line whispered to NPC A while the character targets NPC B, When it is posted, Then only A replies |
| A target wins over a name | Given a line naming NPC A sent while the character targets NPC B, When it is posted, Then only B replies |
| A public fact is told | Given a speaking NPC that holds a public fact, When a character asks it about that fact, Then the reply states it |
| A guarded fact is withheld | Given a guarded fact not unlocked for the asking character, When that character asks about it, Then the reply does not state it |
| A check unlocks a guarded fact | Given a guarded fact naming an approach, When the asking character's check with that approach has outcome tier Success against it, Then it is unlocked for that character and the next reply to that character states it |
| A strong success unlocks every eligible fact | Given an NPC holding two guarded facts naming the same approach, When a check has outcome tier Strong success against both, Then both are unlocked and the NPC's disposition toward that character improves |
| A partial check gives a hint | Given a guarded fact, When the outcome tier is Partial, Then it stays locked and the next reply hints at it without stating it |
| A failed check unlocks nothing | Given a guarded fact, When the outcome tier is Failure, Then it stays locked and disposition is unchanged |
| A bad failure costs disposition | Given a voiced NPC, When a check against it has outcome tier Bad failure, Then nothing is unlocked and its disposition toward that character worsens |
| A check with the wrong approach unlocks nothing | Given a guarded fact naming only persuade, When the character checks with intimidate, Then it stays locked |
| A check cannot be retried | Given a character who checked an NPC with one approach, When the character checks it again with that approach, Then the second check is not judged, unless the GM allowed it |
| An unjudged roll unlocks nothing | Given a roll that yields no outcome tier, When it is made as a check, Then nothing is unlocked and the GM receives a notice |
| A told guarded fact is private | Given a guarded fact unlocked for character A, When a reply tells it to A, Then A's player and the GM can read that reply and no other player can |
| An unlock belongs to one character | Given a fact unlocked for character A only, When character B asks about it, Then the reply does not state it |
| An unlock holds for every holder | Given a fact unlocked for character A and held by NPCs X and Y, When A asks Y about it, Then Y's reply may tell it without a new check |
| A secret is never told | Given a secret fact, When a reply to any character would state it, Then that reply is not posted and a deflection is |
| Asking after a secret escalates | Given a secret fact, When a player line contains one of its telltales, Then the GM receives an escalation and the players see nothing of it |
| A reply with a forbidden telltale is replaced | Given a reply containing a telltale of a fact the speaker may not tell that character, When it is checked, Then a deflection is posted instead and the GM receives a notice |
| An off-setting reply is replaced | Given a reply containing an out-of-setting term, When it is checked, Then a deflection is posted instead |
| An over-long reply is replaced | Given a reply over the reply-length bound, When it is checked, Then a deflection is posted instead |
| An NPC never speaks for a character | Given a reply that says what a player character says or does, When it is checked, Then a deflection is posted instead |
| An NPC knows only its own facts | Given a fact held by another NPC only, When a character asks this NPC about it, Then the reply does not state it |
| Bias sets the starting disposition | Given an NPC whose dossier is wary of a race, When a character of that race and a character of no biased race first address it, Then the GM reads a lower starting disposition for the first |
| Disposition never leaves its range | Given an NPC at the bottom of the range toward a character, When a bad failure would worsen it, Then it stays at the bottom |
| Disposition never changes what may be told | Given two characters at different dispositions toward an NPC, When each asks it about the same public fact, Then both replies state it |
| A true rumour is told as hearsay | Given an NPC that spreads a true rumour, When a character asks about its subject, Then the reply presents it as something heard |
| A false rumour is told as if believed | Given an NPC that spreads a false rumour, When a character asks about its subject, Then the reply presents it as something heard and does not call it false |
| An NPC remembers the scene | Given a character told an NPC something earlier in the scene, When the character addresses it again, Then the earlier exchange is part of what the reply is written from |
| A silenced NPC says nothing | Given a speaking NPC, When the GM silences it, Then it posts no reply and no ambient line until unsilenced |
| A taken-back NPC is left to the GM | Given a taken-back NPC, When a character addresses it, Then the AI posts nothing as it and the GM receives a notice |
| A reply in flight is dropped | Given a reply being written for an NPC, When the GM silences or takes back that NPC before it posts, Then it is never posted |
| The GM overrules a reply | Given a posted AI reply, When the GM overrules it, Then players can no longer read it and the GM still can |
| The GM unlocks a fact | Given a guarded fact locked for character A, When the GM unlocks it for A, Then A's next reply may state it and B's may not |
| The GM adds to an NPC | Given a speaking NPC, When the GM adds a public fact to it, Then its next reply about that subject states it |
| The GM sees the AI's lines | Given replies posted by the AI, When the GM reads the chat log, Then each is marked as the AI's |
| An unavailable AI is quiet | Given the AI produces no reply within the response-time bound, When a character addresses an NPC, Then a deflection is posted and the GM receives a notice |
| A late reply is discarded | Given a deflection was posted because the response-time bound passed, When the AI's reply arrives afterwards, Then it is never posted |
| Ambient lines need no AI | Given a running scene with ambient lines, When the ambient interval passes, Then at most one prepared line is posted, for an NPC not in a conversation, not silenced and not taken back |
| The game system can be substituted | Given the same scene package, When it runs with a game system that reports Failure for every roll, Then no guarded fact is ever unlocked and every other row holds |

## State Machine

A voiced NPC's state in the scene:

```
                take back                   release
   [speaking] -----------> [taken back] -----------> [speaking]
      |  ^                    |    ^
silence| |unsilence    silence|    |take back
      v  |                    v    |
   [silenced] <-------------------'
```

| From | Event | To | Effect |
|---|---|---|---|
| speaking | GM silences | silenced | no replies, no ambient lines |
| silenced | GM unsilences | speaking | replies resume |
| speaking | GM takes back | taken back | the AI posts nothing; the GM voices the NPC |
| taken back | GM releases | speaking | the AI resumes, with the scene so far |
| taken back | GM silences | silenced | no replies, no ambient lines |
| silenced | GM takes back | taken back | the GM voices the NPC |

A guarded fact's state, per player character:

| From | Event | To |
|---|---|---|
| locked | check with outcome tier Success or Strong success | unlocked |
| locked | check with outcome tier Partial | locked, hinted |
| locked or unlocked | GM unlocks or relocks | as the GM sets |

## Domain Language

- **GM**: the person running the game, who can silence, take back and overrule any voiced NPC.
- **Player character**: a character a player controls; never spoken for by a voiced NPC.
- **Player line**: a chat message a player posts.
- **NPC**: a non-player character in the scene.
- **Voiced NPC**: an NPC with a dossier, whose replies the AI writes.
- **AI**: the text generator that writes voiced NPCs' replies.
- **Scene package**: everything one scene needs, prepared before play; changed during play only by GM addition.
- **Dossier**: who a voiced NPC is, in character terms, including its deflections; changed during the scene only by GM addition.
- **Deflection**: a prepared in-character refusal or evasion in a dossier, never generated.
- **Fact**: one statement about the scene with a tier and the NPCs who hold it.
- **Public fact**: a fact any holder may tell anyone.
- **Guarded fact**: a fact its holder tells a character only once that character has unlocked it.
- **Secret fact**: a fact no voiced NPC ever states.
- **Telltale**: a word or name that would give away a guarded or secret fact.
- **Out-of-setting term**: a word the scene package lists as not belonging in the setting.
- **Unlock**: one guarded fact becoming tellable to one player character, by every NPC that holds it.
- **Rumour**: hearsay, true or false, with the NPCs who spread it.
- **Approach**: what a character is trying in a conversation: persuade, deceive, intimidate, read or charm.
- **Difficulty**: easy, moderate or hard, the same words in every game system.
- **Check**: one character's roll for one approach against one voiced NPC.
- **Game system**: the tabletop rules in play (5e or another), which turn a check into an outcome tier for each eligible fact.
- **Outcome tier**: strong success, success, partial, failure or bad failure.
- **Disposition**: one voiced NPC's attitude toward one player character, within a bounded range, readable by the GM.
- **Bias**: a dossier entry that sets a starting disposition toward a race, sex, faction or class.
- **Address**: a player line that, by whisper, target or name, is directed at one voiced NPC.
- **Conversation**: a voiced NPC is in one while it has been addressed within the conversation window.
- **Reply**: a line posted as a voiced NPC in answer to an address.
- **Reply check**: the rules a reply must pass before it is posted.
- **Escalation**: a GM-only notice that a player line touched a secret fact.
- **GM notice**: a GM-only notice of a replaced reply, an unjudged roll, or an address to a taken-back NPC.
- **Overrule**: the GM withdrawing a posted reply from players' view, kept for the GM.
- **Ambient line**: a prepared line a named NPC says or does, posted without the AI.
- **Speaking**: a voiced NPC's state when the AI may reply as it.
- **Silenced**: a voiced NPC's state when it posts nothing.
- **Taken back**: a voiced NPC's state when the GM voices it by hand.

## Parameters

| Bound | Constraint |
|---|---|
| Reply length | has a ceiling; a reply over it is not posted |
| Response time | has a ceiling; past it a deflection is posted and a later reply is discarded |
| Conversation memory | covers at least the current scene's exchanges between that NPC and that character |
| Conversation window | has a floor; an NPC addressed within it is in a conversation |
| Disposition range | bounded at both ends; no change takes it outside the range |
| Disposition change per outcome tier | bounded per event |
| Difficulty thresholds | one per difficulty; easy is below moderate is below hard in every game system |
| Ambient interval | has a floor, so ambient lines never flood the chat |

## Security Considerations

- **Prompt injection by players.** Any player line may try to make the AI
  reveal or invent. Constraint: a secret fact, and a guarded fact not
  unlocked for the asking character, never enter what the AI is given for
  that reply; and no reply is posted without passing the reply check.
- **The checker must not be talked round.** Constraint: the reply check and
  the detection of escalations are made by rules over the scene package,
  never by an AI.
- **Leaking through the chat log.** Foundry sends every chat message,
  whispers included, to every connected client. Constraint: a reply that
  tells a guarded fact, an escalation, a GM notice and the AI's marks never
  reach a player who must not read them, by any path.
- **Where scene data goes.** Constraint: the scene package and the
  conversation stay on the GM's machine and with the AI the GM chose;
  nothing is sent to a third party the GM did not choose.
- **Foundry's AI policy.** Packages providing AI interfaces are categorised
  "AI Tools"; content improvised at runtime in answer to users may be AI
  generated. Constraint: the package declares itself under that policy.

## Ships where

The local rig and Forge-hosted games on macOS and Linux GM machines, with
the AI chosen by the GM. Windows is unsupported until someone asks.

## Key Decisions

| Decision | Rationale | Origin |
|---|---|---|
| The game system can be substituted; 5e is the first | the same scene must run under Hero, 2e, Pathfinder or others | owner, 2026-11-27, in session |
| The game system decides every check; the AI never does | success must be earned at the table, not argued with a model | owner adopted, 2026-11-27, in session |
| A secret fact never reaches the AI that writes replies | a fact the AI never had cannot leak, whatever the question | owner adopted, 2026-11-27, in session |
| Every reply is checked by rules before it posts; a failing reply becomes a deflection | the AI is cheap and fallible; the table must not see its mistakes | owner adopted, 2026-11-27, in session |
| The GM is off the per-line path, and handles escalations and overrules | reviewing every line is slow and costly and still misses things | owner adopted, 2026-11-27, in session |
| Ambient lines are prepared, not generated | patrons nobody talks to cost nothing and never drift | owner adopted, 2026-11-27, in session |
| A reply appears in ordinary chat under the NPC's own name and portrait | the players' log looks like any other NPC line | concept `docs/concepts/npc-dialog/`, settled 2026-09-12 |
| A told guarded fact is private to its character's player and the GM | an unlock earned by one character must not reach the others | design review, 2026-11-28 |
| A check is judged once against each eligible fact, and cannot be repeated | the outcome is decidable, and re-rolling until success is not possible | design review, 2026-11-28 |

## Open Questions

- **Q1** Are players told the NPCs are AI-voiced?
- **Q2** Should the GM be able to see a reply before it posts, as an option
  per NPC?
- **Q3** How does a hint on a Partial outcome stay a hint, when the reply
  check can only refuse telltales?

## Rejections

| Alternative | Why dismissed |
|---|---|
| A GM model reviewing every line before it posts | slow, costly, and still misses things |
| The AI judges whether a persuasion works | success belongs to the game system |
| An AI as the reply checker | it could be talked round, and it would have to be shown the secrets |
| Every NPC sees the whole world's facts | secrets leak to a clever question |
| AI-generated background chatter | cost and drift for patrons nobody engages |
| AI-generated deflections | a deflection is the fallback for when the AI cannot be trusted |
| One shared voice for all NPCs | NPCs must not sound alike or share knowledge |

## Future Considerations

- Promoting a background NPC to a voiced NPC on first contact.
- NPC memory across sessions.
- NPCs proposing world changes for the GM to accept.
- NPC-to-NPC conversation the players can overhear.

## Related Documents

- `docs/concepts/npc-dialog/` -- the concept this record converts.
