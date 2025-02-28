### Step 1: Implement Start Screen Frame
```text
- Build: /start endpoint with initial frame containing title text and "Start Quiz" button
- Outcome: Frame displays correctly and button redirects to /question/1 with ?score=0&current=1 in URL params
```

### Step 2: Create First Question Frame
```text
- Build: /question/1 endpoint with hardcoded question text, 4 dummy options, and answer buttons
- Outcome: Frame shows question with 4 choices, selecting any redirects to /question/2?answers=[X]&current=2
```

### Step 3: Implement Second Question Frame
```text
- Build: /question/2 endpoint mirroring Step 2 structure
- Outcome: Frame shows second question, selecting answer redirects to /score?answers=[X,Y]
```

### Step 4: Build Basic Score Display
```text
- Build: /score endpoint parsing URL params to calculate score (all wrong by default)
- Outcome: Final frame displays "Final Score: 0/2" with non-functional share button
```

### Step 5: Add State Management via URL
```text
- Build: State persistence through URL parameters (score, current question, answers)
- Outcome: Full flow maintains state through reloads and back navigation via URL params
```

### Step 6: Integrate Farcaster Cast Fetching
```text
- Build: API call to Farcaster /v2/casts endpoint with Bearer auth to get user's last 2 casts
- Outcome: Questions use actual cast text (or fallback if none) with castReference populated
```

### Step 7: Implement Image Generation
```text
- Build: OpenGraph image service integration for question frames with text overlay
- Outcome: Question frames display generated images with question text embedded
```

### Step 8: Add Answer Validation Logic
```text
- Build: Score calculation comparing answers to Question.correctIndex
- Outcome: /score displays accurate points based on correct answers from Question data
```

### Step 9: Handle Error Scenarios
```text
- Build: Error boundaries for invalid answers, state mismatch, and missing casts
- Outcome: Invalid flows reset to /start and display fallback content where appropriate
```

### Step 10: Add Easter Egg Feature
```text
- Build: Special image variant detection when score === 2
- Outcome: Perfect score displays "guiter herro" image variant in final frame
```