---
tag: modus-wc-textarea
category: Forms & Data Entry
storybook: https://trimble-oss.github.io/modus-wc-2.0/main/?path=/docs/components-forms-textarea--docs
---

# Modus WC Textarea

Multi-line text field with Modus styling and validation. Supports helper/error/success messages, clear button, two preset sizes, and full form integration.

## Attributes

| Name               | Type      | Default     | Notes                                            |
| ------------------ | --------- | ----------- | ------------------------------------------------ |
| `aria-label`       | `string`  | `''`        | Screen-reader text when no visible label         |
| `auto-focus-input` | `boolean` | `false`     | Auto-focuses textarea on mount                   |
| `autocorrect`      | `boolean` | _(browser)_ | Safari/iOS automatic correction                  |
| `clearable`        | `boolean` | `false`     | Shows inline × clear button                      |
| `disabled`         | `boolean` | `false`     | Blocks input                                     |
| `enterkeyhint`     | `string`  | `''`        | Virtual Enter key suggestion for mobile          |
| `error-text`       | `string`  | `''`        | Error message below field                        |
| `helper-text`      | `string`  | `''`        | Neutral helper message                           |
| `label`            | `string`  | `''`        | Floating label above textarea                    |
| `max-length`       | `number`  | `undefined` | Character limit                                  |
| `min-length`       | `number`  | `undefined` | Minimum character requirement                    |
| `placeholder`      | `string`  | `''`        | Placeholder text                                 |
| `read-only`        | `boolean` | `false`     | Visible but not editable                         |
| `required`         | `boolean` | `false`     | Required for form submission                     |
| `rows`             | `number`  | `undefined` | Initial visible line count                       |
| `size`             | `string`  | `"medium"`  | Height preset (`"medium"`, `"large"`)            |
| `spellcheck`       | `boolean` | _(browser)_ | Spell-checking toggle                            |
| `text-align`       | `string`  | `"left"`    | Text alignment (`"left"`, `"center"`, `"right"`) |
| `valid-text`       | `string`  | `''`        | Success message below field                      |
| `value`            | `string`  | `''`        | Current textarea content                         |
| `custom-class`     | `string`  | `''`        | Extra CSS class for styling                      |

## Events

| Event         | Payload               | Description                 |
| ------------- | --------------------- | --------------------------- |
| `valueChange` | `CustomEvent<string>` | Fires on every input change |

## Public Methods

| Method         | Returns         | Description                     |
| -------------- | --------------- | ------------------------------- |
| `focusInput()` | `Promise<void>` | Focuses the underlying textarea |

## Basic Usage

```html
<form id="feedbackForm" style="max-width: 600px; margin: 2rem auto;">
  <h2>Send Us Your Feedback</h2>

  <div style="display: flex; flex-direction: column; gap: 2rem;">
    <!-- Main feedback textarea -->
    <modus-wc-textarea
      id="feedback"
      name="feedback"
      label="Your Feedback"
      helper-text="Please provide detailed feedback. Maximum 1000 characters."
      placeholder="Tell us about your experience, suggestions, or issues..."
      rows="6"
      max-length="1000"
      required
      size="large"
      clearable
    ></modus-wc-textarea>

    <!-- Additional comments -->
    <modus-wc-textarea
      name="suggestions"
      label="Additional Suggestions (Optional)"
      placeholder="Any other suggestions or ideas?"
      rows="4"
      size="medium"
      max-length="500"
    ></modus-wc-textarea>

    <!-- Technical details -->
    <modus-wc-textarea
      name="technicalDetails"
      label="Technical Details (Optional)"
      helper-text="Browser version, OS, steps to reproduce issues, etc."
      placeholder="Browser: Chrome 118.0
OS: Windows 11
Steps to reproduce:
1. 
2. 
3."
      rows="8"
      size="medium"
      spellcheck="false"
    ></modus-wc-textarea>

    <!-- Read-only confirmation -->
    <modus-wc-textarea
      label="Submission Confirmation"
      value="By submitting this form, you agree to our terms of service and privacy policy."
      rows="3"
      read-only
      size="medium"
    ></modus-wc-textarea>

    <!-- Character counter -->
    <div
      id="charCounter"
      style="text-align: right; color: var(--modus-wc-color-base-content); font-size: 0.875rem;"
    >
      Characters: <span id="charCount">0</span> /
      <span id="charLimit">1000</span>
    </div>

    <div style="display: flex; gap: 1rem; justify-content: flex-end;">
      <modus-wc-button type="reset" variant="outlined">
        Clear Form
      </modus-wc-button>
      <modus-wc-button type="submit" color="primary">
        Send Feedback
      </modus-wc-button>
    </div>
  </div>
</form>

<script type="module">
  const feedbackTextarea = document.getElementById("feedback");
  const charCount = document.getElementById("charCount");

  feedbackTextarea.addEventListener("valueChange", (e) => {
    const length = e.detail.length;
    charCount.textContent = length;

    // Color coding for character limit
    if (length > 900) {
      charCount.style.color = "var(--modus-wc-color-error)";
    } else if (length > 750) {
      charCount.style.color = "var(--modus-wc-color-warning)";
    } else {
      charCount.style.color = "var(--modus-wc-color-base-content)";
    }
  });

  document.getElementById("feedbackForm").addEventListener("submit", (e) => {
    e.preventDefault();
    const formData = new FormData(e.target);
    console.log("Feedback submitted:", Object.fromEntries(formData.entries()));
  });
</script>
```

## SolidJS Integration

```tsx
import { createSignal, createEffect } from "solid-js";

interface BlogPost {
  title: string;
  excerpt: string;
  content: string;
  tags: string;
}

export function BlogPostEditor() {
  const [post, setPost] = createSignal<BlogPost>({
    title: "",
    excerpt: "",
    content: "",
    tags: "",
  });

  const [errors, setErrors] = createSignal<{ [key: string]: string }>({});
  const [wordCount, setWordCount] = createSignal(0);

  let contentTextarea: HTMLModusWcTextareaElement;

  // Word count calculation
  createEffect(() => {
    const content = post().content;
    const words = content.trim() ? content.trim().split(/\s+/).length : 0;
    setWordCount(words);
  });

  const updateField = (field: keyof BlogPost, value: string) => {
    setPost((prev) => ({ ...prev, [field]: value }));

    // Clear error when user starts typing
    if (errors()[field]) {
      setErrors((prev) => ({ ...prev, [field]: "" }));
    }
  };

  const validateForm = (): boolean => {
    const newErrors: { [key: string]: string } = {};
    const currentPost = post();

    if (!currentPost.excerpt.trim()) {
      newErrors.excerpt = "Excerpt is required";
    } else if (currentPost.excerpt.length < 20) {
      newErrors.excerpt = "Excerpt must be at least 20 characters";
    }

    if (!currentPost.content.trim()) {
      newErrors.content = "Content is required";
    } else if (wordCount() < 50) {
      newErrors.content = "Content must be at least 50 words";
    }

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const publishPost = () => {
    if (!validateForm()) {
      alert("Please fix errors before publishing");
      return;
    }
    console.log("Post published:", post());
  };

  return (
    <div style="max-width: 800px; margin: 2rem auto; padding: 2rem;">
      <h1>Blog Post Editor</h1>

      <div style="display: flex; flex-direction: column; gap: 2rem;">
        {/* Title */}
        <input
          type="text"
          placeholder="Enter your blog post title..."
          value={post().title}
          onInput={(e) => updateField("title", e.currentTarget.value)}
          style="width: 100%; padding: 1rem; border: 1px solid var(--modus-wc-color-base-300); border-radius: 0.5rem; font-size: 1.5rem; font-weight: bold;"
        />

        {/* Excerpt */}
        <modus-wc-textarea
          label="Post Excerpt"
          prop:value={post().excerpt}
          prop:errorText={errors().excerpt}
          helper-text="A brief summary for search results and social shares"
          placeholder="Write a compelling excerpt for your blog post..."
          rows="3"
          max-length="300"
          required
          size="large"
          clearable
          on:valueChange={(e) => updateField("excerpt", e.detail)}
        ></modus-wc-textarea>

        {/* Main Content */}
        <modus-wc-textarea
          ref={contentTextarea}
          label="Post Content"
          prop:value={post().content}
          prop:errorText={errors().content}
          helper-text={`Write your blog post content here. Current word count: ${wordCount()}`}
          placeholder="Start writing your blog post content..."
          rows="15"
          required
          size="large"
          clearable
          spellcheck
          on:valueChange={(e) => updateField("content", e.detail)}
        ></modus-wc-textarea>

        {/* Tags */}
        <modus-wc-textarea
          label="Tags"
          prop:value={post().tags}
          helper-text="Separate tags with commas (e.g., javascript, web development, tutorial)"
          placeholder="javascript, web development, tutorial, programming"
          rows="2"
          size="medium"
          max-length="200"
          on:valueChange={(e) => updateField("tags", e.detail)}
        ></modus-wc-textarea>

        {/* State examples */}
        <div style="display: flex; flex-direction: column; gap: 1rem;">
          <modus-wc-textarea
            label="Read-only Content"
            value="This content is read-only and cannot be edited."
            read-only
            rows="2"
            size="medium"
          ></modus-wc-textarea>

          <modus-wc-textarea
            label="Disabled Textarea"
            value="This textarea is disabled."
            disabled
            rows="2"
            size="medium"
          ></modus-wc-textarea>
        </div>

        <div style="display: flex; gap: 1rem; justify-content: flex-end;">
          <modus-wc-button
            onClick={() =>
              setPost({ title: "", excerpt: "", content: "", tags: "" })
            }
            variant="outlined"
          >
            Clear All
          </modus-wc-button>

          <modus-wc-button onClick={publishPost} color="primary">
            Publish Post
          </modus-wc-button>
        </div>
      </div>
    </div>
  );
}
```

## Key Notes

- **Controlled value**: Update `value` programmatically and listen for `valueChange` to sync app state
- **Clear button**: `clearable` adds × icon; style color via `--modus-icon` CSS variable if needed
- **Row height**: `rows` sets initial height; allow vertical resize in CSS for user flexibility
- **Validation states**: Use only one of `errorText`, `validText`, or `helperText` to avoid message clutter
- **Accessibility**: Provide `aria-label` if no visible `label`; component handles `aria-invalid` automatically
- **Focus helper**: Call `focusInput()` after revealing hidden textarea for proper keyboard focus

> **SolidJS Tip**: Use `prop:` directives for reactive values: `prop:errorText={validationMessage()}`, `prop:value={textContent()}`
