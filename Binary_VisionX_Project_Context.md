# Binary VisionX — Project Context and Working Reference

> \*\*Purpose:\*\* This file preserves the project context, design principles, recurring deliverables, and collaboration preferences established in the Binary VisionX project. Use it as a continuity document when moving the work to another assistant or workspace.
>
> \*\*Accuracy note:\*\* Distinguish confirmed project direction from candidate technologies, proposals, illustrative targets, and unverified implementation details. Do not describe a feature as implemented or tested unless the team has confirmed it.

\---

## 1\. Project Identity

* **Project name:** Binary VisionX
* **Shared project/workspace:** Binary VisionX : Innovative Design Lab
* **Core concept:** An AI-powered wearable assistive vision system intended to help people with visual impairments understand nearby objects and obstacles through timely audio feedback and voice interaction.
* **Primary design goal:** Provide useful environmental awareness while prioritizing immediate, dependable safety alerts.
* **Documentation file previously requested:** `binary\_visionx.md`
* **SIH context:** Previous discussions identified SIH 2026 problem statement **SIH26113**, described in the conversation as belonging to Human Augmentation Technology under the Hardware category. However, the user explicitly requested that SIH-related material be excluded from `binary\_visionx.md`. Include SIH context only when a particular deliverable explicitly requires it.

## 2\. Intended User Experience

The intended system should help a wearer:

1. Capture the surrounding environment through a wearable camera.
2. Detect relevant objects and potential obstacles.
3. Identify hazards and prioritize immediate safety information.
4. Deliver concise audio alerts quickly.
5. Offer richer scene descriptions when useful.
6. Support voice-based interaction and questions about the environment.
7. Support Tamil and English as initial language priorities, with Hindi considered as a possible later addition if practical.
8. Allow the user to speak in a supported language and receive the response in that same language where the speech pipeline supports it.
9. Continue providing essential safety feedback when the cloud or internet connection is unavailable.

These are intended capabilities and design goals. Their current implementation status must be verified before reporting them as completed.

## 3\. Architecture and System Workflow

The central architecture direction is a **hybrid edge–cloud system**.

### 3.1 Input layer

* A wearable camera captures frames from the user's surroundings.
* Frame capture and preprocessing prepare the visual input for inference.
* Additional sensors for distance or obstacle ranging may be considered, but the final sensor configuration has not been confirmed.

### 3.2 Local edge-processing layer

* **Candidate hardware:** Raspberry Pi 4 or Raspberry Pi 5, with an 8 GB RAM configuration discussed.
* **Purpose:** Handle time-critical processing and immediate safety responses locally.
* **Potential tasks:** Frame preprocessing, lightweight object detection, basic obstacle awareness, prioritization of hazards, and playback of locally available safety messages.
* **Key requirement:** Essential safety alerts should not depend on an active internet connection or a cloud response.

The exact Raspberry Pi model, camera, inference model, and final local workload remain unconfirmed.

### 3.3 Cloud-processing layer

* **Candidate hosting direction:** A VPS, with Oracle Cloud mentioned as an example rather than a finalized deployment choice.
* **Purpose:** Support computationally heavier models and richer scene understanding when connectivity is available.
* **Potential tasks:** Detailed scene descriptions, more complex visual reasoning, or other functions that exceed the practical limits of the edge device.
* **Fallback:** If the network is unavailable or slow, local safety features should remain available. Cloud-only features may be delayed or temporarily unavailable, but should not block immediate hazard alerts.

The final cloud provider, model, API, data-routing method, and deployment architecture have not been confirmed.

### 3.4 Decision and prioritization layer

The system needs a clear decision policy for selecting which information to communicate and when.

A sensible design direction is to prioritize:

1. **Immediate hazards:** Short, direct, low-latency alerts.
2. **Relevant nearby objects:** Concise identification when useful.
3. **General scene understanding:** Richer descriptions when requested or when the system has sufficient time and confidence.
4. **Voice questions:** Process user requests without delaying urgent safety alerts.

This is a proposed prioritization policy, not a claim that a final decision engine has already been implemented. The exact decision-engine design remains open.

### 3.5 Audio-output layer

* Deliver alerts through an appropriate wearable or connected audio-output device.
* Keep urgent alerts short, distinct, and understandable.
* Avoid excessive or repetitive announcements that could overwhelm the wearer.
* Consider alert confidence, severity, distance, and repetition suppression when designing the notification policy.

The final audio hardware and alert policy have not been confirmed.

### 3.6 Voice-interaction and language layer

The intended speech workflow may include:

1. Capture the user's voice.
2. Perform automatic speech recognition (ASR).
3. Identify or select the appropriate language.
4. Interpret the user's intent.
5. Retrieve relevant visual context or invoke an appropriate processing component.
6. Generate a response in the user's language.
7. Convert the response to speech using text-to-speech (TTS).
8. Play the response through the audio output.

Tamil and English are the initial priorities. Hindi may be considered later if it fits the project's time, hardware, and model constraints. Offline speech recognition and speech synthesis are important considerations, but the selected engines and their offline capabilities are not yet confirmed.

**Safety principle:** Voice interaction and detailed descriptions must not prevent or unnecessarily delay urgent local safety alerts.

## 4\. Candidate Technology Stack

The following technologies have been discussed as possibilities. They must be treated as candidates until the team confirms the actual implementation.

|Area|Candidate technologies or direction|Status|
|-|-|-|
|Main programming language|Python|Discussed candidate|
|Image processing|OpenCV, NumPy|Discussed candidates|
|Object detection|YOLO-family model|Model/version not finalized|
|Edge computing|Raspberry Pi 4 or Raspberry Pi 5, 8 GB RAM discussed|Exact device not confirmed|
|Operating system|Linux / Raspberry Pi OS|Candidate|
|Cloud hosting|VPS; Oracle Cloud mentioned as an example|Provider not finalized|
|Speech recognition|ASR engine suitable for supported languages|Not selected|
|Speech output|TTS engine suitable for supported languages|Not selected|
|Language understanding|NLP and intent processing|Implementation not finalized|
|Communication|Network APIs or other edge–cloud communication mechanism|Not finalized|
|Power|Rechargeable wearable battery system|Chemistry and specifications not finalized|

Do not claim a particular model, framework version, API, sensor, or service is being used unless confirmed by project evidence.

## 5\. Wearable Design and Power

A wearable power unit has been discussed as an arm- or wrist-worn rechargeable battery module that supplies power to the glasses and associated electronics.

* Lithium-ion was discussed as a possible battery chemistry.
* Lithium-polymer was discussed as an alternative.
* Neither chemistry should be described as the final selection without confirmation.
* Battery capacity, voltage, current requirements, charging design, protection circuitry, total system power draw, and expected runtime remain open.
* The physical design should consider weight distribution, comfort, cable routing, heat, secure attachment, ease of charging, and safe operation.
* Power planning must account for the camera, edge computer, sensors, audio components, and any other connected hardware.
* Battery runtime should be measured using realistic operating conditions rather than estimated from battery capacity alone.

A final battery recommendation requires a power budget based on the selected hardware and actual measurements.

## 6\. Safety, Reliability, and Performance

Safety should shape the system architecture, not be treated as a feature added at the end.

### Core principles

* Keep urgent safety detection and alerts available locally.
* Do not make basic hazard alerts dependent on internet connectivity.
* Design clear fallback behavior for cloud outages, network delays, and component failures.
* Keep alerts concise and distinguishable from general descriptions.
* Consider false positives, missed detections, confidence thresholds, and repeated alerts.
* Be transparent about system limitations. Do not imply that the device replaces a white cane, guide dog, orientation-and-mobility training, or other established mobility aids.
* Avoid claiming that object detection alone guarantees collision avoidance or reliable distance estimation.

### Validation areas

The project should be evaluated with a documented test plan that covers:

* Local inference latency.
* End-to-end alert latency.
* Cloud response latency and network variability.
* Behavior when offline or disconnected.
* Object-detection accuracy in representative environments.
* Missed hazards and false alarms.
* Distance estimation, if distance sensing is included.
* Audio clarity and alert comprehensibility.
* Tamil and English speech-recognition and speech-synthesis quality.
* Battery runtime under realistic workloads.
* Device temperature, comfort, and wearability.
* Reliability across indoor and outdoor lighting and environmental conditions.

**Important:** Earlier latency figures such as 0.2–0.5 seconds for local processing or 0.8–2.5 seconds for cloud processing were illustrative examples, not measured project results. Do not report them as benchmarks. Set targets explicitly and validate them through testing.

## 7\. Known Decisions, Proposals, and Open Questions

### Established project direction

* The project is called Binary VisionX.
* It is intended to be a wearable AI-assisted vision system for people with visual impairments.
* The architecture direction is hybrid edge–cloud.
* Immediate safety alerts should work locally and should not depend on cloud connectivity.
* Tamil and English are initial language priorities.
* A wearable rechargeable power arrangement has been considered.
* The user wants detailed, technically grounded project explanations and polished documentation.

### Proposals that require confirmation

* Raspberry Pi 4 or Raspberry Pi 5 with 8 GB RAM.
* A YOLO-family object detector.
* OpenCV, NumPy, and Python.
* A VPS, with Oracle Cloud mentioned as one possible provider.
* Lithium-ion or lithium-polymer battery chemistry.
* Additional distance sensors.
* Specific ASR, TTS, NLP, and scene-understanding components.

### Open technical questions

1. Which Raspberry Pi model and exact hardware configuration will be used?
2. Which camera, audio output, microphone, and optional sensors will be selected?
3. Will distance estimation rely on a sensor, stereo/depth camera, monocular estimation, or another method?
4. Which object-detection model and version will run locally?
5. Which tasks will run locally, and which may be delegated to the cloud?
6. How will frames and metadata be routed between edge and cloud?
7. Which cloud provider, model, API, and deployment approach will be used?
8. Which Tamil and English ASR/TTS engines will be used, and what works offline?
9. What battery chemistry, capacity, voltage, protection circuitry, and charging method will be selected?
10. What are the target alert latency, acceptable false-alarm rate, and missed-hazard rate?
11. How will the system be tested with representative users and environments?
12. What privacy controls, retention rules, and consent measures will govern captured images, audio, and cloud processing?

Do not invent answers to these questions. Clearly label assumptions and recommendations until decisions are made.

### Required working method

1. Inspect the supplied source files and template before drafting.
2. Understand the project and the specific week's topic.
3. Extract relevant facts from the source material rather than inventing progress.
4. Plan the page structure and content hierarchy before formatting.
5. Write natural, specific, professional content that sounds like a real project team.
6. Preserve the template's relevant structure, typography, branding, and layout conventions.
7. Ensure the content is technically consistent with the Binary VisionX architecture.
8. Render or inspect the completed PDF page by page.
9. Correct text collisions, overflow, broken alignment, awkward page breaks, inconsistent spacing, and readability issues.
10. Deliver the finished artifact and clearly state any limitations or missing source information.

### Visual quality requirements

* Clean and professional layout.
* Consistent typography and heading hierarchy.
* Comfortable margins and spacing.
* Readable font sizes.
* Proper alignment of text, tables, diagrams, and images.
* No overlapping text, clipping, broken glyphs, or crowded sections.
* Tables should fit their columns and pages.
* Page breaks should feel intentional.
* Diagrams should be legible and technically correct.
* Avoid excessive decoration and generic AI-looking layouts.
* Review the rendered output, not just the source document.

The user has previously requested that references be removed from a particular report. Treat this as report-specific, not a universal rule. Follow the current instructions for each deliverable.

## 9\. Master Prompts for Other LLMs

The user often asks for a strong, copyable master prompt to provide to Claude or another LLM to create or refine a PDF using a supplied template.

An effective prompt should tell the other LLM to:

* Read the entire source PDF and template before making changes.
* Inspect all supplied images and the week's topic.
* Understand Binary VisionX and preserve its actual architecture.
* Distinguish confirmed facts from proposals and unknowns.
* Create the requested artifact rather than merely saying it has reviewed the files.
* Use the supplied template as a real layout reference.
* Rewrite generic or AI-sounding prose into natural, specific project documentation.
* Avoid unsupported claims, fabricated results, and unverified implementation details.
* Prevent overlap, clipping, inconsistent spacing, and crowded layouts.
* Validate the final rendered PDF page by page.
* Make a second refinement pass after visual inspection.
* Provide the completed file and a concise summary of what was changed.
* Explicitly report any limitation that prevents completion instead of pretending the work is finished.

The user values prompts that are actionable, precise, detailed, and difficult to misinterpret.

## 10\. Speeches, Pitches, and Presentations

The user often requests content for:

* Project introductions and pitches.
* Feasibility speeches.
* Architecture explanations.
* Technical-stack explanations.
* Step-by-step workflow descriptions.
* Evaluator or invigilator presentations.
* Explanations aligned with an architecture diagram.
* Easy-to-memorize speaking scripts.

### Preferred approach

* Use a natural spoken style rather than formal report prose.
* Explain the system in a logical sequence: **input → local processing → decision/prioritization → cloud support where appropriate → audio output**.
* Explain what each component does and why it is needed.
* Highlight the edge–cloud split and offline safety behavior.
* Explain language interaction in plain language while preserving technical accuracy.
* Keep the script easy to say aloud and remember.
* Avoid unexplained jargon and exaggerated claims.
* Tailor content to the audience and time limit.
* If SIH alignment is requested, connect the solution to the specified problem statement without falsely claiming official approval or compliance.

When asked “which decision engine are we using?” or “which ML model are we using?”, first verify the current project information. If the choice has not been confirmed, explain the proposed design and identify what remains undecided rather than inventing a finalized answer.

## 11\. Collaboration and Response Preferences

The user communicates directly and iteratively. Common requests include “analyze deeply,” “give me the better prompt,” “refine this PDF,” “professional,” “human-created,” “tech stack,” “step by step,” and “easy to memorize.”

The user's feedback indicates the following priorities:

1. **Substance before polish:** Understand the actual project and task before producing a major deliverable.
2. **Specificity:** Avoid generic content that could describe any AI project.
3. **Natural writing:** Avoid repetitive, overly polished, formulaic language that reads as AI-generated.
4. **Technical consistency:** Preserve the edge–cloud architecture, local safety principle, language priorities, and wearable power constraints.
5. **Execution:** When asked to create or revise an artifact, complete the work if the required tools and files are available. Do not stop at an acknowledgement or an outline.
6. **Meaningful revision:** When the user says the result is poor, identify the concrete problems and fix them rather than making superficial changes.
7. **Visual verification:** Inspect the final PDF or other artifact for layout defects.
8. **Honest status:** Never claim that a file was created, edited, tested, or benchmarked unless that actually happened.
9. **Template fidelity:** Follow supplied templates and source material closely.
10. **Context-specific exclusions:** Respect requests to omit SIH content, references, or other sections for the particular deliverable where requested.
11. **Clear delivery:** Provide a usable download link when a file is created in the active environment.
12. **Audience awareness:** Technical documentation can be detailed; presentation scripts should be concise, natural, and easy to deliver.

## 12\. Recommended End-to-End Workflow

For any substantial Binary VisionX task:

1. **Identify the deliverable:** Report, PDF revision, project Markdown, speech, pitch, architecture explanation, or master prompt.
2. **Review available evidence:** Read the supplied files, images, template, and project notes.
3. **Establish the scope:** Identify the exact topic, audience, page count, language, and any required exclusions.
4. **Separate facts from assumptions:** Mark implemented features, planned features, illustrative estimates, and open decisions distinctly.
5. **Design the content:** Create a logical outline and ensure the material addresses the request.
6. **Produce the artifact:** Use the appropriate document or file-generation workflow.
7. **Review for technical accuracy:** Check architecture, terminology, hardware constraints, and unsupported claims.
8. **Review for presentation quality:** Check layout, readability, consistency, and naturalness.
9. **Validate the output:** Open or render the final artifact and inspect it.
10. **Deliver clearly:** Link the finished file and mention any material limitations or remaining decisions.

## 13\. Context and Identity Cautions

* Shared project conversation records contain the names Sakthi Prakash, Sanjay, and Sri Ganth in different entries. Their relationship and individual roles are unclear. Do not assume they are the same person or assign formal responsibilities without confirmation.
* No confirmed formal team structure, job title, client, or career objective is established by this context.
* English is the primary language of the project conversations. Tamil is an important product language requirement; Hindi has been considered as a possible additional product language. Do not infer the user's personal language preferences beyond what they state in the current task.
* The project memory context indicated memory was disabled. Do not claim that this file has been saved to persistent account memory.

\---

## Quick Reference

**Project:** Binary VisionX  
**Purpose:** Wearable AI-assisted environmental awareness for people with visual impairments  
**Architecture:** Hybrid edge–cloud  
**Edge principle:** Immediate safety alerts remain local and available offline  
**Edge hardware discussed:** Raspberry Pi 4/5, 8 GB RAM (not finalized)  
**Cloud direction discussed:** VPS; Oracle Cloud was only an example  
**Languages prioritized:** Tamil and English; Hindi may be considered later  
**Power direction:** Wearable rechargeable battery unit; final chemistry/specifications unconfirmed  
**Possible software:** Python, OpenCV, NumPy, YOLO-family detector, ASR, TTS, NLP (all subject to confirmation)  
**Documentation standard:** Natural, specific, professional, template-faithful, technically accurate, visually inspected  
**Never fabricate:** Implemented features, model versions, measured latency, accuracy, battery runtime, test results, or finalized hardware choices

