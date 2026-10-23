# Personal AI Stylist

**AI product concept · Conversational UX · Working local prototype**

A conversational wardrobe assistant designed to help people explore their style and assemble outfits from clothes they already own. The project focuses on a practical product question: how can an AI assistant provide a useful first result while keeping the person in control of what is inferred, added and saved?

This case study presents the product design, interaction logic and technical boundaries. It is not a launch announcement or a fully validated consumer MVP. The complete commercial prompts, private project materials and personal wardrobe data are not distributed in this showcase.

### The problem

Style advice can become a long questionnaire, rely on generic aesthetic labels, or recommend items the person does not own. An assistant may also mistake “I like this colour” for approval of the entire outfit, or claim to save something without a real storage mechanism.

The proposed experience provides concrete outfit ideas early, asks short questions as needed, and separates visual inspiration from the person's confirmed wardrobe.

### Designed experience

1. **Discover a direction.** Start with the person's goal and a small set of visual directions when the host supports images. Distinguish “I enjoy looking at this” from “I want to wear this”. Avoid repeating questions whose answers are already known.
2. **Confirm wardrobe items.** Interpret one item from an image or description, show a draft description, and let the person correct it before adding it to the working wardrobe. Image interpretation depends on the host model.
3. **Build an outfit.** Use confirmed items first. Label missing items as ideas. Once a direction is chosen, refine one outfit rather than continually expanding the selection.
4. **Visualise honestly.** An AI illustration communicates an idea; a collage assembled from original photos shows the supplied items. These are different outputs with different requirements.
5. **Save with permission.** Approval of an outfit and permission to save it are separate. An explicit “This works; save this outfit” can provide both. Partial approval with a requested change does not.

### Architecture

The design has two execution modes. The core interaction is intended for an ordinary AI chat. An optional Python runtime provides concrete local data and image operations when connected by a tool-capable host.

```text
Person
  ↓
AI chat + conversation instructions
  ├─ Host capabilities: image interpretation, search, image generation
  │  Available only when provided by the selected platform and mode
  ├─ Chat-only mode: working wardrobe and outfits in the current context
  └─ Optional explicit tool connection
       ↓
     Python JSON CLI
       ├─ SQLite: drafts, confirmed items, preferences, saved outfits
       ├─ Pillow: composition from supplied image files
       └─ ZIP: local export and restore, including supplied images
```

The runtime is not a vision model and does not itself provide web search, weather data or an autonomous connection to every chat platform. A text prompt does not install the runtime or create persistent memory.

### What exists today

- Conversation specifications, portable instruction candidates and reproducible evaluation scenarios.
- A working Python runtime with item confirmation, outfit approval and save operations, local storage, image composition, deletion, export and restore.
- Automated tests using temporary storage and synthetic fixtures, including release packaging and privacy regressions.
- An explicit release manifest that limits which reviewed files enter a package.

Technical test results establish the behaviour of these local components. They do not establish the quality of style advice, visual recognition or consistent behaviour across AI services.

### Illustrative interaction

The following is a newly authored synthetic example of the intended interaction, not a user transcript or a model test result. Its test wardrobe contains blue wide-leg jeans, an off-white T-shirt, a cream jacket and beige flats.

> **Person:** I don't have the blouse, but I have an off-white T-shirt.
>
> **Assistant:** Use the T-shirt with your blue jeans, cream jacket and beige flats. Tucking it in will make the jacket's shorter shape clearer. Would you keep the jacket, or try the same outfit without it?
>
> **Person:** Keep the jacket. This outfit works; save it as “Light day” in this conversation.
>
> **Assistant:** Added “Light day” to this conversation's collection: jeans, T-shirt, jacket and flats.

The example does not claim permanent storage, a generated image, or ownership of any unlisted accessory. In local-runtime mode, a successful storage operation would be required before reporting a persistent save.

### Evaluation and current limitations

An initial real-platform test of the Lite instruction candidate in Alice AI surfaced a specific issue: the assistant introduced extra accessories beyond the constrained wardrobe scenario. That run did not validate the complete product or compatibility of both editions.

The GigaChat attempt was blocked at the login step, so model behaviour there was not tested. User acceptance testing is currently paused. Internal same-context self-tests are recorded separately and are not independent target-platform validation.

Other important boundaries remain:

- Photo interpretation, web access and image generation depend on the host platform.
- Exact garment extraction from a photo of a person and virtual try-on are not established capabilities of the local runtime.
- Chat-only collections are limited by the available conversation context. Cross-service synchronisation is not implemented.
- Local storage is designed for a single user. The prototype does not provide a hosted multi-user service, account system or cloud synchronisation.
- Synthetic image tests do not demonstrate accurate recognition or styling of real personal photographs.

### Next steps

After the acceptance-testing pause is lifted: address the wardrobe-boundary issue, rerun the relevant scenario in a clean target session, and evaluate a complete flow with explicit evidence for each operation. Further work includes visual fidelity checks and longer conversations. A login-blocked service remains unverified until an authorised test can actually run.

### Skills demonstrated

Product scoping, conversation design, consent and state modelling, capability-aware AI interaction, evaluation design, Python prototyping, SQLite data modelling, image composition and release hygiene. The case study distinguishes intended UX, implemented components and observed behaviour so a reviewer can assess each on its own evidence.
