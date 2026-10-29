# Lemon Clicker Bug Report

**Name:**
**Tickets solved:** <!-- e.g. 1, 2, 3, 5, 6 (at least five) -->

Copy the template below once for each ticket you solved. 

---

## Ticket <number>: <ticket title>

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function:**

**What was wrong:**

### Fix

<!-- What did you change, and why does that change fix the problem? -->

### How did you find it?

#### Hypothesis testing

**What hypothesis ultimately led you to identify the buggy code, and why?**

#### Backward tracing

<!-- Start from the symptom you saw and work backwards, one step at a time,
     until you reach the buggy line. For each step, note where you looked
     and what you saw there. Add steps as needed. -->

1. **Symptom:** <!-- what you saw, e.g. text on screen, console error -->
2. **Traced back to:** <!-- where that came from, e.g. a function, a request in the Network tab, a server log -->
3. **Traced back to:**
4. **Buggy code:** <!-- the file, function and line where the trace ended -->

<!-- It is okay to use your judgement here in limiting the number of traceback steps. We want to understand your 
    reasoning for how you discovered where the bug was -->

### Confirming the fix

<!-- How did you confirm the problem is gone? -->

### bug reports below 

-----------------------------------------------------------------------------------------------------------------

## Ticket 1: Stats always say "Stats unavailable"

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function:** MenuScreenController.ts in loadStatsFromServer() function

private async loadStatsFromServer(): Promise<LeaderboardStats> {
		const response = await fetch("/api/stat");
		if (!response.ok) {
			throw new Error("Unable to load stats");
		}
		return (await response.json()) as LeaderboardStats;
	}

**What was wrong:** response was pulling from the wrong url link

previous wrong: const response = await fetch("/api/stat");
corrected: const response = await fetch("/api/stats");

### Fix

<!-- What did you change, and why does that change fix the problem? -->

### How did you find it?
found it through console and saw that the api it was fetching was unavailable
it pointed at the right line/function to where it failed

#### Hypothesis testing

**What hypothesis ultimately led you to identify the buggy code, and why?**

#### Backward tracing

<!-- Start from the symptom you saw and work backwards, one step at a time,
     until you reach the buggy line. For each step, note where you looked
     and what you saw there. Add steps as needed. -->

1. **Symptom:** console error
2. **Traced back to:** MenuScreenController.ts in loadStatsFromServer() function
3. **Traced back to:**
4. **Buggy code:** const response = await fetch("/api/stat");

<!-- It is okay to use your judgement here in limiting the number of traceback steps. We want to understand your 
    reasoning for how you discovered where the bug was -->
i explained above...

### Confirming the fix

<!-- How did you confirm the problem is gone? -->
i saw the expected behavior whne displaying the score report in the menu

---


## Ticket 3: The lemon only lands in four spots

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function:**  GameScreenController.ts -> handleLemonClick() -> randomizeLemonPosition()

**What was wrong:** random position generated was always 0 or 1 with the founding
		// Generate random position
		const randomX = Math.round(Math.random()) * (maxX - minX) + minX;
		const randomY = Math.round(Math.random()) * (maxY - minY) + minY;

### Fix

<!-- What did you change, and why does that change fix the problem? -->

### How did you find it? hypothesis testing in assuming the position being updated was wrong, so position is where i should look

LOOKED IN THE GAME SCREEN FOLDER, CONSIDERED THE LEMON, CONSIDERED THE LEMON ACTIONS, CONSIDER THE RESET POSITION AFTER CLICK

#### Hypothesis testing

**What hypothesis ultimately led you to identify the buggy code, and why?**

#### Backward tracing

<!-- Start from the symptom you saw and work backwards, one step at a time,
     until you reach the buggy line. For each step, note where you looked
     and what you saw there. Add steps as needed. -->

1. **Symptom:** <!-- what you saw, e.g. text on screen, console error --> random positions were limited since the math to generate was wrong
2. **Traced back to:** GameScreenController.ts -> handleLemonClick() -> randomizeLemonPosition()
3. **Traced back to:**
4. **Buggy code:** <!-- the file, function and line where the trace ended -->

WRONG
		// Generate random position
		const randomX = Math.round(Math.random()) * (maxX - minX) + minX;
		const randomY = Math.round(Math.random()) * (maxY - minY) + minY;

    CORRECT
        		// Generate random position
		const randomX = Math.round(Math.random() * (maxX - minX) + minX);
		const randomY = Math.round(Math.random() * (maxY - minY) + minY);

<!-- It is okay to use your judgement here in limiting the number of traceback steps. We want to understand your 
    reasoning for how you discovered where the bug was -->

### Confirming the fix

i played the game and saw updated random positions

----------------------------------------------------------------------------------------------------

## Ticket 4: Stuck on the Game Over screen

GameScreenController's endGame function had this.stopTimer, which doesnt call the function

backwards testing, i played the game then saw that the game kept submitting scores,
so i saw the game didnt end

i then investigated where the end game functionality is

i saw the function wasnt being called.

orginally
	private endGame(): void {
		this.stopTimer;

now
	private endGame(): void {
		this.stopTimer();