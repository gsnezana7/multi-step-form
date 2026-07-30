# Frontend Mentor - Multi-step form solution

This is a solution to the [Multi-step form challenge on Frontend Mentor](https://www.frontendmentor.io/challenges/multistep-form-YVAnSdqQBJ). Frontend Mentor challenges help you improve your coding skills by building realistic projects.

## Table of contents

- [Overview](#overview)
  - [The challenge](#the-challenge)
  - [Screenshot](#screenshot)
  - [Links](#links)
- [My process](#my-process)
  - [Built with](#built-with)
  - [What I learned](#what-i-learned)
  - [Continued development](#continued-development)
  - [Useful resources](#useful-resources)
  - [AI Collaboration](#ai-collaboration)
- [Author](#author)

## Overview

### The challenge

Users should be able to:

- Complete each step of the sequence
- Go back to a previous step to update their selections
- See a summary of their selections on the final step and confirm their order
- View the optimal layout for the interface depending on their device's screen size
- See hover and focus states for all interactive elements on the page
- Receive form validation messages if a field has been missed, or the email address is not formatted correctly

### Screenshot

![](./screenshot.jpg)

### Links

- Solution URL: [GitHub Repository](https://github.com/gsnezana7/multi-step-form>)
- Live Site URL: [Live Demo Website](https://<твое-приложение>.vercel.app)

## My process

### Built with

- Semantic HTML5 markup (including Definition Lists `<dl>` and `<fieldset>`)
- CSS custom properties (Variables)
- Flexbox & CSS Grid
- Mobile-first workflow
- [Vue 3](https://vuejs.org) - Composition API (`<script setup>`)
- [Vue Tel Input](https://github.comiamstevendao/vue-tel-input) - For international phone validation
- BEM (Block Element Modifier) methodology for styling

### What I learned

While working on this project, I significantly leveled up my skills in building robust multi-step architectures in Vue 3 and implementing high-level Web Accessibility (A11y) standards.

Key takeaways:

1. **Native Semantic Forms over Custom Divs:** I learned that instead of simulating custom radio buttons and checkboxes via `div` elements, it's far better to use natively hidden `<input type="radio">` and `<input type="checkbox">` wrapped in `<label>`. This grants automatic keyboard navigation and screen reader support out of the box.
2. **Dynamic ARIA Attributes:** I implemented dynamic `aria-current="step"` on step components and mapped `aria-describedby` to input fields to announce validation errors dynamically.
3. **Financial Semantics:** I refactored the final summary list from standard `<ul>` lists to semantic Definition Lists (`<dl>`, `<dt>`, `<dd>`), which is a best practice for structured checkout checks.

Here is a snippet of the accessibility-focused Vue template I am proud of:

```html
<li
  class="wizard__step-item"
  :aria-current="currentStep === 1 ? 'step' : undefined"
>
  <span
    class="wizard__step-number"
    :class="{ 'is-active': currentStep === 1 }"
    aria-hidden="true"
    >1</span
  >
  <div class="wizard__step-text-container">
    <span class="wizard__step-meta">Step 1</span>
    <span class="wizard__step-name">Your info</span>
  </div>
</li>
```

### Continued development

In future projects, I want to keep focusing on:

- **Advanced A11y Standards:** Diving deeper into Focus Management in complex Single Page Applications (SPAs).
- **Form State Management:** Exploring global state management tools like Pinia for splitting vast form logic across multiple files.

### Useful resources

- [W3C WAI-ARIA Authoring Practices](https://w3.org) - This helped me understand how to implement accessible multi-step workflows.
- [The Markdown Guide](https://www.markdownguide.org/) - A great reference for creating professional repository documentation.

### AI Collaboration

I collaborated with an AI programming assistant acting as a Senior Frontend Developer and Mentor during this challenge.

- **Tools Used:** ChatGPT / Claude.
- **Workflow:** Instead of just generating ready-made code blocks, the assistant guided me through debugging reactive Vue 3 components, optimizing BEM architectures, and finding semantic code bottlenecks.
- **What worked well:** The AI was exceptional at code auditing, explaining deep web accessibility concepts (like `aria-current` vs `aria-live`), and helping me refactor my initial template structures into production-ready semantic HTML.

## Author

- Frontend Mentor - [@<твой-логин-на-frontend-mentor>](https://www.frontendmentor.io/profile/<твой-логин-на-frontend-mentor>)
- GitHub - [<Snezana>](https://github.com<твой-логин-на-гитхабе>)
