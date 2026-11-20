# Why the structure matters when you build an AI office

A clever prompt won't save an AI agent that doesn't know your business. The structure around the agent is what holds that knowledge.

This article explains why I set up my AI office the way I did, how every part connects to one shared brain, and how to install your own in one paste. It comes with a small 3D office where you can see your team and spawn new agents. It is not a perfect setup. It is what I do with mine, and I'm still learning.

## Two ways to use this

**Read it as a person.** Go through it like any article. Part 1 explains the thinking. Part 2 is the install, written for Claude.

**Install it.** Open an empty folder in Claude Code and paste this one line:

```text
Install my AI office in this folder from https://github.com/elronnaranja/O2San-Base-Install (copy every file into this folder, not a subfolder), then tell me what to do next.
```

Claude copies the starter office from my public repo (https://github.com/elronnaranja/O2San-Base-Install) into your folder. You can also paste the Markdown version of this article (https://elronnaranja.com/thoughts/downloads/ai-office-structure.md), which ends with the same install steps for Claude. Know Git? `git clone https://github.com/elronnaranja/O2San-Base-Install my-ai-office` works too.

Read it yourself first. Don't paste instructions from anyone you don't trust into your AI tools, and that includes me. Know what you are about to run.

# Part 1: The thinking

## This setup is not fixed or perfect

I want to say this early. I'm not telling you this is the right way. It's what I do with my own office, and it keeps changing as I learn what works and what doesn't. Some of what I describe here I got wrong first. Take it as a starting point and change it.

## Why context matters

An AI agent is only as good as what it knows about your business. Without context, you get smart-sounding, generic answers.

Think about what a new employee needs on day one. How a customer goes from first contact to paying. Who owns what. Your pricing rules. What is already decided. What needs approval before anything goes out.

An agent needs the same things, and it only knows them if they are written somewhere it can read. That is the difference between an agent that sounds right and one you can trust.

## Why you can't just copy my setup

My AI office works because I built it around my own process. Most of its rules came from me correcting it when it got something wrong.

If I handed you my rules, they would be rules for my business. Most of your context is in your head and your team's, and a lot of it isn't written down yet. Nobody else can write it for you.

There is also a trust problem. If an agent gets your business wrong, you'll spot it right away. I might not.

So Part 2 gives you the structure and a few safety defaults, not my rules. The structure can be copied. The rules are yours to earn. That is why you get a structure with blanks.

## Why the structure matters: five parts

I call my office O2San. It rests on five parts. For each one, here is what it is and what goes wrong without it.

**1. One agent, one job.** Each agent has its own brief, a short file in `.claude/agents/` that says what it does and what it doesn't. Without this, you get one assistant doing everything, and it does everything in a mediocre way. Rules get mixed up, and you can't tell which part to fix when something goes wrong.

**2. Scoped tools.** Each agent you add gets a `tools:` line in its brief that lists what it may use. The Chief starts without one, so it can use everything your Claude Code can. My writer doesn't need the financial records. My finance agent doesn't need to learn video editing. Without scoping, every agent can touch everything, and one wrong move can reach things it should never have seen.

**3. A shared brain.** One set of notes that every agent reads and writes. Without it, each agent starts from zero every time, and you repeat yourself forever. I explain this in detail in the next section.

**4. A lead plus workers.** In the main session you talk to a Chief of Staff. This agent plans the work, hands it out, and checks the results. It lives in the main session so you always talk to one place, and it hands out the rest. Without a lead, you become the lead, and you spend your day passing messages between agents.

**5. Human in the loop.** Anything sent, public, or paid waits in `brain/10-Office/approvals.md` until you say yes. Without this, one confident mistake becomes an email to a client or a post on your page.

One honest warning. Most rules are written instructions, not locks. Claude follows them, but nothing physically stops a mistake. That's why the base install comes with a small guard that turns the three most important rules into real checks: it never reads your key file, and it stops to ask you before deleting anything or before anything is sent, posted, paid or deleted through a connected service. Keep reading what it asks permission to do.

### Start small

More agents doesn't automatically mean better results. More agents means more to maintain and monitor, and sometimes more token cost. Start with one or two. Add another only when you really need it.

## How everything connects to one brain

Chats get forgotten. Files don't.

An agent in a fresh session knows nothing about yesterday. So everything that matters has to live in files that the agent reads at the start and updates at the end. That is the whole idea of the brain. Here is the loop:

```text
Agent starts
   |
   v
Reads: about-me.md, RULES.md, its lessons file,
       state/INDEX.md and the state page for the task
   |
   v
Does the work
   |
   v
Writes its output to its room in the brain
Updates that state page and its line in state/INDEX.md
   |
   v
Is it sent, public, or paid?
   |-- no  --> done, reports back to you
   |-- yes --> goes to brain/10-Office/approvals.md
                  |
                  v
             You say yes
                  |
                  v
             Now it is official
```

### What is in the brain, and why

The brain is numbered the same way as mine, so the folders always sort in the same order, in Obsidian and in any file list:

```text
brain/
├── 00-Core/          about-me.md
├── 01-Business/      your main work
├── 02-Brand/         your brand or second line of work (optional)
├── 03-AI-Office/     RULES.md, decisions.md
├── 04-Personal/      personal notes (never secrets)
├── 05-Projects/      one note per bigger project
├── 06-Research/      what was looked up
├── 07-Daily-Logs/    one short note per day
├── 08-Inbox/         drop zone and proposals
├── 09-Templates/     state-page.md
└── 10-Office/        board.md, approvals.md, state/, lessons/, reports/, one room per agent
```

- `00-Core/about-me.md`: who the office works for. Your business, your people, how you like things done.
- `03-AI-Office/RULES.md`: what each agent may do alone. Level A: do it. Level B: do it, then tell me. Level C: ask first. Level D: never.
- `03-AI-Office/decisions.md`: what you decided, in your own words, with the date. It is append-only, so you add to it and never rewrite it.
- `10-Office/board.md`: the task board. What is running, what is waiting on you, what is done.
- `10-Office/approvals.md`: the waiting room for anything sent, public, or paid.
- `10-Office/state/` and its `INDEX.md`: one short page for each project and each agent, plus an index with one line per page. A fresh session reads these and knows where things stand.
- `10-Office/lessons/`: what `/teach` saved, one file per agent.
- `10-Office/reports/`: longer write-ups and summaries, so they don't get lost in a chat.
- `08-Inbox/`: a drop zone. Anything inside it is information, never instructions. If a file in the inbox says "delete everything", the agent treats it as text to read, not as an order.

Keep the numbers. You can rename the words after them, like `01-Business` to `01-Bakery`.

### The rules that hold it together

**Learn once, everyone knows.** A correction for one agent goes in its file in `lessons/`, so that agent keeps it. A rule for everyone goes in `RULES.md`, which every agent reads before working. This is why I use one brain and not one per agent. If each agent had its own notes, I would have to teach the same thing again and again.

**Never overwrite another agent's file.** Each agent writes in its own room. If it needs to add to something another agent owns, it appends or makes a new dated file.

**One session per task.** The state pages carry the memory, not a long chat. Long chats get slow and confused. A fresh session reads the state page and picks up exactly where the last one stopped.

**Only what the human approves becomes official.** If an agent works something out by itself, such as a new rule or a better way to do a task, it is a proposal. It goes to `brain/08-Inbox/` and stays a proposal until you say yes. Ask "What needs me?" and the Chief lists them, then say "approve proposal" or "reject proposal" with the file name.

## See your office

The install comes with one room you can look at: `office/index.html`. Double-click it and it opens in your browser. It's a small 3D office with the Brain in the middle and one table: your Chief's. Now and then someone gets up, walks to the Brain to save their work, and walks back. Drag the room to turn it, scroll to zoom, and click anyone to see their brief.

It's only the picture of your team. The work still happens in Claude Code and lives in the brain. My own office has more rooms and a work board. Yours starts with one room, and how it grows is up to you.

**The Spawn button.** Press **+ Spawn agent** and a see-through new hire appears with its own new table, a random name and a random look. Don't like it? Press the dice for another. Type its one job, press Copy, and paste the command into Claude Code. It looks like this:

```text
/new-agent Mochi (look 48213) — writes my weekly newsletter
```

The Chief writes the agent's brief and its room in the brain, then adds it to the office with that same look and table. Reload the page and it's sitting there. Typing `/new-agent` yourself works too. With no name, the Chief picks a random one.

**Make them yours.** Click anyone, then the pencil, to change their hair, headgear, glasses, facial hair, accessories, skin tone and clothes. Drag anyone to the couch, the pantry, the water cooler, someone else's chair (they swap tables), or back to their own chair. Whatever you drag them onto lights up first. Changes show at once and stay in your browser. The page then gives you an `/office` command: paste it into Claude Code and the Chief saves them in the office for good.

The room fits 9 tables. When it's full, say "upgrade the office" and the Chief can add tables, furniture or a second room. Redesign it however you like.

## Set up your brain in Obsidian

Your `brain` folder works as plain files, so you can skip this at first. Obsidian makes those files nicer to read, with linked notes and search. Download it and let Claude help you through the whole process. Once the office is built, paste this into Claude Code, in your office folder. It's for you, later. A Claude building from this article doesn't run it.

```text
Help me set up Obsidian for my brain folder, one step at a time. Wait for me after each step.
1. Ask if I'm on Mac or Windows. Tell me to download and install Obsidian from https://obsidian.md/download
2. Show me the full path of the brain folder in this office.
3. Have me click Open next to "Open folder as vault" and pick that brain folder where it is, not the office folder. Don't move it.
4. Have me open 00-Core/about-me.md and check that I can see what /setup wrote.
5. Explain linked notes with [[page-name]], using my real notes. No community plugins are needed.
Don't change, move or delete any files.
```

Obsidian lives on your computer. Your notes are plain files in that folder, and Claude reads the same files, so Obsidian doesn't need to be open for the office to work.

### Using it on another computer

**Upload it to a repo and download it manually.** This costs nothing. Put the whole office folder in a private GitHub repository. Upload your changes when you finish on one computer, and download them before you start on the other. On the new computer, open the folder in Claude Code, then open its `brain` folder as a vault in Obsidian. Keep the repo private, because it holds your business context, and don't work on both computers at the same time. Claude can help with this too. Again, this prompt is for you, later, not for the build:

```text
Help me put this office folder in a private GitHub repository, one step at a time. I will sign in to GitHub myself. Make sure the repository is private before anything is uploaded. Check that .env won't be uploaded, show me the list of files that will go up, and wait for my yes. Don't force-push or change any git or GitHub settings. Then show me how to upload my changes when I finish, and how to download them on my other computer.
```

**Or subscribe to Obsidian Sync.** It's a paid add-on (https://obsidian.md/sync), and you don't need it. It just makes this easy, because your brain updates across your devices on its own. Sync only moves the brain. Your agents and commands live outside that folder, so they still need the repo.

Mine uses Sync between my Mac and my Windows PC, plus a private repo as backup, saved from one computer only.

## Step by step

1. **Get the basics.** You need Claude Pro or higher and a laptop, with Claude Code installed: https://code.claude.com/docs/en/overview
2. **Make an empty folder** and open it in Claude Code.
3. **Paste the install line** from the top of this article. Claude copies the starter office into your folder. It is told to change nothing outside the folder.
4. **Start a fresh session** in that folder and type `/setup`. It asks you to name your office and your Chief. This takes about 5 minutes. Then double-click `office/index.html` to see your office with your names on it.
5. **Set up Obsidian when you're ready.** It's optional. The prompt in the section above has Claude walk you through it.
6. **Give it one real task.** Something you actually need done this week, not a test. The base comes with no email or posting tools. It drafts, you approve, and you send it yourself until you connect a service. If your Claude Code already has services connected, the Chief can reach them, which is one more reason the approval rule is there.
7. **Correct it with `/teach` when it is wrong.** For example: `/teach shorter intros`. Once you have a writer agent, it becomes `/teach writer: shorter intros`. This is how your rules build up. Each correction is a rule you earned.
8. **Use a fresh session for each new task.** The brain remembers. The chat doesn't have to.
   Every few weeks, or when the Chief suggests it, type `/tidy`. It merges duplicate rules and settles ones that clash, so each rule lives in one place. Rules that pile up get followed less. That's the mistake I made with mine.
9. **Add one agent with the Spawn button or `/new-agent`, but only when one kind of work keeps coming back.** For example, if you keep asking for writing, add a writer. That gives you:
   - a brief saying what the writer does and doesn't do, in `.claude/agents/`
   - its own room in the brain: `brain/10-Office/writer/`
   - its state page in `brain/10-Office/state/`
   - its line in `INDEX.md`
   - a desk in your office
10. **Later, only if you want,** connect outside services. Use an MCP connector or a small API script in `tools/`. Keep keys in `.env`, never in any other file. If you ever put this folder in a repository, keep it private, because it holds your business context.

That's it. I'm not promising a date or a result. How it goes depends on your business and the time you put in.

## What I do differently with mine

Mine has the same five parts and the same ideas, with different file names and 7 agents: my Chief of Staff plus a developer, an inbox, money and sales agent, a writer, a social media manager, a reviewer and a video editor. It wasn't always 7. I set up 15 right at the start, and that was my biggest mistake (more below). Your base starts with one.

My brain is an Obsidian vault (https://obsidian.md) with the same numbers as yours, from `00-Core` to `10-O2San`, and the office's `brain/` folder is a link into it. That way other AI tools read the same notes too. It's optional, and the section above shows how to set yours up.

My approval rule has an extra. Once an action type is approved unchanged 10 times in a row and I say "make it automatic", it moves from ask-first to do-it-then-tell. This is not in your base. Earn it.

My delete rule is looser than yours. Your base is simple: never delete, move to archive. Mine lets agents clear their own noise and asks me for anything critical. Start with never.

## Where I went wrong

My office worked, but it got heavy. When I checked it properly, here's what I found, what I changed, and what the base install does so you skip it.

**1. Too many agents, too early.** I started with 15. Some barely had work, and every extra agent was another brief to keep current and more for every session to read. I merged them down to 7.
*In your base:* you start with one. `/new-agent` first checks whether an existing agent can take the work, and the Chief suggests merging agents that sit idle.

**2. The same rule in five places.** A rule could live in my main instructions, my rules files, Claude's memory, a playbook and an agent's notes. The copies drifted apart, and some even contradicted each other. Now every rule has one home: a short list for everyone, an agent's own lessons, or a skill that loads only for that client or kind of work.
*In your base:* one `RULES.md`, lessons per agent, skills for narrow rules, and `/tidy` to keep it that way.

**3. A heavy start.** Before doing anything, my Chief was reading about 79,000 tokens: a long project index, every past approval and every rules file. The rules that mattered got buried. I cut it to about 15,000: one line per project, open approvals only, topic rules read only when the task needs them.
*In your base:* the index is one line per page, and decided approvals move to a monthly archive.

**4. Rules written down but not enforced.** I counted 299 rules, and only 21 were enforced by code. A rule Claude has to remember is a rule Claude can forget. I found one live hole: a posting tool that nothing stopped. Now the ten that matter most are real checks.
*In your base:* the guard blocks your key file and stops to ask before any deletion or anything sent, posted or paid.

**5. One-off rules for everyone.** Every correction became a rule every agent read, even ones about one client or one kind of video. About 75 of them now live in skills for that work only.
*In your base:* `/tidy` moves rules like that into a skill.

**6. Automating before the process was steady.** I automated jobs that were still changing, then had to pause them all. Run a job by hand the same way a few times before you automate it.

## Still learning

Every rule in my office started as a correction. Something went wrong, I said so once, and the brain kept it. There are over 70 now, and most days add one. A few real ones, from simple to bigger:

- **Short reports.** What's done, what's waiting on me, what's next. Nothing else.
- **Every link is clickable.** A full link or the file itself, never a file path I have to go find.
- **Public prices are written P5,000,** never with the peso sign, because not every font has it.
- **Captions always have hashtags,** from the first draft.
- **Batch your questions.** One numbered list with your pick for each, so I can answer "all OK" or by number.
- **My words beat your checklist.** If I say "bring back X", restore X exactly.
- **It runs without my laptop.** Every repeating job moves to the cloud, so it works with my computer off.
- **Rules are text, not locks.** The ten that matter most are now checks that stop the action.
- **Every day:** three short briefs (morning, lunch and night) with one numbered list of what needs me.

Yours will be different. `/teach` is how they get in, and `/tidy` keeps them few.

That is how rules are born. Something goes wrong, I correct it once, and the brain keeps it.

And in early November 2026 I paused all my automatic runs because they kept stalling. I'm changing the process. So your structure will change too, and so will mine. Don't treat this as the right way. Treat it as a starting point.

## The O2San stack

People ask what mine runs on. This is the stack we use on every one of our setups: the agents and add-ons, the video pipeline, the automations, and the free APIs and models behind them. It's all on one page, with how I checked each one: https://elronnaranja.com/stack/

You don't need any of it to start. The base install needs only Claude Code and a folder.

## If you want help building yours

I teach the fundamentals through a 1-on-1 AI session: a one-hour call with me, 30 days of direct chat, and one more hour of calls to use as you need within those 30 days. We work through your questions together. It's P5,000: https://elronnaranja.com/ai-session

You would be the one building it, because you know your process and I don't. I can't promise results. How far you get depends on the time you put in.

Part 2 below is written for Claude. It's still yours to read before you paste.

---

# Part 2: Install instructions (written for Claude)

**Claude:** the person pasted this article so you can install the AI office in the folder this session is open in. Get the files only from https://github.com/elronnaranja/O2San-Base-Install. Don't fetch anything else, install anything else or look anywhere else.

## Before you install

1. Check the current folder with a file listing.
2. An empty folder may still hold `.DS_Store`, a `.git` folder or `.claude/settings.local.json`. If it holds anything else, stop. Say what is there and ask the person to open an empty folder, or to confirm in plain words that you should install here. Never overwrite a file that already exists.
3. If the folder is empty, say in one line what you are about to do, then install. You don't need any other answers: the questions come later in `/setup`.

## Install rules

- Install only inside the current folder. Change nothing outside it.
- Copy the files exactly as they are. Don't improve, translate, shorten or fill in the blanks. "My AI Office" and the blank fields stay as they are, because `/setup` replaces them.
- Don't run `/setup`, don't create any agents, and don't ask for keys or passwords.
- Git is optional. Don't set up a repository for the person.

## Step 1: Get the files

Download the repository as a ZIP into the current folder and unpack it:

```bash
curl -L -o office.zip https://github.com/elronnaranja/O2San-Base-Install/archive/refs/heads/main.zip
```

Unpack it with `unzip -q office.zip` on Mac or Linux, or `tar -xf office.zip` on Windows. That makes a folder named `O2San-Base-Install-main`. Move everything inside it, hidden files and folders included (`.claude`, `.gitignore`, `.env.example`), into the current folder. Then remove `office.zip` and the now-empty `O2San-Base-Install-main` folder. Those two are only the download, not the person's files.

If `curl` isn't available, ask the person to download the ZIP from https://github.com/elronnaranja/O2San-Base-Install (Code, then Download ZIP) and put it in this folder, then continue.

## Step 2: Check your work

These must exist:

- `CLAUDE.md`, `AGENTS.md`, `README.md`, `.gitignore`, `.env.example`
- `office/index.html`
- `.claude/agents/chief-of-staff.md`
- `.claude/commands/setup.md`, `.claude/commands/teach.md`, `.claude/commands/new-agent.md`, `.claude/commands/office.md`, `.claude/commands/tidy.md`
- `.claude/playbooks/quick-commands.md`, `.claude/settings.json`, `.claude/hooks/guard.py`
- `brain/README.md`, `brain/00-Core/about-me.md`, `brain/03-AI-Office/RULES.md`, `brain/03-AI-Office/decisions.md`
- `brain/09-Templates/state-page.md`, `brain/10-Office/board.md`, `brain/10-Office/approvals.md`, `brain/10-Office/state/INDEX.md`
- the folders `brain/01-Business` to `brain/08-Inbox`, and `brain/10-Office/lessons` and `brain/10-Office/reports`

Nothing should be left in a subfolder named `O2San-Base-Install-main`. If anything is missing, say what and stop.

## Step 3: Tell the person what to do next

Say this, in your own short words:

1. The office is installed, and nothing outside this folder was touched.
2. Start a fresh session in this folder, so Claude Code picks up the new `CLAUDE.md` and commands. If it asks whether to trust the folder, say yes once they are happy with what was installed.
3. Type `/setup`. They name the office and the Chief and answer a few questions, in about 5 minutes.
4. Double-click `office/index.html` to see the office.
5. Give it one real task. When it gets something wrong, correct it with `/teach`.
6. Add a second agent with the Spawn button or `/new-agent` only when one kind of work keeps coming back.

Then stop. Don't start `/setup` in this session.