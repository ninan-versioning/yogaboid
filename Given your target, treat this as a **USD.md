Given your target, treat this as a **USD-native virtual production system**, not a web-animation project. Use Omniverse Kit as the application and rendering runtime, OpenUSD as the source of truth, and motion models as services that author animation into USD layers; stream the rendered viewport to web/mobile instead of rendering the avatar client-side. NVIDIA’s Kit templates explicitly support streaming-ready apps, container packaging, USD Composer for scene authoring, and USD Viewer for viewport-focused web streaming. [github](https://github.com/NVIDIA-Omniverse/kit-app-template/blob/main/templates/apps/usd_composer/README.md)
## Recommended Architecture
```text
Yoga motion corpus
      │
      ├── motion reconstruction / mocap cleanup
      ↓
Canonical SMPL-X motion representation
      │
      ├── MotionBricks / GPC generation
      ├── ProtoMotions physical validation
      └── IK and contact correction
      ↓
Rig retargeting + facial performance
      ↓
UsdSkel animation clips
      ↓
USD composition
  avatar.usd
  wardrobe.usd
  studio.usd
  props.usd
  lighting.usd
  sequence.usd
      ↓
Custom Omniverse Kit application
      ↓
RTX render streamed to browser/mobile
```

OpenUSD is designed for scalable authoring, reading, composition, and streaming of time-sampled scene descriptions, making it the appropriate persistence and interchange layer—not just an export format. [github](https://github.com/PixarAnimationStudios/OpenUSD)
## Repositories to Clone
### Essential USD stack
Have the agent clone and study these first:

- **PixarAnimationStudios/OpenUSD** — schemas, composition arcs, variants, payloads, clips, `UsdSkel`, `UsdShade`, `UsdLux`, asset resolution and validation. [github](https://github.com/PixarAnimationStudios/OpenUSD)
- **NVIDIA-Omniverse/kit-app-template** — build your actual yoga-teacher application from this, preferably beginning with USD Composer during development and a stripped-down USD Viewer application for deployment. [github](https://github.com/NVIDIA-Omniverse/kit-app-template/blob/main/templates/apps/usd_composer/README.md)
- **NVIDIA-Omniverse/kit-automation-sample** — useful for exposing remote APIs that load poses, select lessons, change cameras, trigger renders and run regression tests. [github](https://github.com/NVIDIA-Omniverse/kit-automation-sample)
- **NVIDIA-Omniverse GitHub organization samples** — examples of Kit extensions and Omniverse integration patterns. [github](https://github.com/orgs/NVIDIA-Omniverse/repositories)

Ask the agent to implement these USD capabilities early:

- Non-destructive layer stack for avatar, animation, lesson, environment, lighting and user-specific overrides
- `UsdSkel` skeleton, skinning and animation clips
- Asset variants for body, wardrobe, mat, studio and lighting
- Payload-based scene loading
- Value clips for long lessons
- `UsdShade` MaterialX/MDL material bindings
- `UsdLux` lighting rigs
- A consistent stage contract: meters, Y-up or Z-up, frame rate, naming and skeleton conventions
### Motion intelligence
Keep the four projects you already identified, then add:

- **MaskedMimic**, through ProtoMotions — likely the most useful controller for reconstructing missing pose targets, combining sparse constraints and controlling selected body parts.
- **PhysDiff** — adds physics-based projection during diffusion sampling to improve physical plausibility. This matters for foot plants, hand-to-floor support and balance in yoga. [jankautz](https://jankautz.com/publications/PhysDiff_ICCV23.pdf)
- **PDP: Physics-Based Character Animation via Diffusion Policy** — useful research for robust corrective behavior; it trains task experts, gathers noisy-state/clean-action pairs and distills them into a diffusion policy. [tml.stanford](https://tml.stanford.edu/PDP.github.io/)
- **OmniControl** — study for specifying arbitrary joint constraints over time, such as “keep left hand here while rotating the pelvis.”
- **InterControl** — relevant later for teacher–student spatial interaction or assisted poses.
- **HOI-Diff / CG-HOI** — useful when poses interact with mats, blocks, straps, walls or chairs.
- **MDM / MotionDiffuse / MotionGPT-style models** — useful baselines for text-to-motion, but not sufficient as the final yoga controller.
- **Motion Tools survey repository** — a useful index covering controllable diffusion, physical animation, interaction generation and motion editing. [github](https://github.com/Frank-ZY-Dou/Motion_Tools)

Do **not** ask the agent to integrate every generator. Make MotionBricks/GPC the primary generation experiments, ProtoMotions the physical-validation path, and choose one diffusion baseline for comparison.
### Body reconstruction
For converting instructor footage into canonical motion:

- **SMPLer-X** — expressive SMPL-X body, hand and face estimation from images. [github](https://github.com/MotrixLab/SMPLer-X)
- **WHAM** — world-grounded human-motion reconstruction
- **4DHumans** — monocular human tracking and SMPL recovery
- **SLAHMR** — multi-person, world-grounded motion reconstruction
- **GVHMR** — global human-motion recovery
- **MMPose** — 2D/3D pose-estimation baseline and annotation tooling
- **EasyMocap** — calibrated multi-camera capture and SMPL-family fitting

For production-quality yoga, monocular reconstruction should be treated as **initialization**, not ground truth. Record synchronized multi-view footage where possible, then fit SMPL-X and manually validate spine, shoulder, hip, knee, ankle, wrist and hand alignment.
### Facial performance
Clone and evaluate:

- **NVIDIA/ACE** — reference applications and services for digital humans. [github](https://github.com/NVIDIA/ACE)
- **NVIDIA/Audio2Face-3D**
- **NVIDIA/Audio2Face-3D-Samples** — converts audio and optional emotion input into ARKit-style facial blendshapes for a rendering engine. [github](https://github.com/NVIDIA/Audio2Face-3D-Samples)
- **NVIDIA/Audio2Face-3D-Training-Framework** — useful if the default performance does not match the teacher’s voice, face or desired meditative delivery. [github](https://github.com/NVIDIA/Audio2Face-3D)
- **NVIDIA/Maya-ACE** — Maya reference client with gRPC libraries, test assets and a sample scene. [github](https://github.com/NVIDIA/Maya-ACE)

Audio2Face handles speaking performance, but you should separately author gaze, blinks, breathing, subtle head motion and idle posture. A serene yoga teacher will look artificial if lip sync works but the eyes, chest and posture remain static.
## Papers by Priority
### Read first
1. **GPC** — transferable motor-control pretraining and efficient adaptation.
2. **MaskedMimic** — sparse, multimodal and partial-body motion control.
3. **MotionBricks** — composable latent motion primitives and low-latency synthesis.
4. **SONIC** — scalable motion tracking, although its robot-control assumptions are not directly your deployment target.
5. **PhysDiff** — physics correction applied to generative motion. [jankautz](https://jankautz.com/publications/PhysDiff_ICCV23.pdf)
6. **OmniControl** — space-time joint constraints.
7. **WHAM / GVHMR / SLAHMR** — reconstructing stable world-space instructor motion.
8. **SMPL-X and SUPR** — understand body-model limitations before choosing the canonical representation.
### Read second
- **PDP** for diffusion-based corrective control [tml.stanford](https://tml.stanford.edu/PDP.github.io/)
- **InterControl** for teacher–student interaction
- **COUCH** for scene/contact-aware sitting and support
- **HUMANISE** for language-conditioned motion in 3D scenes
- **InterDiff / HOI-Diff / CG-HOI** for human-object interaction
- **TEACH** for composing sequences of actions from language
- **PriorMDM / DoubleTake** for long-duration motion composition
- **VPoser / Pose-NDF** for pose priors and plausibility
- **ContactFormer** for contact reasoning
## Agent Skills to Create
Rather than one giant “avatar” skill, give your coding agent narrowly scoped skills with explicit input/output contracts.

| Skill | Responsibility | Output |
|---|---|---|
| `openusd-scene-architect` | Composition arcs, references, payloads, variants and layer policy | USD stage templates |
| `usdskel-authoring` | Skeletons, skinning, blendshapes and animation clips | Valid `UsdSkel` assets |
| `omniverse-kit-extension` | Kit extensions, commands, events and UI | Installable Kit extension |
| `kit-streaming-deployment` | Containerization, GPU runtime and client connection | Streaming-ready package |
| `smplx-motion-pipeline` | Coordinate normalization, joint mapping and motion conversion | Canonical SMPL-X sequences |
| `avatar-retargeting` | SMPL-X-to-production-rig transfer | Retargeted animation |
| `yoga-contact-analysis` | Detect feet, hands, knees, elbows and torso support | Contact tracks |
| `yoga-alignment-validator` | Joint-angle, symmetry and alignment rules | Per-frame correction metrics |
| `motion-quality-qc` | Sliding, penetration, jitter, acceleration and discontinuity tests | QC report plus corrected clip |
| `motion-physics-projection` | IK or physics correction after generation | Stable corrected motion |
| `materialx-lookdev` | Skin, eyes, hair, cloth and mat materials | MaterialX/USDShade library |
| `rtx-lighting` | Studio, daylight and meditative lighting setups | `UsdLux` lighting layers |
| `audio2face-integration` | Audio/emotion to blendshapes | Facial animation layer |
| `digital-human-behavior` | Gaze, blink, breathing, listening and turn-taking | Behavioral animation tracks |
| `usd-asset-validation` | Stage units, paths, missing assets, schemas and bounds | CI validation report |
| `usd-render-regression` | Fixed-camera reference renders and image diffs | CI render artifacts |
## Recommended Platform Split
| Requirement | Platform |
|---|---|
| USD scene composition and look development | **USD Composer / custom Kit app** |
| High-fidelity RTX rendering | **Omniverse Kit renderer** |
| Motion-model training | **PyTorch outside Omniverse** |
| Physics-based controller training | **Isaac Lab only when needed** |
| Motion verification and contacts | **Isaac Sim or a lighter physics stage** |
| Production animation cleanup | **Maya and/or Blender with USD exchange** |
| Facial animation | **ACE / Audio2Face** |
| Web/mobile delivery | **Kit application streaming** |

Isaac Lab is therefore an **optional motion R&D dependency**, not your main product platform. Use it only when you need learned balance, recovery, physically simulated contacts or ProtoMotions compatibility; use a custom Omniverse Kit app for the product itself.
## USD Asset Structure
Ask your agent to establish this convention before importing significant content:

```text
/assets
  /characters/yoga_teacher
    model.usd
    rig.usd
    materials.usd
    wardrobe.usd
    character.usd
  /environments/studio_01
    architecture.usd
    materials.usd
    lighting.usd
    environment.usd
  /props
    mat.usd
    block.usd
    strap.usd

/motions
  /canonical_smplx
  /retargeted
  /corrected
  /facial

/lessons
  /sun_salutation
    choreography.usd
    dialogue.usd
    cameras.usd
    lesson.usd

/shots
  shot_001.usda
  shot_002.usda

/app
  /extensions
  /source
  /tests
```

Each lesson should compose references to stable avatar/environment assets and contribute only animation, dialogue, camera and lesson-specific overrides. That prevents destructive edits and lets you swap teacher appearance, wardrobe, studio or lighting through USD variants.

## We might need but not limited to

>  OpenUSD, NVIDIA Omniverse Kit App Template, Kit Automation Sample, ProtoMotions, GR00T WholeBodyControl with MotionBricks, and the official GPC code when available. Clone SMPLer-X, WHAM, GVHMR, 4DHumans, MMPose, Audio2Face-3D Samples and Audio2Face-3D Training Framework. Index the GPC, MaskedMimic, MotionBricks, SONIC, PhysDiff, OmniControl, PDP, WHAM, GVHMR and SMPL-X papers. Do not build a Three.js/WebGL renderer. Build a USD-native asset pipeline and custom Omniverse Kit application supporting `UsdSkel`, MaterialX/MDL, `UsdLux`, payloads, variants, value clips, RTX rendering and application streaming. Treat Isaac Lab as an optional physics-training environment, not the product runtime. Produce an architecture decision record, repository compatibility matrix, pinned environments, minimal end-to-end USD avatar test, and automated USD/render validation.

One caution: ACE and Audio2Face services may involve NVIDIA AI Enterprise evaluation or product licensing, while the sample repositories and service availability have separate terms; have the agent produce a license matrix before adopting them commercially. [github](https://github.com/NVIDIA/ACE)

