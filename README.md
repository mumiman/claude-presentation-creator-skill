presentation-creator
A Claude Skill for creating polished, mode-aware PowerPoint presentations with a clear narrative arc and consistent visual design — including source-grounded "NotebookLM-style" decks built from your own research papers, articles, or notes.

This skill plans content and design. It hands off to the base pptx skill (bundled with Claude) for the actual .pptx file generation.


Why this exists
Asking Claude to "make me a slide deck" usually produces something serviceable but generic — eight slides of bullet points in a default theme, regardless of whether you're pitching investors or defending a thesis. The structure, tone, and visual treatment for those two situations should be very different.
presentation-creator adds that missing layer:

Mode awareness. Pick the audience and purpose (or let the skill infer it), and the deck follows the appropriate narrative arc — Sequoia-style for pitches, Background → Methods → Findings for academic talks, BLUF for executive briefings, and so on.
Source grounding. Drop in a PDF, paper, transcript, or notes file. Every fact on every slide traces back to the source. No invented statistics, no hallucinated citations.
Visual restraint. Three richness levels (Rich / Moderate / Minimal) and a small set of consistency rules that keep the deck from looking like AI slop.

It's the layer that turns "make a deck" into something you'd actually use.

What it does
When triggered, the skill walks Claude through a six-step workflow:

Capture intent — topic, source material, mode, visual richness, slide count, speaker notes (off by default), brand constraints, author/affiliation.
Process source material — read everything thoroughly, build a fact inventory, find the natural narrative inside the sources.
Draft an outline — slide-by-slide titles + one-line descriptions, with the right narrative arc for the chosen mode.
Write slide content — titles, bodies, visual briefs (and speaker notes if you asked for them).
Hand off to pptx — generate the real .pptx file with a topic-informed palette, font pairing, and visual treatment scaled to the richness level.
Present — give you the file.


Modes
ModeForDefault countDefault richnessInformativeGeneral audiences, public talks, explainers10–15ModerateAcademicResearchers, conferences, lectures, thesis defense12–20Moderate (data-heavy)PitchInvestors, buyers, decision-makers10–12RichExecutive BriefingLeadership, decision-makers, board updates5–8ModerateTrainingNew hires, workshop attendees, learners15–25ModerateCreative / MarketingCampaign launches, brand reveals, press8–12RichCustomAnything elseYour callYour call
Each mode has a defined narrative arc, audience profile, tone, and content style. See presentation-creator/SKILL.md for the full definitions.

Visual richness

Rich — image placeholders, diagrams, charts, decorative elements. For pitches and creative work where you want to wow.
Moderate (default) — charts for data, diagrams only when genuinely clarifying, otherwise clean typography. For most professional decks.
Minimal — typography-led, generous whitespace, two colors at most. For editorial or premium-feeling decks.


Source grounding (NotebookLM-style)
When you provide source material, the skill follows strict rules to prevent hallucination:

Build the deck from sources outward, not topic inward.
Every concrete claim (numbers, dates, names, quotes) traces to the source.
Three options for missing information, in order of preference: restructure to avoid needing it → ask the user → mark [needs source] and leave it for the user to fill in.
Charts whose shape is reconstructed from a published figure (rather than plotted from raw data) carry an "Illustrative — reconstructed from Fig. X" caption.
A Sources slide is included for any deck drawing on external material.


Installation
Claude.ai (web, desktop, mobile)

Download the latest presentation-creator.skill file from Releases.
In Claude.ai, go to Settings → Capabilities → Skills.
Click Upload skill and select the .skill file.
The skill is now available in any conversation.

Claude Code
bash# Clone or download this repo
git clone https://github.com/YOUR-USERNAME/presentation-creator.git

# Copy the skill folder into your Claude Code skills directory
cp -r presentation-creator/presentation-creator ~/.claude/skills/
Restart your Claude Code session and the skill is ready.
Anthropic API (programmatic use)
The presentation-creator/ folder in this repo is a self-contained skill. Mount or include it wherever your application loads skills from. The skill has no scripts or external dependencies of its own — it relies on the base pptx skill, which ships with Claude.

Examples
Thesis defense from a published paper

"Please create my thesis defense presentation slide deck, based on my published paper. No fake data, no hallucination. Use only data from this paper. Make it look interesting."

Attach your paper. The skill reads it, picks Academic mode, structures a 4-part defense (Background → Methods → Findings → Discussion), and produces a 20-slide deck with section dividers, a U-shaped burden bar chart, COVID comparison cards, pathogen spotlight slides, an interrupted-time-series visual for the vaccine impact, an adjusted-odds-ratio table, and a "Thank you" closer with acknowledgements. Every number traces to a table or section in your paper.
Pitch deck for a fictional startup

"Make me a 10-slide pitch deck for a startup that delivers fresh meals to office buildings using autonomous lockers. We're raising a seed round."

Pitch mode, Rich richness. Hook → Problem → Solution → Why Now → Market → Product → Traction → Business Model → Team → Ask. Image briefs throughout, big typography, one thought per slide.
Training deck on a new tool

"I need a training deck for our customer success team on the new Linear workflow. ~20 slides, with practice prompts."

Training mode. Welcome → Objectives → Concept chunks (introduce → illustrate → check understanding) → Walkthrough → Practice prompt → Recap → Resources.

What's in this repo
presentation-creator/
├── presentation-creator/      ← the actual skill
│   └── SKILL.md
├── README.md                  ← you are here
└── LICENSE
The skill is intentionally simple — a single SKILL.md file with no scripts or reference modules. All the heavy lifting (palette logic, layout patterns, file generation, QA conversion) is delegated to the base pptx skill that ships with Claude.

Tips for getting the best results

Provide source material when you have it. The skill is much stronger with grounded inputs than with general knowledge — and it'll tell you upfront when a request is risky without sources.
Be specific about your audience. "For my dissertation committee" is more useful than "for school." It changes the mode, the tone, and the slide count.
Speaker notes are off by default. Ask for them explicitly if you want them.
For research papers, the skill will pull authors and affiliations from the source automatically. You don't need to repeat them.
Don't haggle over slide counts. Each mode has a sensible default range. Trust it unless the deck has a hard time constraint.


Limitations

The skill produces image placeholders with descriptive briefs, not generated images. If you want real images, you'll need to add them yourself or use a session with image-generation tools available.
For research papers, charts are reconstructed from the figure shape when raw data isn't available — these are clearly labeled as illustrative.
Heavy custom branding (specific font files, exact corporate palettes) is best applied as a post-step in PowerPoint after the deck is generated.


Acknowledgments

Built on top of the base pptx skill that ships with Claude.
Modeled in part on the experience of using NotebookLM's slide deck feature, which set a high bar for source-grounded presentation generation.
Developed iteratively using Anthropic's skill-creator skill, including a thesis-defense test scenario built from a real Lancet Regional Health — Southeast Asia paper.


Contributing
Issues and pull requests welcome. If you find a mode that's missing, a narrative arc that doesn't quite fit your use case, or a hallucination case the source-grounding rules didn't catch, please open an issue with a concrete example.
