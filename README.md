# Quiz CLI

An interactive command-line quiz game for learning JavaScript, Node.js, and general programming concepts.

An interactive command-line quiz game for learning JavaScript, Node.js, and general programming concepts.

## Overview

Quiz CLI is a dependency-free Node.js application that runs in an interactive terminal. It loads quiz content from `data/questions.json`, lets you choose a category and question count, presents shuffled questions, and provides immediate feedback with explanations. At the end of a round it displays the score and a review of incorrect answers, then offers the option to play again.

The repository name is `test-app`, while the npm package name declared in `package.json` is `quiz-cli`.

## Features

- Interactive terminal-based category and question-count selection.
- Three included categories: JavaScript Basics, Node.js Fundamentals, and General Programming.
- Choice of all available questions, three questions, or five questions when the selected category has enough questions.
- Shuffled question order for each quiz round.
- Immediate correct/incorrect feedback and explanations.
- Progress bar and question counter.
- Final score, percentage, performance message, and incorrect-answer review.
- Replay prompt after each completed quiz.
- ANSI-colored output, Unicode progress indicators, and emoji for terminal presentation.
- No external runtime or development dependencies.

## Tech Stack

- **Runtime:** Node.js 18 or newer
- **Language:** JavaScript using native ES modules
- **Input:** Node.js built-in `readline` module
- **File I/O:** Node.js built-in `node:fs/promises`, `node:path`, and `node:url` modules
- **Data:** JSON file loaded at runtime
- **Dependencies:** None

## Prerequisites

- Node.js `18.0.0` or newer, as specified by the `engines` field in `package.json`.
- An interactive terminal capable of accepting standard input.

No browser, server, database, environment variables, or external services are required.

## Installation

Clone the repository and enter its directory:

```bash
git clone https://github.com/juna1981/test-app.git
cd test-app
```

The application has no declared dependencies, so installing packages is not required. If you want npm to process the package manifest, running `npm install` is optional and may create a lockfile.

## Configuration

There is no environment-based configuration. Quiz content is stored in [`data/questions.json`](data/questions.json) and is read relative to `index.js` when the application starts.

### Customizing questions

Questions are grouped beneath the top-level `categories` object. Each category contains a display `name` and a `questions` array. A question uses this structure:

```json
{
  "question": "What keyword is used to declare a constant in JavaScript?",
  "options": ["var", "let", "const", "define"],
  "answer": 2,
  "explanation": "The 'const' keyword declares a block-scoped constant that cannot be reassigned."
}
```

`answer` is a zero-based index into `options`; in the example, `2` identifies `const`. Keep the answer index valid when changing the options. The `explanation` is shown after the answer and should describe the correct response.

To add or edit a category, preserve the existing category shape and provide a unique category key, a display `name`, and a `questions` array. Categories are discovered from the JSON file, so they appear in the category menu without a separate registration step.

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

Each category currently contains five questions. When three questions is selected, the first three entries in that category's JSON array are chosen and then shuffled; the application does not randomly sample three questions from all five entries.

## Quiz Categories

The bundled question data contains:

| Category key | Display name | Topics represented |
| --- | --- | --- |
| `javascript` | JavaScript Basics | Constants, array methods, strict equality, primitive types, and `typeof null` |
| `nodejs` | Node.js Fundamentals | File-system modules, the event loop, npm initialization, `process.argv`, and ES module imports |
| `general` | General Programming | APIs, recursion, JSON, callbacks, and version control |

## File Structure

```text
.
├── data/
│   └── questions.json    # Categories, questions, answer indexes, and explanations
├── src/
│   ├── colors.js         # ANSI color and text-style helpers
│   ├── input.js          # readline interface and interactive prompts
│   └── quiz.js            # Quiz state, shuffling, scoring, and results
├── index.js              # Application entry point and main interaction loop
├── package.json           # npm metadata, scripts, and Node.js requirement
└── README.md              # Project documentation
```

## Architecture Overview

1. `index.js` resolves the application directory and reads `data/questions.json` using Node.js file-system APIs.
2. The loaded category keys populate the category selection menu.
3. The selected category's questions are sliced according to the chosen count and passed to the `Quiz` class.
4. `Quiz` makes a copy of the selected questions and shuffles that copy with the Fisher–Yates algorithm.
5. `input.js` uses the built-in `readline` module to validate numbered selections, yes/no confirmation, and Enter-to-continue prompts.
6. `Quiz` records answers in memory, updates the score, shows explanations, and renders final results.
7. `colors.js` supplies ANSI escape-code helpers for the terminal UI.

Scores and progress exist only during the running process. The application does not persist results.

## Scripts

The `package.json` manifest defines these npm scripts:

| Command | Description |
| --- | --- |
| `npm start` | Runs `node index.js` and starts the quiz. |
| `npm test` | Runs Node's built-in test runner with `node --test`. |

## Testing

A test script is present, but no test files are included in the repository. Consequently, automated test coverage and specific test behavior are not documented.

To run the declared test command:

```bash
npm test
```

## Limitations and Notes

- The game requires an interactive terminal; it is not a browser or server application.
- Output uses ANSI color codes, Unicode bar characters, and emoji. The appearance may vary by terminal.
- There is no database, network service, build step, or deployment configuration.
- There are no environment variables or environment files used by the application.
- Results are not saved after the process exits.
- Question counts are limited by the options implemented in `index.js`; the current data provides five questions per category.
- If a question file is malformed or cannot be read, the application reports the error and exits with status code 1.

## Contributing

Contributions can improve the question bank, terminal experience, or quiz logic. When changing question data, preserve the documented JSON structure and ensure every `answer` value points to an item in its question's `options` array. For code changes, keep the application compatible with Node.js 18 or newer and run the declared npm commands where applicable.

No repository-specific contribution workflow is documented.

## Deployment

This project is a local command-line application. No deployment process or hosting configuration is included; run it directly with Node.js on a machine with an interactive terminal.

## License

The package metadata declares the project under the **MIT License**. A separate `LICENSE` file is not present in the repository, so refer to `package.json` for the declared license information.
