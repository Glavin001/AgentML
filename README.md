# AgentML
> Agent Markup Languages for AI Agents

## Acceptance Criteria

- [ ] Document annotations for feedback/suggestions
  - [ ] Ability to quote/select/annotate user or assistant messages (useful for replies or )
- [ ] Citations from excerpts of read documents
  - [ ] Make explicit the boundaries between input documents, etc
  - [ ] Document meta data and contents separated
- [ ] Tool usage, with bulk/batching / parallel processing
  - [ ] Can run without stopping while streaming (clear end of tool arguments)
- [ ] Stateful application usage
  - [ ] Nested visual components
  - [ ] Self-Managing Resources (e.g. Close web browser window when ready after extracting needed info)
- [ ] Scratchpad / Notepad (add arbitrary data to remember for future prompts)
- [ ] Planning / Task management with dependencies
- [ ] Thoughts
  - [ ] Fast / "Thoughtless" mode
- [ ] Reflection
- [ ] Inline mental math
- [ ] Unstuck / course correction / inline edits without stopping
- [ ] System/instruction prompt, which can be injected multiple times
- [ ] Collaborative / Group chat / Multi-user discussions
  - [ ] Mention specific users
- [ ] Complex visual & interactive output (e.g. [dynamic form from JSON Schema](https://github.com/rjsf-team/react-jsonschema-form))
- [ ] Cost/Token efficient (e.g. use `<|special tokens|>`, shorter reference/alias IDs, prefer longer prompt to reduce completion, etc)
- [ ] Autoregressive (careful ordering of prompt input to be sequential/casual)
- [ ] Learn from textual feedback (reward/negative tokens)
  - [ ] Only train/learn to complete within `<|im_start|>assistant` and `<|im_end|>` (outside tokens `labels=-100`)
- [ ] File / Resource management
- [ ] Parse-able / Constraint prompting
- [ ] Examples with token separators
- [ ] Noop (allow LLM to explicitly do nothing)
- [ ] Efficient Editing operations (reorder items, find-and-replace, etc)
- [ ] Able to schedule (cron/calendar) or listen for events (e.g. when see person, receive email, webhook, etc)

## Inspiration

- [WebGPT](https://openai.com/research/webgpt)
- [Natbot](https://github.com/nat/natbot)
- [AlphaStar](https://www.deepmind.com/blog/alphastar-mastering-the-real-time-strategy-game-starcraft-ii)
- [Tree of Thoughts: Deliberate Problem Solving with Large Language Models](https://arxiv.org/pdf/2305.10601.pdf)
- [HuggingGPT: Solving AI Tasks with ChatGPT and its Friends in Hugging Face](https://arxiv.org/pdf/2303.17580.pdf)
- [LIMA: Less Is More for Alignment](https://arxiv.org/pdf/2305.11206.pdf)
- [Aligning Large Language Models through Synthetic Feedback](https://arxiv.org/pdf/2305.13735.pdf)
- [LETI: Learning to Generate from Textual Interactions](https://arxiv.org/pdf/2305.10314.pdf)
- [Principle-Driven Self-Alignment of Language Models from Scratch with Minimal Human Supervision](https://arxiv.org/pdf/2305.03047.pdf)
- [REACT: SYNERGIZING REASONING AND ACTING IN LANGUAGE MODELS](https://arxiv.org/pdf/2210.03629.pdf)
- [Schema.org](https://schema.org/)
- [Declarative UI](http://courses.csail.mit.edu/6.831/archive/2006/lectures/L9.pdf)

# Syntax

TODO: review WebGPT, HuggingGPT, ReAct, ChatML, etc to devise the syntax.

<!--
```xml
<|im_start|>system

<|im_end|>
<|im_start|>user 1

<|im_end|>
<|im_start|>assistant

<|im_end|>
<|im_start|>user2

<|im_end|>
```
--->

## Observations: Important Elements for UI Design

From [Declarative UIs](http://courses.csail.mit.edu/6.831/archive/2006/lectures/L9.pdf):

### Layout
- Box `<div>`
- Grid `<table>`

### Text
- Font & color `<span>`
- Paragraph `<p>`
- List `<ul>`, `<ol>`

### Widgets
- Hyperlink `<a>`
- Textbox `<input type=“text”>`
- Textarea `<textarea>`
- Drop-down `<select>`
- Listbox `<select multiple=“true”>`
- Button `<input type=“button”>`, `<button>`
- Checkbox `<input type=“checkbox”>`
- Radiobutton `<input type=“radio”>`

### Pixel output
- `<img>`

### Stroke output
- `<canvas>` (Firefox, Safari)

<!--
### Procedural code
- `<script>`

### Style sheets
- `<style>`
-->

## Actions

### From WebGPT

| Command | Effect |
| --- | --- |
| `Search <query>` | Send `<query>` to the Bing API and display a search results page |
| `Clicked on link <link ID>` | Follow the link with the given ID to a new page |
| `Find in page: <text>` | Find the next occurrence of `<text>` and scroll to it |
| `Quote: <text>` | If `<text>` is found in the current page, add it as a reference |
| `Scrolled down <1, 2, 3>` | Scroll down a number of times |
| `Scrolled up <1, 2, 3>` | Scroll up a number of times |
| `Top` | Scroll to the top of the page |
| `Back` | Go to the previous page |
| `End: Answer` | End browsing and move to answering phase |
| `End: <Nonsense, Controversial>` | End browsing and skip answering phase |

Prompt text given to the model:

```
♦Question
How can I train the crows in my neighborhood to bring me gifts?

♦Quotes
From Gifts From Crows | Outside My Window (www.birdsoutsidemywindow.org)
> Many animals give gifts to members of their own species but crows and
other corvids are the only ones known to give gifts to humans.

♦Past actions
Search how to train crows to bring you gifts
Click Gifts From Crows | Outside My Window www.birdsoutsidemywindow.org
Quote
Back

♦Title
Search results for: how to train crows to bring you gifts

♦Scrollbar: 0 - 11
♦Text
【0†How to Make Friends With Crows - PetHelpful†pethelpful.com】
If you did this a few times, your crows would learn your new place, but
as I said, I’m not sure if they will follow or visit you there since it’s
probably not in their territory. The other option is simply to make new
crow friends with the crows that live in your new neighborhood.

【1†Gifts From Crows | Outside My Window†www.birdsoutsidemywindow.org】
The partial piece of apple may have been left behind when the crow was
startled rather than as a gift. If the crows bring bright objects you’ll
know for sure that it’s a gift because it’s not something they eat.
Brandi Williams says: May 28, 2020 at 7:19 am.

♦Actions left: 96
♦Next action
```


# Tasks

## Example

> Sign Up for a service (e.g OpenAI):

1. Internet search for the service
    - Search: `openai sign up`
2. Browse to webpage from results
3. Click links to go to Sign Up page
    - https://platform.openai.com/signup
4. Fill in sign up form and submit
5. Check email for verification email
6. Complete email verification process

Example prompt for agent completing step 4:

```xml
<|im_start|>system
You are a helpful assistant.
<|im_end|>
<|im_start|>user 1
Sign Up for OpenAI
<|im_end|>
<|im_start|>summary
Searched for "openai sign up" and found webpage.
<|im_end|>
<|im_start|>system
<|application_start|>1 web_browser title="OpenAI API" url="https://platform.openai.com/signup"
Create your account
Please note that phone verification is required for signup. Your number will only be used to verify your identity for security purposes.
<input id=1>Email address</input>
<button id=2>Continue</button>
<button id=3>Already have an account? Log in</button>
OR
<button id=4>Continue with Microsoft Account</button>
<button id=5>Continue with Google
<button id=6>Continue with Apple</button>
<button id=7>Terms of use</button> | <button id=7>Privacy policy</button>
<|application_end|>
<|im_end|>
<|im_start|>assistant
I think the initial procedure necessitates submitting the email in the designated field (id=1), followed by pressing the "Continue" button (id=2).
<|action_start|>1
type 1
email@email.com
<|act_end|>
<|action_start|>2
click 2
<|act_end|>
<|im_end|>
```

<!--
<|actions_start|>
enum ClickableElement {
  Submit = 1
}
function click(element: ClickableElement);
enum InputElement {
  "Email Address" = 1
}
function type(element: InputElement, text: string);
<|actions_end|>
-->

## Ideas

### Instruction Following

1. **Cooking Recipes:** These often involve sequences of actions, some of which must be performed simultaneously or in a specific order.

2. **Assembly Instructions:** These could range from assembling furniture to building a computer or other devices.

3. **Science Experiments:** Step-by-step instructions for experiments in areas like chemistry, physics, or biology could be included.

4. **Math Problems:** Detailed instructions for solving various types of math problems could be included.

5. **Games and Puzzles:** Instructions for completing puzzles (like Sudoku or crossword puzzles) or playing complex games (like chess or Go) could be included.

6. **DIY/Craft Instructions:** These could include instructions for knitting a sweater, building a birdhouse, making jewelry, or other crafts.

7. **Medical Procedures:** Basic first aid procedures or even more complex procedures could be included, although care must be taken to ensure these instructions are accurate and safe to follow.

8. **Software and Hardware Troubleshooting Steps:** These could include instructions for resolving common computer issues.

9. **Emergency Procedures:** These could include instructions for what to do in various types of emergencies, like a fire or earthquake.

10. **Fitness Routines:** Detailed workout plans or yoga sequences could be included.

11. **Gardening Instructions:** These could include instructions for planting a tree, starting a vegetable garden, or caring for specific types of plants.

12. **Navigation Instructions:** These could include instructions for getting from one location to another, either on foot, by car, or using public transportation.

13. **Music and Art Lessons:** These could include step-by-step instructions for learning to play a musical instrument or creating a piece of art.

14. **Algorithms:** This could include sorting and searching algorithms, graph algorithms, numerical algorithms, string algorithms, geometrical algorithms, etc.
