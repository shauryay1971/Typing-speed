# Typing Speed Test Implementation Analysis

## Overview

This project (`typing-speed-test.html`) is a standalone HTML file that implements an interactive **Typing Speed Test** using vanilla HTML, CSS, and JavaScript.

The application measures:

- Typing Speed (WPM)
- Accuracy
- Correct/Wrong Words
- Remaining Time

The user types a randomly selected passage while the application tracks performance in real time.

---

# Core Features

- Random typing passages
- Live WPM calculation
- Live accuracy tracking
- Word-by-word highlighting
- Correct and wrong word coloring
- Countdown timer
- Restart functionality
- Real-time UI updates
- Fully built without libraries/frameworks

---

# Code Structure Breakdown

## 1. HTML & CSS

### UI Components

The page contains:

- Title and subtitle
- Passage display area
- Typing input box
- Live stats section
- Result box
- Restart button

### Styling

CSS is used for:

- Layout and spacing
- Word highlighting
- Correct/wrong word colors
- Responsive sizing
- Minimal clean UI

### Important CSS Classes

```css
.word-current
.word-correct
.word-wrong
```

These visually indicate:

- Current active word
- Correctly typed words
- Incorrectly typed words

---

## 2. Passages and State Variables

### Passages Array

```javascript
var PASSAGES = [
  ...
];
```

Stores multiple typing passages.

A random passage is selected at the start of each test.

---

### Timer Configuration

```javascript
var TIME_LIMIT = 60;
```

Defines the test duration in seconds.

---

### State Variables

```javascript
var words
var currentIndex
var correctCount
var wrongCount
var timeLeft
var timer
var testStarted
```

These track:

- Current word
- Score
- Accuracy
- Timer state
- Test progress

---

## 3. DOM References

The program stores references to important HTML elements:

```javascript
var passageDiv
var typingInput
var timeDisplay
var wpmDisplay
var accDisplay
var resultDiv
var restartBtn
```

This allows JavaScript to dynamically update the page.

---

## 4. Passage Setup Logic

### loadPassage()

This function:

1. Selects a random passage
2. Splits it into individual words
3. Creates one `<span>` per word
4. Adds the spans into the passage container
5. Highlights the first word

---

### Example

```javascript
words = passage.split(' ');
```

Converts:

```text
Hello world today
```

into:

```javascript
["Hello", "world", "today"]
```

---

### Word Span Creation

Each word becomes:

```html
<span id="word-0">Hello</span>
```

This allows individual word styling.

---

## 5. Reset Logic

### resetTest()

Resets the entire application state.

It:

- Stops timer
- Resets counters
- Clears input
- Enables typing
- Resets stats
- Loads a new passage

---

## 6. Timer System

### startTimer()

Uses:

```javascript
setInterval()
```

to decrease the timer every second.

---

### Timer Flow

Every second:

```javascript
timeLeft--;
```

updates the displayed timer.

When:

```javascript
timeLeft <= 0
```

the test ends.

---

## 7. Word Highlighting System

### highlightWord(index)

Adds the blue highlight to the current word.

---

### clearHighlight(index)

Removes active highlight.

---

### markWord(index, isCorrect)

Marks words as:

- green → correct
- red → wrong

using CSS classes.

---

## 8. WPM Calculation

### Formula

```javascript
correct words ÷ minutes elapsed
```

Implemented as:

```javascript
correctCount / minutesElapsed
```

---

### Example

If:

- correct words = 30
- elapsed time = 0.5 min

Then:

```text
WPM = 60
```

---

## 9. Accuracy Calculation

### Formula

```javascript
(correct ÷ total attempted) × 100
```

---

### Example

If:

- correct = 40
- wrong = 10

Then:

```text
Accuracy = 80%
```

---

## 10. Live Stats Updates

### updateStats()

Updates:

- WPM display
- Accuracy display

in real time while typing.

---

## 11. End Test Logic

### endTest()

Stops the timer and disables input.

It then displays:

- Final WPM
- Accuracy
- Correct words
- Wrong words

inside the result box.

---

## 12. Typing Input Handler

### Main Event Listener

```javascript
typingInput.addEventListener('keyup', ...)
```

This is the heart of the application.

---

## How It Works

### Step 1 — Start Timer

On the first key press:

```javascript
startTimer();
```

---

### Step 2 — Detect Spacebar

The app waits for:

```javascript
event.key === ' '
```

because words are checked only after pressing Space.

---

### Step 3 — Compare Words

The typed word is compared with the expected word.

```javascript
typedWord === expectedWord
```

---

### Step 4 — Update Counters

Depending on correctness:

```javascript
correctCount++
```

or

```javascript
wrongCount++
```

---

### Step 5 — Move Forward

The app:

- colors the word
- moves to next word
- clears input
- updates stats

---

### Step 6 — Finish Test

If all words are completed:

```javascript
endTest();
```

runs immediately.

---

## 13. Restart Button

### restartBtn.addEventListener()

Allows the user to restart the test anytime.

---

# Process Flowchart

```mermaid

flowchart TD

Start([Page Load]) --> Reset[resetTest()]
Reset --> LoadPassage[Load Random Passage]
LoadPassage --> CreateSpans[Create Word Spans]
CreateSpans --> Highlight[Highlight First Word]
Highlight --> Wait[Wait for User Typing]

Wait --> KeyPress[User Types]

KeyPress --> Started{Test Started?}

Started -- No --> StartTimer[startTimer()]
Started -- Yes --> Continue

StartTimer --> Continue

Continue --> SpacePressed{Space Key Pressed?}

SpacePressed -- No --> Wait
SpacePressed -- Yes --> CheckWord[Compare Typed Word]

CheckWord --> Correct{Correct Word?}

Correct -- Yes --> IncrementCorrect[correctCount++]
Correct -- No --> IncrementWrong[wrongCount++]

IncrementCorrect --> MarkWord
IncrementWrong --> MarkWord

MarkWord[Color Word Green/Red]
MarkWord --> NextWord[Move to Next Word]

NextWord --> UpdateStats[Update WPM and Accuracy]

UpdateStats --> Finished{All Words Done?}

Finished -- Yes --> EndTest[endTest()]
Finished -- No --> HighlightNext[Highlight Next Word]

HighlightNext --> Wait

StartTimer --> TimerLoop((Every 1 Second))

TimerLoop --> DecreaseTime[timeLeft--]
DecreaseTime --> UpdateTimer[Update Timer Display]

UpdateTimer --> TimeOver{timeLeft <= 0 ?}

TimeOver -- Yes --> EndTest
TimeOver -- No --> TimerLoop

EndTest --> ShowResults[Display Final Stats]

ShowResults --> RestartWait[Wait for Restart]

RestartWait --> RestartClick[Restart Button Click]

RestartClick --> Reset

```

---

# Concepts Demonstrated

This project demonstrates:

- DOM Manipulation
- Event Listeners
- Timers
- Arrays
- String Handling
- Real-Time UI Updates
- Dynamic HTML Generation
- State Management
- User Input Handling
- JavaScript Logic Building

---

# Learning Purpose

This project is excellent for learning:

- JavaScript fundamentals
- Interactive web applications
- Real-time typing systems
- Frontend logic
- Browser events
- Dynamic rendering

---

# Author

Shaurya Yadav

---

# License

This project is open-source and free to use.
