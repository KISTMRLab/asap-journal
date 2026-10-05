# Requirements and provenance

## Paper-supported facts

The full journal article was read before this document was written. It describes a two-phase ASAP pipeline: a Final Draft screenplay is parsed into Character, Dialogue, Parenthetical, and Action paragraphs; those paragraphs drive character selection, speech/co-speech gesture and gaze, emotion/facial expression, and physical action modules. The paper uses Sentence-BERT (`all-mpnet-base-v2` for gesture and `multi-qa-mpnet-base-dot-v1` for action), seven basic emotions, plausible subject/action/object/position combinations, movement toward a prop, and an interaction animation at a configured interaction point. It reports three outputs: captured 2D storyboards, recorded 3D/360 previz, and VR immersive previz.

The paper also says that the original code is unavailable, action/gesture sentences are available only on reasonable request, props/actions must be prepared, paragraphs are processed sequentially, simultaneous actions and automatic camera direction are unsupported, and output diversity is limited by the available character, prop, and motion libraries.

## Public implementation requirements

This repository must parse `.fdx` XML and a documented structured-text fallback; retain scene/action/character/dialogue/parenthetical records; resolve dialogue and actions against a user-authored motion catalog; emit an auditable sequential timeline with gaze, speech, gesture, emotion, movement, and object-interaction events; and render portable SVG/HTML storyboard, schematic previz, and immersive inspection views.

## Explicit assumptions and substitutions

This is an independent educational reimplementation, not the institute implementation. The standard-library TF-IDF resolver is a transparent offline baseline substituted for the unavailable trained gesture/action stack. The optional sentence-transformer adapter downloads nothing itself and uses the user's installed package/model cache. Durations, stage coordinates, lip markers, camera framing, SVG actors, and browser-based immersive view are engineering assumptions. They do not reproduce Unity, SALSA, Final IK, NavMesh, commercial TTS, motion capture, GestureCLR, 3D assets, 360 video, or VR hardware rendering. The exported `timeline.json` is an interoperability extension; the paper explicitly notes that the studied ASAP system lacks a timeline editor.


## Interactive implementation

The local demo compiles the actual parser/resolver output into a Three.js stage with bundled fictional CC0 avatars. It supports text/FDX upload, editable character/prop/action catalogs, timeline scrubbing, dialogue playback, PNG storyboard capture and timeline/storyboard export. Recompilation resets the stage. The lexical example requires no weights; semantic mode uses independently configurable local gesture/action encoders. The small official BEAT sample and fitted retrieval artifacts stay in ignored local outputs; pinned renderer modules are downloaded into ignored vendor storage. The journal's GestureCLR pose-matching lineage is documented as a research dependency; the browser uses locally prepared BEAT body-motion clips for dialogue while keeping authored action poses; it does not claim to reproduce institute motion capture or GestureCLR weights. Early ASAP variants expose a subset of this component implementation and do not claim the later journal evaluation. Optional Kokoro and faster-whisper adapters replace browser speech/typed input; mouth motion is an approximate envelope, not aligned visemes.

## Bundled fictional avatar substitution

Two newly generated fictional CC0 humanoids replace the original avatar assets in the browser demo. They provide a 53-bone rig and named ARKit/viseme targets. Motion retargeting adapts source joints to their bind pose; speaking envelopes approximate mouth motion rather than phoneme alignment. The optional recorded BEAT companion inspects public motion, face and audio files prepared locally, independently of the paper's learned algorithm. No dataset recordings or trained weights are bundled.

## Local recorded co-speech integration

The browser application retrieves prepared BEAT body-motion clips with `wild` mode: the journal paper's GestureCLR wild-pose matching lineage. The first `python scripts/start_demo.py` run fetches a small official BVH/TextGrid sample, constructs a nine-clip bank, and fits the local retrieval artifact under ignored `outputs/beat-library/`. Install `scripts/requirements-demo.txt` first. Preparation code and method dependencies are vendored in this repository; no sibling clone, original institute library, full dataset, or pretrained weights are bundled. The screenplay parser, action resolver, scene timeline, and storyboard capture remain this application's core. The separate recorded-motion companion remains available for local motion/face/audio inspection.
