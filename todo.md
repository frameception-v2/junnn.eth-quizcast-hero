- [ ] Task 1: Create Start Screen Component  
  File: src/app/start/page.jsx  
  Action: Create new page component  
  Description:  
  ```jsx
  export default function StartScreen() {
    return (
      <div className="container">
        <h1>Farcaster Quiz</h1>
        <Link href="/question/1?score=0&current=1">
          <button className="start-button">Start Quiz</button>
        </Link>
      </div>
    )
  }
  ```
  UI: Title text container, styled start button  
  Criteria: Clicking button navigates to /question/1?score=0&current=1

- [ ] Task 2: Implement Question 1 Frame  
  File: src/app/question/[id]/page.jsx  
  Action: Create dynamic route  
  Description:
  ```jsx
  export default function Question1({ searchParams }) {
    const options = ['Option 1', 'Option 2', 'Option 3', 'Option 4'];
    
    const buildAnswerURL = (choiceIdx) => {
      return `/question/2?answers=${choiceIdx}&current=2`
    };

    return (
      <div className="question-frame">
        <h2>Sample Question 1</h2>
        {options.map((opt, idx) => (
          <Link key={idx} href={buildAnswerURL(idx)}>
            <button className="answer-button">{opt}</button>
          </Link>
        ))}
      </div>
    )
  }
  ```
  API Endpoint: GET /question/1  
  UI: 4 vertically stacked answer buttons  
  Criteria: Selection redirects to /question/2?answers=[X]&current=2

- [ ] Task 3: Create Question 2 Component  
  File: src/app/question/2/page.jsx  
  Action: Create new page  
  Description: Mirror Task 2 structure with:
  ```jsx
  const buildAnswerURL = (choiceIdx) => {
    const existing = searchParams.answers || [];
    return `/score?answers=${[...existing, choiceIdx]}`
  };
  ```
  Criteria: Redirects to /score?answers=[X,Y] after selection

- [ ] Task 4: Build Score Display Page  
  File: src/app/score/page.jsx  
  Action: Create score calculation  
  Description:
  ```jsx
  export default function Score({ searchParams }) {
    const answerCount = searchParams.answers?.split(',')?.length || 0;
    
    return (
      <div className="score-frame">
        <h2>Final Score: {answerCount}/2</h2>
        <button disabled className="share-button">Share Results</button>
      </div>
    )
  }
  ```
  Criteria: Always shows 0/2 with disabled share button

- [ ] Task 5: Add URL State Propagation  
  Files: All question pages + start screen  
  Action: Modify all navigation links to preserve URL params  
  Description: Update Link hrefs to include:
  ```jsx
  // In Question1:
  `/question/2?answers=${choiceIdx}&current=2&score=${searchParams.score}`
  
  // In Question2:
  `/score?answers=${[...existing, choiceIdx]}&score=${searchParams.score}`
  ```
  Criteria: Full quiz flow maintains params through browser reload

- [ ] Task 6: Implement Farcaster API Integration  
  File: src/app/api/farcaster/route.js  
  Action: Create API endpoint  
  Description:
  ```js
  export async function GET() {
    const res = await fetch('https://api.farcaster.xyz/v2/casts', {
      headers: { Authorization: `Bearer ${process.env.FARCASTER_KEY}` }
    });
    return new Response(JSON.stringify(await res.json()));
  }
  ```
  File: src/data/questions.js  
  Action: Create question data structure with:
  ```js
  export const Questions = [
    { text: 'Cast 1 Text', correctIndex: 0 },
    { text: 'Cast 2 Text', correctIndex: 1 }
  ];
  ```
  Criteria: Questions display actual cast text from API

- [ ] Task 7: Add OG Image Generation  
  File: src/app/api/og/route.js  
  Action: Create image endpoint  
  Description:
  ```js
  export async function GET(req) {
    const text = new URL(req.url).searchParams.get('text');
    // Implement @vercel/og ImageResponse with text overlay
  }
  ```
  File: All question pages  
  Action: Add image component:
  ```jsx
  <img src={`/api/og?text=${encodeURIComponent(questionText)}`} />
  ```
  Criteria: Questions display generated images with text

- [ ] Task 8: Implement Answer Validation  
  File: src/app/score/page.jsx  
  Action: Modify score calculation  
  Description:
  ```js
  import { Questions } from '@/data/questions';
  
  const correctAnswers = searchParams.answers?.split(',')
    .filter((ans, idx) => Questions[idx].correctIndex === parseInt(ans));
  ```
  Criteria: Score reflects actual correct answers

- [ ] Task 9: Add Error Boundary  
  File: src/app/error.jsx  
  Action: Create error component  
  Description:
  ```jsx
  'use client';
  export default function Error({ reset }) {
    return (
      <div>
        <h2>Invalid Quiz State</h2>
        <button onClick={() => window.location.href='/start'}>Restart</button>
      </div>
    )
  }
  ```
  Criteria: Invalid URLs redirect to /start

- [ ] Task 10: Add Perfect Score Easter Egg  
  File: src/app/score/page.jsx  
  Action: Modify image source conditionally  
  Description:
  ```jsx
  {correctAnswers.length === 2 && (
    <img src="/guiter-herro.png" alt="Special Celebration" />
  )}
  ```
  Criteria: 2/2 score displays special image