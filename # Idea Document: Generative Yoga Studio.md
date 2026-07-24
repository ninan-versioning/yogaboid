# Idea Document: Generative Yoga Studio

**Working title:** Prana  
**Category:** Generative digital wellness, embodied AI, virtual production  
**Product:** A high-fidelity, personalized digital yoga teacher that generates complete lessons as composed OpenUSD experiences  
**Status:** Concept and research definition  
**Initial platform:** NVIDIA Omniverse Kit  
**Distribution:** Streamed interactive experience and rendered video  
**Explicitly out of scope:** Student pose detection, webcam correction, lightweight Three.js/WebGL avatars

***

## Vision

Build a digital yoga teacher capable of creating and performing a complete personalized course from a natural-language request.

The user might ask:

> “Create a 20-minute beginner Hatha session for evening relaxation. My hamstrings are tight, avoid wrist-loaded poses, teach in Hindi, and use a calm female instructor in a Himalayan sunrise studio.”

The system generates:

- A pedagogically coherent course
- Exact pose and transition timings
- Personalized pose modifications
- Physically plausible continuous movement
- Breath-synchronized instruction
- A persistent, photorealistic digital teacher
- Automatic instructional cinematography
- Facial performance and voice
- Lighting, environment and sound
- A complete layered OpenUSD lesson
- Real-time playback or cinematic offline rendering

The product is not a collection of prerecorded clips. It is a **generative virtual yoga studio**.

***

## Core Thesis

Existing yoga applications generally assemble recorded demonstrations. Generative motor-control systems can now learn broad movement distributions, adapt to designated skills and execute physically coherent behavior.

GPC learns a discrete motor representation through FSQ and end-to-end PPO, then trains an autoregressive transformer over those skill tokens. Its CoLA adaptation method adds less than 1% new parameters and supports SFT followed by reinforcement-learning fine-tuning for user-designated downstream behaviors. [developer.apple](https://developer.apple.com/documentation/healthkit/running-workout-sessions)

ProtoMotions provides the physics-based character-training substrate, including SMPL and SMPL-X humanoids, motion tracking, MaskedMimic, multiple simulation backends and mocap-processing pipelines. [developer.apple](https://developer.apple.com/documentation/healthkit/hkhealthstore/workoutsessionmirroringstarthandler)

The product opportunity is to combine:

```text
Generative course planning
+ validated yoga choreography
+ motor foundation models
+ physics-based animation
+ photorealistic digital humans
+ semantic camera direction
+ OpenUSD scene composition
```

***

## Product Promise

> Describe the yoga class you need, and a digital teacher creates and performs it for you.

The experience should feel like commissioning a private teacher and virtual production crew—not configuring a workout playlist.

***

## Target Audience

### Primary users

- People who want yoga sessions suited to their available time
- Beginners intimidated by fixed advanced demonstrations
- Practitioners with known mobility limitations
- Users seeking Hindi, English or Marathi instruction
- People who value premium, beautiful and calming digital experiences
- Practitioners bored by repetitive prerecorded classes

### Secondary users

- Yoga teachers generating original digital courses
- Wellness brands commissioning branded experiences
- Hotels, spas and retreat operators
- Connected fitness platforms
- Media companies producing wellness programming
- Researchers studying generative embodied instruction

***

## User Experience

### Input

The initial creation interface asks for:

- Duration
- Experience level
- Goal
- Yoga style
- Desired intensity
- Mobility constraints
- Poses or loads to avoid
- Available props
- Preferred teacher
- Language
- Voice and teaching style
- Environment
- Music and atmosphere
- Live session or rendered course

Example:

```json
{
  "durationMinutes": 20,
  "level": "beginner",
  "style": "hatha",
  "goals": ["evening_relaxation", "hamstring_mobility"],
  "constraints": ["tight_hamstrings", "avoid_wrist_loading"],
  "props": ["mat", "blocks"],
  "language": "hindi",
  "teacher": {
    "presentation": "female",
    "tone": "calm_precise",
    "instructionDensity": "moderate"
  },
  "environment": "himalayan_sunrise",
  "cinematography": "instructional_cinematic"
}
```

### Output

The user receives:

- Course title and intention
- Pose sequence
- Duration and intensity profile
- Personalized modifications
- Full avatar performance
- Spoken guidance
- Breath timing
- Automatic camera direction
- Complete visual environment
- Optional course manifest
- Replayable OpenUSD lesson

***

## Product Principles

### High fidelity is fundamental

The digital teacher must feel present, credible and intentional.

Requirements include:

- Realistic anatomy and deformation
- High-quality skin, eyes, hair and wardrobe
- Correct cloth and floor interaction
- Natural breathing and subtle idle motion
- Accurate hand and foot articulation
- Expressive but restrained facial performance
- Cinematic lighting
- Stable contact-aware movement
- No visible sliding, penetration or animation discontinuities

### Movement is generated, not disguised

The system should not conceal disconnected animation clips with camera cuts. Approved pose transitions must be performed as continuous, physics-checked motion.

### Cameras must teach

The camera is not decorative. Its job is to reveal the exact anatomical relationship discussed by the teacher.

### Personalization is explicit

The first product personalizes from user-provided requirements and preferences. It does not infer health status from a webcam.

### Yoga knowledge is curated

AI may realize and adapt approved movement, but it should not freely invent yoga pedagogy or unsafe anatomical configurations.

### OpenUSD is canonical

The course, teacher, environment, cameras, lighting and variants remain editable, composable and reproducible as USD assets.

***

## Experience Boundaries

### Included

- Prompt-to-course generation
- Exact-duration sequencing
- Approved yoga pose selection
- Personalized modifications
- Continuous pose transitions
- Generative teacher movement
- Breath synchronization
- Voice and facial animation
- Automatic instructional cameras
- Scene, lighting and wardrobe variants
- Interactive playback
- 4K offline rendering
- Course regeneration from manifests
- Multilingual instruction

### Excluded initially

- Webcam pose detection
- Student skeleton tracking
- Automatic form correction
- Medical diagnosis
- Injury treatment
- Unrestricted pose invention
- Physical robot deployment
- Client-side Three.js/WebGL rendering
- Mass template video generation

***

## Course Planning

### Validated pose ontology

Each supported asana is a structured entity:

```json
{
  "id": "trikonasana",
  "family": "standing",
  "level": ["beginner", "intermediate"],
  "goals": ["hamstring_mobility", "lateral_spine_length"],
  "requiredContacts": ["left_foot", "right_foot"],
  "jointDemand": {
    "hamstrings": 0.7,
    "hips": 0.6,
    "spine": 0.4
  },
  "contraindications": [],
  "modifications": [
    "hand_on_block",
    "reduced_stance",
    "reduced_depth"
  ],
  "minimumHoldSeconds": 20,
  "maximumHoldSeconds": 90,
  "instructionalTargets": [
    "foot_orientation",
    "hip_stack",
    "spinal_length"
  ]
}
```

### Transition graph

Pose sequencing uses a yoga-teacher-reviewed directed graph:

```json
{
  "from": "warrior_ii",
  "to": "triangle",
  "allowed": true,
  "level": "beginner",
  "requiredContacts": ["front_foot", "rear_foot"],
  "preserveFacing": true,
  "maximumTempo": 0.7,
  "motionConstraintProfile": "standing_lateral_transition_v1"
}
```

The graph determines whether a transition is pedagogically valid. The motor model determines how that approved edge is physically performed for the selected teacher, tempo and modification.

### Exact-duration solver

The planner allocates time across:

\[
T_{\text{course}} =
T_{\text{arrival}}
+ T_{\text{warmup}}
+ T_{\text{main}}
+ T_{\text{cooldown}}
+ T_{\text{transitions}}
+ T_{\text{rest}}
\]

For each pose:

\[
T_i =
T_{\text{entry}}
+ n_{\text{breaths}}T_{\text{breath}}
+ T_{\text{hold}}
+ T_{\text{exit}}
\]

The resulting course must equal the requested duration within a defined tolerance, initially one second.

***

## Motion Architecture

### Motion foundation layer

Use GPC to explore:

- Yoga-domain motor tokens
- Natural variation
- Perturbation recovery
- Parameter-efficient yoga adaptation
- Style-specific adapters
- Designated skill composition

GPC reports scaling its FSQ tracker to 600 hours of movement with a 99.98% success rate and describes emergent recovery behavior under external perturbations. [developer.apple](https://developer.apple.com/documentation/healthkit/running-workout-sessions)

### Physics layer

Use ProtoMotions for:

- SMPL/SMPL-X simulated characters
- Motion tracking
- MaskedMimic experiments
- Reinforcement-learning environments
- Contact and balance validation
- Mocap conversion
- Kinematic replay
- Evaluation and perturbation testing

ProtoMotions supports Isaac Gym, Isaac Lab and Genesis, and separates much of its simulator logic from tasks to make backend selection configurable. [developer.apple](https://developer.apple.com/documentation/healthkit/hkhealthstore/workoutsessionmirroringstarthandler)

### Interactive generation layer

Use MotionBricks for:

- Latent motion primitives
- Real-time transition composition
- Interactive motion controls
- Fast motion candidate generation
- Eventual integration with the runtime lesson director

### Initial policy

Do not retrain every layer before validating the experience.

1. Import curated yoga motion.
2. Train or reuse a strong motion tracker.
3. Build the approved transition graph.
4. Test MotionBricks/GPC generation inside approved edges.
5. Apply physics, contact and motion-quality validation.
6. Fine-tune only where zero-shot behavior is insufficient.

***

## Automatic Cinematography

### Objective

The camera should automatically move to the position that best communicates the current instruction.

Example:

```text
Voice cue: “Keep the front knee stacked above the ankle.”

Camera response:
- Move to low side three-quarter
- Frame hip, knee, ankle and foot
- Use a moderate focal length
- Hold until the alignment cue finishes
- Return wide before the next transition
```

### Semantic metadata

Each teaching cue includes:

- Target body region
- Alignment concept
- Preferred viewing direction
- Required framing
- Priority
- Minimum shot duration
- Permitted camera motion
- Occlusion constraints

```json
{
  "cue": "front_knee_over_ankle",
  "targetJoints": ["front_hip", "front_knee", "front_ankle"],
  "preferredView": "low_side_three_quarter",
  "framing": "medium_lower_body",
  "minimumDurationSeconds": 5,
  "allowCameraMotion": false,
  "priority": 0.95
}
```

### Shot grammar

| Instruction | Camera behavior |
|---|---|
| Complete pose | Full-body three-quarter |
| Spinal alignment | Side profile |
| Hip stacking | Front/rear three-quarter |
| Knee and ankle | Low side medium shot |
| Foot orientation | Low close-up |
| Hand placement | Prop-inclusive close-up |
| Transition | Wide tracking shot |
| Breath cue | Upper torso or restrained facial close-up |
| Bilateral comparison | Centered front or rear view |
| Balance pose | Stable full-body shot |

### Camera solver

Candidate cameras are scored for:

- Teaching-target visibility
- Full-body visibility where required
- Composition
- Continuity
- Occlusion
- Lens distortion
- Motion smoothness
- Screen-direction consistency
- Collision and clipping
- Background quality
- Voice/cue synchronization

The selected camera parameters are authored into a dedicated USD camera layer.

***

## OpenUSD Architecture

### Lesson composition

```text
lesson.usd
├── teacher reference
├── wardrobe reference
├── environment reference
├── props reference
├── choreography sublayer
├── facial-performance sublayer
├── dialogue sublayer
├── cameras sublayer
├── lighting sublayer
├── soundscape sublayer
└── lesson overrides
```

### Variants

- Teacher identity
- Body type
- Wardrobe
- Language
- Voice
- Course level
- Pose modification
- Environment
- Time of day
- Cinematography mode
- Music
- Render quality

### Example repository

```text
generative-yoga-studio/
├── apps/
│   ├── kit-runtime/
│   ├── course-generator/
│   └── review-console/
├── external/
│   ├── gpc/
│   ├── protomotions/
│   └── groot-wbc/
├── ontology/
│   ├── poses/
│   ├── transitions/
│   ├── constraints/
│   └── camera-grammar/
├── data/
│   ├── raw/
│   ├── canonical/
│   ├── annotations/
│   └── manifests/
├── models/
│   ├── course-planner/
│   ├── motion-controller/
│   ├── motionbricks/
│   └── adapters/
├── usd/
│   ├── teachers/
│   ├── wardrobe/
│   ├── environments/
│   ├── props/
│   ├── lessons/
│   └── cameras/
├── extensions/
│   ├── yoga.course_planner/
│   ├── yoga.motion_runtime/
│   ├── yoga.camera_director/
│   ├── yoga.scene_composer/
│   └── yoga.validation/
├── evaluation/
├── renders/
├── docs/
└── infra/
```

***

## Technology Stack

| Layer | Technology |
|---|---|
| Scene representation | OpenUSD |
| Application runtime | NVIDIA Omniverse Kit |
| Rendering | NVIDIA RTX |
| Digital character | Production humanoid rig with `UsdSkel` |
| Materials | MaterialX and/or MDL |
| Motion generation | GPC and MotionBricks |
| Physics animation | ProtoMotions |
| Simulation | Isaac Lab initially; MuJoCo/Genesis for selected tests |
| Facial animation | Audio2Face-3D / ACE |
| Voice | Multilingual neural TTS |
| Asset authoring | Blender and Maya |
| Course planner | Typed constraint solver with LLM interpretation |
| Configuration | Hydra and OmegaConf |
| ML | Python, PyTorch and PPO |
| Service layer | Python/FastAPI and TypeScript |
| Storage | S3-compatible object storage |
| Metadata | PostgreSQL |
| Experiment tracking | Weights & Biases |
| Infrastructure | AWS GPU instances |
| Project management | GitHub Issues, Projects and PRs |
| Delivery | Kit application streaming and offline video |

***

## Specialized Agents

### Course Planner Agent

- Converts requests into typed constraints
- Searches the approved pose graph
- Allocates exact durations
- Produces a deterministic course manifest

### Motion Composer Agent

- Chooses motion references or generated primitives
- Produces continuous movement for approved transitions
- Requests physics corrections when needed

### Yoga Validator Agent

- Checks sequence validity
- Checks constraint compliance
- Validates pose depth and modification selection
- Produces explainable rejection reports

### Motion Quality Agent

- Detects jitter, foot sliding and penetration
- Evaluates contact and balance
- Checks transition continuity
- Generates correction jobs

### USD Scene Agent

- Composes teacher, wardrobe, studio and props
- Authors lesson-specific layers
- Ensures reference and variant integrity

### Camera Director Agent

- Converts teaching cues into camera objectives
- Solves and scores candidate shots
- Writes the camera sequence

### Performance Agent

- Aligns speech, breath, facial expression and gaze
- Avoids exaggerated or constant facial movement
- Maintains a calm teacher presence

### Render Director Agent

- Selects quality profile
- Validates framing and exposure
- Executes previews and final output
- Runs visual-regression tests

***

## Validation Requirements

Every generated course must pass:

### Course validation

- Requested duration met
- Requested goal represented
- Exclusions respected
- Appropriate level and intensity
- Warm-up before demanding poses
- Cooldown and closure included
- Approved transition edges only

### Motion validation

- Required contacts maintained
- No significant foot or hand sliding
- No floor penetration
- Joint constraints respected
- Balance maintained
- Transition velocity within limits
- No abrupt acceleration discontinuities
- Breath movement remains subtle and plausible

### Camera validation

- Relevant joints visible during the cue
- Teacher not unintentionally cropped
- No obstacle or self-occlusion
- No clipping
- Lens distortion below threshold
- Stable horizon
- Camera motion does not compete with balance instruction
- Screen direction remains understandable

### Render validation

- Correct character and wardrobe variants
- Correct materials and textures
- No cloth explosion or deformation failure
- Acceptable skin and eye rendering
- No temporal flicker
- Voice, face and movement synchronized
- Expected scene dependencies resolve correctly

***

## MVP

### Product statement

> Generate a personalized 20-minute Surya Namaskar course, performed continuously by a photorealistic digital teacher with automatic instructional cinematography in an RTX-rendered USD environment.

### Content scope

- One photorealistic teacher
- One premium studio
- Twelve canonical Surya Namaskar states
- Fifteen to twenty-five transitions
- Beginner, standard and advanced profiles
- Wrist-safe modification
- Tight-hamstring modification
- English and Hindi
- Calm and precise teaching voices
- Eight to twelve camera templates
- Breath-driven timing
- Real-time playback
- 4K offline rendering
- Deterministic course manifest and USD output

### MVP success criteria

- User generates a course in under 60 seconds, excluding optional offline rendering.
- Course duration is within one second of the request.
- All transitions belong to the approved graph.
- Teacher moves continuously without cuts hiding discontinuities.
- Every major alignment cue receives an appropriate shot.
- Lesson can be regenerated from its manifest.
- A yoga professional approves at least 90% of generated sequences without structural edits.
- Reviewers rate motion quality and visual fidelity above a predefined benchmark.

***

## Development Phases

### Phase 0: Reproduction

- Reproduce ProtoMotions SMPL tracking.
- Reproduce MaskedMimic control.
- Run available GPC code/checkpoints when released.
- Run MotionBricks.
- Document skeleton, coordinate and motion-format differences.

### Phase 1: Yoga ontology

- Define the initial twelve poses.
- Define modifications.
- Define approved transition edges.
- Encode teaching cues.
- Encode semantic camera targets.
- Establish expert-review workflow.

### Phase 2: USD prototype

- Import one production teacher.
- Compose one studio.
- Author the layer and variant policy.
- Build course, camera and validation extensions.
- Render one manually authored reference lesson.

### Phase 3: Generative course

- Build requirement interpreter.
- Build pose-graph planner.
- Implement exact-duration solver.
- Produce deterministic lesson manifests.
- Generate narration and breath timelines.

### Phase 4: Generative movement

- Run curated yoga motion through ProtoMotions.
- Integrate MotionBricks/GPC experiments.
- Generate continuous motion inside approved edges.
- Implement physics and quality gates.

### Phase 5: Automatic direction

- Implement semantic shot grammar.
- Generate and score candidate cameras.
- Author USD camera sequences.
- Synchronize cameras, voice and movement.

### Phase 6: Productization

- Build the custom Kit runtime.
- Add session configuration and replay.
- Add streaming deployment.
- Add course saving and publishing.
- Launch the public yoga channel using generated lessons.

***

## Business Models

### Consumer subscription

Users pay for:

- Unlimited personalized classes
- Premium instructors and environments
- Longer courses
- Saved preferences
- Course history
- Higher render/stream quality

### Creator platform

Yoga teachers can:

- Define their pedagogy
- License voice and appearance
- Approve pose graphs
- Publish generated courses
- Receive revenue share

### Enterprise licensing

Potential customers:

- Wellness platforms
- Hotels and spas
- Healthcare-adjacent wellness programs
- Fitness hardware companies
- Media and streaming platforms
- Workplace wellness providers

### Media channel

Generated courses and public build episodes create:

- Audience acquisition
- Product demonstrations
- Sponsorship opportunities
- Teacher partnerships
- Feedback on desired courses
- A public record of technical progress

***

## Defensibility

The moat is not the conversational interface. It is the accumulated system of:

- Proprietary yoga motion data
- Approved transition graph
- Yoga-specific GPC adapters
- Pose modification ontology
- Motion reward functions
- Instructional camera grammar
- Motion and render evaluation data
- Persistent teacher identities
- High-quality USD asset library
- Course-generation feedback
- Expert approvals and corrections

The long-term asset is a **foundation model for embodied movement instruction**, beginning with yoga.

***

## Principal Risks

### Motion quality

Generated motion may remain less polished than handcrafted animation.

**Mitigation:** Use curated motion where necessary, generate only inside approved edges and apply deterministic cleanup and review.

### Anatomical credibility

Physics-valid movement may still be poor yoga.

**Mitigation:** Separate physical validity from yoga correctness and require both validation layers.

### Avatar uncanny valley

A nearly realistic teacher can feel worse than a stylized one.

**Mitigation:** Invest heavily in eyes, skin, breathing, cloth, hair, facial restraint and temporal stability.

### Camera distraction

Automatic cameras may become cinematic but pedagogically unhelpful.

**Mitigation:** Optimize for instructional visibility first and aesthetics second.

### Platform dependency

Omniverse, RTX and ACE create NVIDIA dependency.

**Mitigation:** Keep canonical assets and course manifests in OpenUSD and isolate vendor services behind interfaces.

### Computation cost

High-fidelity real-time streaming may be expensive.

**Mitigation:** Support both pre-rendered courses and streamed premium experiences; use H100s for training rather than routine rendering.

### Safety and claims

Personalized wellness content may be interpreted as medical guidance.

**Mitigation:** Avoid diagnosis and treatment claims, constrain inputs and require expert-reviewed sequencing.

***

## North-Star Demonstration

The defining demo should show this prompt:

> “Give me a 20-minute beginner evening class for tight hamstrings. Avoid wrist pressure. Teach slowly in Hindi with a calm female instructor. Use blocks and place the class in a Himalayan studio at sunrise.”

The viewer should see:

1. The generated course plan.
2. A photorealistic teacher enter the studio.
3. Continuous breath-aware movement.
4. Modified poses rather than generic demonstrations.
5. The camera move to reveal feet, hips and spine during corresponding cues.
6. Lighting evolve gently through the lesson.
7. The course conclude at exactly 20 minutes.
8. The complete lesson preserved as editable USD layers.

That single demonstration communicates the product: **personalized choreography, generative motor intelligence, instructional cinematography and high-fidelity digital embodiment.**

## Positioning

### One sentence

> A generative virtual yoga studio that creates and performs personalized classes through a photorealistic digital teacher.

### Technical

> Prompt-to-OpenUSD embodied instruction built on generative motor control, validated choreography and semantic virtual cinematography.

### Consumer

> Tell your teacher what you need today. Your class is created for you.

### Creator

> Define your teaching method once, then generate new premium classes without recording every variation.