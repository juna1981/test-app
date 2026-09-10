# Quiz CLI

An interactive command-line quiz game for learning JavaScript, Node.js, and general programming concepts.

## Overview

Quiz CLI is a dependency-free Node.js application that runs in an interactive terminal. It loads quiz content from [`data/questions.json`](data/questions.json), lets the player choose a category and question count, shuffles the selected questions, and provides immediate feedback with explanations. After each round it displays the score and incorrect-answer review, then offers the option to play again.

The repository is named `test-app`; the npm package declared in `package.json` is `quiz-cli` version `1.0.0`.

## Features

- Interactive terminal-based category and question-count selection.
- Three included categories: JavaScript Basics, Node.js Fundamentals, and General Programming.
- Choice of all available questions, three questions, or five questions when the category contains enough questions.
- Fisher–Yates shuffling for each quiz round.
- Immediate correct/incorrect feedback and optional explanations from the question data.
- A 30-character Unicode progress bar and question counter.
- Final score, percentage, performance message, and incorrect-answer review.
- Replay prompt after each completed quiz.
- ANSI-colored output, Unicode progress indicators, and emoji without external packages.
- In-memory state only; scores are not persisted.

## Tech Stack

| Area | Implementation |
| --- | --- |
| Runtime | Node.js 18 or newer |
| Language | JavaScript with native ES modules (`"type": "module"`) |
| Terminal input | Node.js built-in `readline` module |
| File access | Node.js built-in `node:fs/promises`, `node:path`, and `node:url` modules |
| Content | JSON loaded at runtime |
| Dependencies | None; no runtime or development dependencies are declared |

## Prerequisites

- Node.js `18.0.0` or newer, as specified by `package.json`.
- An interactive terminal capable of accepting standard input.

The application does not require a browser, server, database, environment variables, or external services.

## Installation

Clone the repository and enter its directory:

```bash
git clone https://github.com/juna1981/test-app.git
cd test-app
```

No dependency installation is required because `package.json` declares no dependencies. Running `npm install` is optional and is not needed to run the application.

## Configuration

There is no environment-based configuration. At startup, `index.js` resolves the application directory and reads `data/questions.json` relative to it.

### Question format

Question content is grouped beneath a top-level `categories` object. Each category has a display `name` and a `questions` array. Each question has this shape:

```json
{
  "question": "What keyword is used to declare a constant in JavaScript?",
  "options": ["var", "let", "const", "define"],
  "answer": 2,
  "explanation": "The 'const' keyword declares a block-scoped constant that cannot be reassigned."
}
```

`answer` is a zero-based index into `options`; in this example, `2` identifies `const`. Keep the index valid when editing questions. If present, `explanation` is displayed after the answer.

To add or edit a category, preserve the existing category shape and provide a category key, display `name`, and `questions` array. Category keys are read dynamically by `index.js`, so a new category is included in the menu without a separate code registration step.

### Included categories

The bundled data currently contains five questions in each category:

| Key | Display name | Topics represented |
| --- | --- | --- |
| `javascript` | JavaScript Basics | Constants, array methods, strict equality, primitive types, and `typeof null` |
| `nodejs` | Node.js Fundamentals | File-system modules, the event loop, npm initialization, `process.argv`, and ES module imports |
| `general` | General Programming | APIs, recursion, JSON, callbacks, and version control |

## Usage

Start the game with npm:

```bash
npm start
```

Or invoke the entry point directly:

```bash
node index.js
```

The interactive flow is:

1. Choose a category by entering its menu number.
2. Choose all available questions, three questions, or five questions when that option is available.
3. Press Enter to begin.
4. Select each answer by entering its menu number.
5. Review immediate feedback and the explanation.
6. View the final score and incorrect-answer review.
7. Choose whether to play again.

When three or five questions is selected, `index.js` takes that many questions from the beginning of the category's JSON array. The `Quiz` class then shuffles that selected copy; the application does not randomly sample from the entire category before slicing.

### npm scripts

| Command | Description |
| --- | --- |
| `npm start` | Runs `node index.js` and starts the quiz. |
| `npm test` | Runs Node's built-in test runner with `node --test`. |

### Runtime behavior

`src/input.js` validates numbered selections and repeats the prompt until a valid option is entered. Yes/no confirmation returns `true` only for an answer beginning with `y`. If startup or execution fails, `index.js` prints the error and exits with status code `1`.

## File Structure

```text
.
├── data/
│   └── questions.json    # Categories, questions, answer indexes, and explanations
├── src/
│   ├── colors.js         # ANSI color and text-style helpers
│   ├── input.js          # readline interface and interactive prompts
│   └── quiz.js            # Quiz state, shuffling, scoring, and results
├── index.js              # Application entry point and replay loop
├── package.json           # npm metadata, scripts, and Node.js requirement
└── README.md              # Project documentation
```

### Architecture and data flow

1. `index.js` loads and parses `data/questions.json` with Node.js file-system APIs.
2. Category keys and display names populate the category menu.
3. The selected category's questions are sliced according to the chosen count.
4. A `Quiz` instance copies and shuffles those questions with the Fisher–Yates algorithm.
5. `input.js` handles `readline` prompts, numeric validation, confirmation, and pause prompts.
6. `Quiz` records answers in memory, updates the score, displays explanations, and renders results.
7. `colors.js` applies ANSI escape-code styles to terminal output.

## Testing

The repository includes a test script, but no test files are present. Automated coverage and specific test behavior are therefore not documented.

Run the declared command with:

```bash
npm test
```

## Deployment

This is a local command-line application. No deployment process, hosting configuration, build step, or service configuration is included. Run it directly with Node.js on a machine with an interactive terminal.

## Contributing

Contributions can improve the question bank, terminal experience, or quiz logic. When changing `data/questions.json`:

- Preserve the documented JSON structure.
- Ensure every `answer` value points to an item in its question's `options` array.
- Keep category names and question text suitable for the intended educational quiz.

For code changes, maintain compatibility with Node.js 18 or newer and run the declared npm commands where applicable. No repository-specific contribution workflow is documented.

## License

`package.json` declares the project under the **MIT License**. A separate `LICENSE` file is not present in the repository.
