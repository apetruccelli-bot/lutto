# voice-based grief mapping web application

This web application helps a user speak about their grief through a voice-based conversation with an AI that gathers memories, stories, and concrete evidence over time. The system is not a therapist: it listens, documents, and gradually builds an evolving emotional map of grief, first as provisional checkpoints and eventually as a final network-style map.

## core mechanics

The experience begins with a voice-based data collection phase. The system listens to spoken memories, stories, and descriptions of grief, treating them as evidence rather than emotions to resolve.

The system does not generate a map immediately. It gathers enough material across multiple exchanges (minimum ~10 meaningful question-answer pairs) before producing the first provisional emotional map.

The map evolves over time. After key moments in the conversation, the system generates checkpoint maps to reflect its current understanding. The final map is only generated when the user confirms satisfaction.

## interaction pipeline

START
the user is presented with an introductory screen

User is presented the privacy policy
["agree"]
├─ Yes → interaction starts
└─ No → interaction ends

System introduction (voice or text):

- explains it is not a therapist
- explains it gathers stories and evidence
- explains that a visual map will emerge over time

Voice session begins

Loop:
User speaks (memory / story / detail)

System responds (1–3 short sentences):

1. brief comprehension
2. short indication of how this contributes to the map
3. next focused question

Speech-to-text conversion
↓
Store transcript + audio reference

Track interaction count

["continue?"]
├─ Yes → continue conversation
└─ No → pause session

Checkpoint logic:
↓
["≥10 meaningful exchanges?"]
├─ No → continue gathering
└─ Yes → generate first provisional map

During conversation:

- generate additional provisional maps when:
  - a significant memory is fully described
  - a recurring theme emerges
  - a cluster becomes clear

Return current map (JSON structure)

["are you satisfied?"]
├─ No → continue gathering and refining
└─ Yes → generate final emotional map

END

## implementation guide

### 1. System Structure

Divide the system into three main modules:

- Input Module → voice capture, speech-to-text, uploads
- Processing Module → narrative + pattern analysis
- Output Module → JSON network map generation

### 2. Input Flow (Frontend Logic)

Create a voice-first interface:

1. introduction screen
2. privacy agreement
3. microphone activation
4. guided conversational prompts
5. optional uploads (images, sounds)

The system should prompt for:

- specific memories
- lived moments
- people involved
- places and environments
- objects and symbols
- repeated events or rituals
- sensory details (sound, atmosphere)

All input should be:

- recorded as audio
- transcribed into text
- stored for analysis

### 3. Data Structure (Backend)

{
"user_id": "",
"session_id": "",
"interactions": [
{
"question": "",
"answer_text": "",
"audio_file": "",
"timestamp": ""
}
],
"extracted_data": {
"people": [],
"places": [],
"objects": [],
"events": [],
"themes": [],
"emotions": [],
"sensory": {
"sounds": [],
"visuals": [],
"atmosphere": []
}
},
"map_state": {
"nodes": [],
"edges": [],
"clusters": [],
"version": ""
}
}

### 4. Speech Processing Layer

Implement:

- speech-to-text transcription (required)
- optional audio tagging (tone, pauses, emphasis)

Store:

- raw audio
- transcript
- link between them

### 5. Narrative Analysis Engine

Use NLP to:

- extract key entities (people, places, objects)
- detect recurring memories and themes
- identify patterns across sessions
- cluster related experiences

Focus on:

- evidence gathering
- structural understanding
  NOT:
- emotional guidance
- therapeutic interpretation

### 6. Map Readiness Logic

The system should only generate a map when:

- at least ~10 meaningful exchanges are completed
- sufficient diversity of memories exists
- recurring elements or patterns are detected

Before this threshold:

- remain in data collection mode

### 7. Mapping Logic (Network Graph)

Construct a network graph where:

Nodes represent:

- people
- places
- memories/events
- objects
- emotions
- sensory elements (sounds/images)

Edges represent:

- relationships (e.g. “occurred with”, “associated with”, “reminds of”)
- emotional connections
- temporal links

Clusters represent:

- recurring themes
- significant grief structures (e.g. loss, absence, rituals)

### 8. Visualization Output (JSON)

For this experiment:

- output the map as JSON

Example:

{
"nodes": [
{"id": "mother", "type": "person"},
{"id": "hospital", "type": "place"},
{"id": "last_visit", "type": "event"}
],
"edges": [
{"source": "mother", "target": "hospital", "relation": "associated_with"},
{"source": "mother", "target": "last_visit", "relation": "memory"}
]
}

### 9. Provisional Maps

Generate maps:

- after major memory clusters are formed
- after the 10-interaction threshold
- intermittently when meaningful structure appears

These maps act as:

- checkpoints for validation
- reflections of current understanding

### 10. Final Map Generation

The final map is generated when:

- the user explicitly confirms satisfaction

The final map should:

- include all nodes and connections
- reflect strongest patterns and clusters
- represent the full grief structure gathered

### 11. Image Generation Rules

The system may:

- generate abstract or environmental visuals

The system must NOT:

- generate images of people

### 12. Response Behavior Rules

The chatbot must:

- respond in 1–3 short sentences
- maintain a calm, precise, respectful tone
- sound like a strict anthropology professor

Each reply includes:

1. brief comprehension
2. short map-building insight
3. next focused prompt

The chatbot must:

- ask for stories and concrete memories
- remain curious and investigative

The chatbot must NOT:

- act as a therapist
- give advice or coping strategies
- interpret or resolve grief
- rush conclusions

## general rules

- Voice is the primary interaction mode
- No UI: minimal CSS just for layout/disposition purposes
- No gamification
- The system must clearly state it is not therapy
- All input is subjective lived evidence
- The agent remains neutral and observational
- The map evolves over time and is never fixed
- Privacy must be maintained for all audio, text, and uploads
- The system should prioritize clarity, structure, and readability
- The user must be able to pause and return over time
