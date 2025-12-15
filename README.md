# 🟨🖼️✨ AI Image Masker (Anki Add-on)

**AI Image Masker** is an Image Occlusion–style add-on for Anki.  
It lets you pick an image, draw masks (occlusions) in a built-in editor, and generate review cards from those masks.

It also includes **optional AI features**:
- 🤖 **AI mask suggestions** (auto-propose regions to hide)
- 🧠 **AI metadata generation** (auto-generate a Title / Explanation)

> Works best for anatomy diagrams, histology, pathology slides, charts, maps, and any “label-the-picture” learning.

---

## 🌈 What it does

### ✅ Core (no AI required)
- Creates (or updates) an **Image Masker** note type with the required fields and templates
- Opens a WebView-based editor where you can:
  - pick an image from file
  - (optionally) import an image from the clipboard
  - draw masks on top of the image
  - export those masks into Anki notes/cards

### 🤖 Optional AI
If enabled in config, the add-on can:
- suggest masks automatically (**AI suggest…**)
- generate **Title** / **Explanation** text from the image

AI is **off by default**.

---

## 🚀 Quick Start

### 1) Install
Install via AnkiWeb (recommended) or place the add-on folder in your `addons21` directory, then restart Anki.

### 2) Create / edit an Image Masker note
In the **Add Cards editor**, you will see a button (configurable) that opens the Image Masker editor:

- **Create / edit image occlusion notes** (tooltip)

Inside the editor:
1. Click **Pick image…** and choose a file
2. Draw masks (occlusions)
3. Click **Create cards**

The add-on will create/update notes and you can start reviewing.

---

## 🧩 How cards are stored

The add-on uses a dedicated note type named:

- **Image Masker** (default, configurable)

Key fields used by the note type include:
- `ImageFile` / `ImageHTML`
- `MasksB64` (base64-encoded mask payload)
- `ActiveIndex`, `GroupId`
- `Title`, `Explanation`, `No`, `SortKey`
- `MaskLabel` (for labeling masks / groups)

To improve compatibility with Anki’s **Media Check**, the add-on migrates existing notes so that `ImageHTML` contains an `<img src="...">` reference to your media file (best-effort, capped per session).

---

## 🧠 AI Features (optional)

### ✅ AI mask suggestions
When enabled, the editor button **AI suggest…** becomes available.

- Provider: `openai` or `gemini`
- The image is **downscaled for AI** (does not modify the original media file)

If AI is disabled, you’ll see:
- “AI is disabled in config (04_ai.enable_ai).”

### ✅ AI Title / Explanation generation
When enabled, the add-on can generate:
- `Title`
- `Explanation`

If metadata AI is disabled, you’ll see:
- “Metadata AI is disabled in config (04_ai.enable_metadata_ai).”

---

## 🎛️ Settings (Config GUI)

Open:
- **Tools → Add-ons → AI Image Masker → Config**

This add-on uses **nested config keys only** (legacy/flat keys are not supported).

### 01_general
- `enabled` (default: true)
- `note_type_name` (default: `"Image Masker"`)
- `always_update_note_type_templates` (kept as an option; safe)
- `auto_open_browser_after_create` (reserved)

### 02_editor
- `add_editor_button` (default: true)
- `editor_button_label` (default: icon)
- `editor_button_tooltip`

### 03_masks (colors & outline)
- `default_fill_front` (active mask fill)
- `default_fill_other` (other masks fill)
- `default_stroke` (outline color)
- `outline_width_px`

### 04_ai
- `enable_ai` (default: false)
- `enable_metadata_ai` (default: false)
- `provider` (`openai` / `gemini`)
- `max_suggestions`

Provider-specific blocks:

**OpenAI**
- `api_key_env` (default: `OPENAI_API_KEY`)
- `base_url` (default: `https://api.openai.com/v1/responses`)
- `model` (default: `gpt-4.1-mini`)
- `timeout_sec`
- `max_output_tokens`

**Gemini**
- `api_key_env` (default: `GEMINI_API_KEY`)
- `endpoint` (default uses `{model}` template)
- `model` (default: `gemini-2.5-flash`)
- `timeout_sec`
- `max_output_tokens`

### 05_image_processing
Controls image scaling/quality for different purposes:

- `display` (image shown in the editor WebView)
- `ai_suggest` (image sent to AI for mask suggestions)
- `ai_metadata` (image sent to AI for title/explanation)

Each has:
- `max_side_px`
- `jpeg_quality`

---

## 🛠️ Troubleshooting

### “Image Masker is disabled in config.”
Enable:
- `01_general.enabled = true`

### “This note is not an Image Masker note.”
You opened the editor on a note that isn’t using the Image Masker note type.

### “Pick an image first.”
Select an image before exporting/creating cards.

### Clipboard import doesn’t work
The add-on can wait for a clipboard image:
- “📋 Waiting for next clipboard image… (copy an image to continue)”

Some OS/app combinations do not provide images to the clipboard in a compatible way.

### AI buttons are disabled
Enable:
- `04_ai.enable_ai` for mask suggestions
- `04_ai.enable_metadata_ai` for title/explanation generation

Also ensure your environment variable API key is set.

---

## 🔒 Privacy

If AI is enabled, the add-on may send a **downscaled copy** of your image to the selected provider (OpenAI / Google Gemini).  
Do not use sensitive or private images unless you understand and accept the provider’s data handling policies.

If AI is disabled, everything stays local.

---

## 📜 License
See `LICENSE` in this repository.
