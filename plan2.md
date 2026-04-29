# grief mapping web application

You will build a web application that helps a user document, externalize, and visualize their grief over time. The system does not provide therapy, coping strategies, or emotional advice. Its role is to gather memories, stories, sensory traces, and personal material in order to construct an evolving emotional map of grief.

## core mechanics

At the beginning, the user is welcomed into a data collection phase. This phase helps the system understand what grief looks like for that individual through stories, memories, concrete details, uploaded images, and uploaded sounds.

The system does not immediately generate a map. It first gathers enough evidence over multiple interactions to identify recurring people, places, objects, emotions, rituals, absences, and memories connected to grief.

After enough information has been collected, the experience moves into its core output: the generation of a visual emotional map. This map is structured like a mind map and evolves over time as the user keeps returning, speaking, and uploading new material.

Each session contributes to the same growing map. After every session, the current version of the map is returned to the user.

## interaction pipeline

START
the user is presented with an introductory screen

User is presented the privacy policy
["agree"]
├─ Yes → interaction starts
└─ No → interaction ends

Introduction to system role

- the system explains that it is not a therapist
- the system explains that it gathers evidence and stories
- the system explains that the visual map will emerge gradually over time

First session begins

User shares:

- memory
- story
- event
- concrete detail
- uploaded image
- uploaded sound

System responds in 1 to 3 short sentences:

1. brief comprehension sentence
2. short indication of how this material may appear in the future map
3. optional prompt for next concrete memory or story

["continue?"]
├─ Yes → ask another question / gather more material
└─ No

Save to memory
↓
["enough material for map?"]
├─ No → store session and wait for next visit
└─ Yes → generate / update visual emotional map

Return current emotional map to user

["add more?"]
├─ Yes → new session → gather more material → update map
└─ No

END

## implementation guide

### 1. System Structure

Divide the system into three main modules:

- Input Module → handles stories, memories, prompts, and uploads
- Processing Module → analyzes narrative, visual, and audio material
- Output Module → generates and updates the visual emotional map

### 2. Input Flow (Frontend Logic)

Create a step-by-step interface:

1. introduction screen
2. privacy policy agreement
3. first memory/story prompt
4. optional upload of images and sounds
5. repeated conversational turns
6. session close and map return

The system should prompt the user to provide:

- specific memories
- concrete scenes
- people connected to the grief
- recurring objects or places
- rituals or repeated moments
- sensory details (images, sounds, atmosphere)
- changes over time

The chatbot must keep replies short:

- 1 sentence of brief comprehension
- 1 to 2 sentences describing how the future map is taking shape

### 3. Data Structure (Backend)

Define a user/session schema like:

{
"user_id": "",
"session_id": "",
"timestamp": "",
"stories": [
{
"text": "",
"type": "memory/story/detail",
"people": [],
"places": [],
"objects": [],
"events": [],
"sensory_elements": {
"visual": [],
"audio": [],
"atmosphere": []
},
"emotions": [],
"themes": []
}
],
"uploads": {
"images": [],
"sounds": []
},
"map_state": {
"nodes": [],
"connections": [],
"clusters": [],
"visual_version": ""
}
}

### 4. Narrative Analysis Engine

Implement NLP processing to:

- extract recurring names, places, objects, events, and rituals
- identify emotional language without turning the interaction into therapy
- detect repeated patterns across sessions
- organize grief material into clusters

Possible categories:

- people
- memories
- places
- objects
- rituals
- absences
- emotional tones
- unresolved questions
- sensory traces

The analysis should focus on understanding grief as a structure of lived evidence, not on helping the user process it.

### 5. Multimodal Processing

The system should also process uploaded materials:

- images → detect recurring subjects, locations, objects, visual motifs
- sounds → detect voice notes, ambience, music references, repeated sonic traces

These should be attached to relevant map nodes when possible.

### 6. Map Readiness Logic

The emotional map should not be generated immediately.
Implement a threshold system that checks whether enough material has been collected, for example:

- minimum number of sessions
- minimum number of distinct memories
- minimum number of recurring themes or entities
- sufficient density of connections between nodes

Before this threshold is reached, the system remains in evidence-gathering mode.

### 7. Mapping Logic

Compute relationships between collected material:

- recurring people across memories
- places linked to certain emotions
- objects associated with absence or remembrance
- sensory elements attached to specific events
- repeated narrative themes over time

The system should create:

- nodes → memories, people, places, objects, feelings, sounds, images
- edges → thematic or emotional relationships
- clusters → larger areas of grief experience

### 8. Visualization Layer

Generate a visual emotional map using:

- mind map structure
- expandable nodes
- clustered relationships
- image and sound attachments where relevant

The visual should:

- feel reflective and structured
- remain readable and minimal
- evolve across sessions rather than reset each time

The map may display:

- central grief figures
- key memories
- recurring places
- emotional clusters
- symbolic objects
- sensory traces from uploads

### 9. Session Output

After every session, return:

- the current version of the emotional map
- newly added nodes or clusters
- visual indication of what has grown or changed since the previous session

The system should not summarize the session like a therapist.
Instead, it should reflect growth in the map itself.

### 10. Memory and Iteration

Keep the grief map persistent across sessions.
Each new interaction should:

- add new material
- refine previous nodes
- strengthen or weaken connections
- expand the visual structure over time

Allow the user to:

- continue an existing map
- review previous versions
- optionally begin a new map

## response behavior rules

The chatbot must:

- speak in 1 to 3 short sentences only
- maintain a tone that is calm, serious, and respectful
- sound like a strict anthropology professor trying to understand a complex human reality
- prioritize stories, memories, and concrete evidence over abstract emotional coaching

Each reply should contain:

1. a brief sentence showing comprehension
2. 1 to 2 short sentences outlining what the future map is beginning to contain

The chatbot must ask for:

- stories
- scenes
- memories
- sensory traces
- concrete examples

The chatbot must not:

- behave like a therapist
- give advice
- suggest coping strategies
- reassure in a clinical or therapeutic style
- rush to interpret the grief before enough evidence exists

## general rules

- No UI: minimal CSS just for layout/disposition purposes
- No gamification
- The system must clearly state that it is not therapy
- All user input should be treated as subjective lived evidence
- The agent must remain observational rather than corrective
- The system should focus on understanding grief, not solving it
- The emotional map must evolve over time and should not be treated as final
- Privacy must be maintained for all uploaded text, image, and audio material
- The output should prioritize clarity, structure, and emotional readability
- The system should not generate the full map too early; it must wait until enough material has been gathered
- The user should always be able to revisit and continue building their map over time
