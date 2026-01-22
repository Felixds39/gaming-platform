# Game Score Auto-Submission API

Games embedded in the platform can automatically submit scores to the backend when they complete. This guide shows how to implement automatic score tracking in your games.

## How It Works

1. **Game completes** - Player finishes playing
2. **Game sends score** - Game iframe sends score via `postMessage`
3. **Parent captures score** - Game.jsx receives the message
4. **Score submitted** - Platform automatically records the score in the database
5. **Admin dashboard** - Score appears in analytics

## Implementation Guide

### For HTML Games (Embedded in iframes)

When your game is complete and you want to submit the final score, use `postMessage` to send it to the parent window:

```javascript
// When game ends
const finalScore = 150; // Your game's score calculation

// Send score to parent window
window.parent.postMessage({ 
  type: 'gameScore', 
  score: finalScore 
}, '*');
```

### Example: Quiz Game

```javascript
function endGame() {
  const correctAnswers = 7;
  const totalQuestions = 10;
  const scorePercentage = (correctAnswers / totalQuestions) * 100;
  
  // Show completion message
  document.getElementById('result').innerHTML = `
    <h2>Quiz Complete!</h2>
    <p>Your Score: ${correctAnswers}/${totalQuestions}</p>
    <p>Percentage: ${scorePercentage}%</p>
  `;
  
  // Send score automatically
  window.parent.postMessage({ 
    type: 'gameScore', 
    score: scorePercentage 
  }, '*');
}
```

### Example: Action/Canvas Game

```javascript
function gameOver() {
  // Game ended, calculate final score
  const finalScore = player.points + (lives * 50);
  
  // Show game over screen
  document.getElementById('game-over').style.display = 'block';
  document.getElementById('final-score').textContent = finalScore;
  
  // Send score to platform
  window.parent.postMessage({ 
    type: 'gameScore', 
    score: finalScore 
  }, '*');
}
```

## Score Format

The message object should have:
- **type**: `'gameScore'` (required)
- **score**: Number between 0-100 (or any numeric value, recommended 0-100)

## Message Format

```javascript
window.parent.postMessage({ 
  type: 'gameScore', 
  score: 85 
}, '*');
```

## What Happens Next

1. Parent window (Game.jsx) receives the message
2. Validates the message type
3. Calls `api.submitScore(gameId, score)`
4. Score is saved to database with timestamp
5. User sees confirmation message
6. Admin can see score in dashboard analytics

## Score Calculation Tips

**Quiz Games:**
```javascript
const score = (correctAnswers / totalQuestions) * 100;
```

**Action Games:**
```javascript
const score = points + bonusPoints;
```

**Time-based Games:**
```javascript
const score = Math.max(0, 100 - (timeElapsed / 10));
```

**Collecting Games:**
```javascript
const score = itemsCollected * 10;
```

## Admin Dashboard Display

After submission, scores appear in:
- **Game Statistics** - Max score, average score per game
- **Top Scorers** - Top player for each game
- **User Analytics** - Tracks participation

## Testing Score Submission

1. Play a game locally
2. Complete the game
3. Check the Game page for "✅ Score recorded: X" message
4. Go to Admin Dashboard
5. Verify score appears in Game Statistics section

## Security Notes

- Scores are only recorded if user is logged in
- `requireAuth` middleware validates user token
- Database tracks user_id with each score
- No client-side score validation (submit any number)

## Migration Guide

To update existing games:

1. Find where the game ends (endGame, onComplete, etc.)
2. Calculate your final score as a number
3. Add: `window.parent.postMessage({ type: 'gameScore', score: yourScore }, '*');`
4. Test by playing the game and checking for confirmation message

---

**Example games already updated:**
- Books of the Bible Quiz
- Ten Commandments Quiz
- Jesus Miracles Quiz
- Beatitudes Runner
