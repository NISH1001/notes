# GitJot Notes

This is a template repository for [GitJot](https://github.com/CarsonDavis/note-taker), an Android app that captures voice-dictated notes and pushes them to a GitHub repo you own.

Fork this repo to get started — the app will push your notes here, and a Claude agent will organize them for you.

## How It Works

1. You speak a thought into GitJot on your phone
2. The app pushes it as a markdown file to `inbox/`
3. You run the inbox processor agent, which reads the raw notes, cleans up speech-to-text artifacts, sorts them into topic folders, and updates the indexes
4. Your organized notes live in `topics/` as plain markdown — browsable in the app, on GitHub, or with any text editor

## Repository Structure

```
inbox/              ← raw notes land here from the app
archive/            ← processed originals (audit trail)
topic-index.md      ← high-level summary of all categories
topics/
  books/
    index.md        ← catalog of book topics
    some-book/
      notes.md      ← organized notes (managed by agent)
      review.md     ← your own writing (never touched by agent)
  personal/
    index.md
  podcasts/
    index.md
```

New categories and topics are created automatically as the agent encounters them.

## File Ownership

- **`notes.md`, `quotes.md`, `index.md`** — managed by the agent. It creates, appends to, and rewrites these freely.
- **Everything else** in a topic folder (`review.md`, `essay.md`, etc.) — yours. The agent never reads, modifies, or deletes these.

## The Inbox Processor Agent

The agent lives at `.claude/agents/inbox-processor.md`. It runs with [Claude Code](https://docs.anthropic.com/en/docs/claude-code) and processes your inbox autonomously:

- Classifies each note by category (book, podcast, personal, etc.) and topic
- Cleans speech-to-text artifacts while preserving your voice and tone
- Appends to the right `notes.md` with appropriate formatting (paragraphs for commentary, bullets for keywords and quote references)
- Archives the original
- Updates all indexes
- Commits and pushes

Run it with:

```bash
claude code --agent inbox-processor "process the inbox"
```

## Setup

1. **Fork this repo** (or use it as a template) — name it whatever you want
2. **Install [GitJot](https://play.google.com/store/apps/details?id=com.carsondavis.notetaker)** on your Android device
3. **Create a fine-grained Personal Access Token** at [github.com/settings/personal-access-tokens/new](https://github.com/settings/personal-access-tokens/new):
   - Repository access: select only your forked notes repo
   - Permissions → Contents: Read and write
4. **Open GitJot**, enter your repo and token
5. **Start capturing notes** — they'll appear in `inbox/`
6. **Run the inbox processor** whenever you want your notes organized

## Processing Your Notes

The agent needs [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed on your machine. After capturing some notes:

```bash
cd your-notes-repo
claude code --agent inbox-processor "process the inbox"
```

The agent pulls, processes all inbox files, commits, and pushes. No interaction required.

## Topic Detection

- **Books**: Most notes are assumed to be about whatever book you're currently reading. Say "new book" or "I just started reading X" to signal a switch.
- **Podcasts, personal reflections, standalone ideas**: Routed to the appropriate category automatically.
- **Uncertain**: If the agent can't confidently classify a note, it sorts to its best guess and flags the entry.

## Working With Your Notes

The inbox processor handles the initial sorting, but the real value comes after — your notes repo is a workspace you can build on over time, either on your own or with Claude as a collaborator.

Once your raw notes are organized, you can create artifacts alongside them: a book review, an essay spinning off from a personal reflection, a structured argument inspired by a podcast. These files live right next to your `notes.md` in the same topic folder, and the agent will never touch them. They're yours.

You can write these entirely by hand, or you can open Claude Code in your repo and work on them together — ask it to help you draft a review from your reading notes, flesh out an idea you captured in fragments, or synthesize scattered thoughts across topics into something cohesive. Your raw notes become the source material, and the artifacts are whatever you make of them. Take it a step further by having Claude read all your past reviews to make a style guide that distills your voice and preferences into a reference doc, and then work together to draft reviews or essays.