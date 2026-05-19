---
name: chaoxing-assistant
description: Automate 学习通 (Chaoxing) online course tasks. Use when the user asks to 刷课, 挂机看视频, 自动刷学习通, auto-watch courses, automate Chaoxing, complete online course videos, handle in-video quizzes, or manage Xuexitong coursework. Triggers on any mention of 学习通, 超星, 刷网课, 自动看课, or course automation.
---

# Chaoxing Assistant

Automate 学习通 (Chaoxing / Xuexitong) online course tasks using opencli.

## Prerequisites

- `opencli` installed and Chrome extension connected (`opencli doctor` must show all OK)
- The user MUST already be logged into 学习通 in their Chrome browser

## Workflow

### Phase 1: Check Setup

```bash
opencli doctor
opencli chaoxing --help
```

If the extension is disconnected, tell the user to:
1. Open Chrome → `chrome://extensions`
2. Make sure "OpenCLI" extension is enabled
3. Run `opencli doctor` again

### Phase 2: Discover Courses & Tasks

Use `opencli-browser` to navigate and inspect:

```
opencli browser state     → check current page
opencli browser open https://i.mooc.chaoxing.com → navigate to course center
opencli browser state     → snapshot the course list
```

The course center at `i.mooc.chaoxing.com` shows all enrolled courses. Each course card links to its chapters/tasks page.

### Phase 3: Check Assignments & Exams

```bash
opencli chaoxing assignments --format json
opencli chaoxing exams --format json
```

This lists pending assignments and exams. These need the user to be on the relevant course page in Chrome.

### Phase 4: Watch Course Videos

For video-based courses, the flow on each video page:

1. **Navigate to video page** — use `opencli browser open <video-url>`
2. **Wait for player to load** — `opencli browser wait --text "视频" --timeout 10`
3. **Click play if needed** — `opencli browser find "播放按钮"` → `opencli browser click <target>`
4. **Handle popup confirmations** — Chaoxing pops up a "继续观看" or verification dialog during videos:
   ```
   opencli browser snapshot   → check for popup
   opencli browser click <popup-confirm-button>   → dismiss it
   ```
   Check for popups every 2-3 minutes during video playback.
5. **Handle in-video quiz questions** — Some videos pause and show quiz questions:
   ```
   opencli browser snapshot   → read the question and options
   opencli browser click <correct-answer>   → select and submit
   ```
   If unsure about the answer, choose the most reasonable option; Chaoxing usually allows retries.

### Phase 5: Track Progress

After completing a video or task:
```
opencli browser snapshot   → check the progress indicator
```

Chaoxing shows a green checkmark or percentage for completed items. Use `opencli browser state` to read the current status.

## Tips

- **Persistent session**: Use `--site-session persistent` with chaoxing commands to keep the logged-in state across commands
- **Background mode**: Add `--window background` to keep Chrome minimized during automation
- **Popup polling loop**: Videos longer than 10 minutes almost always trigger verification popups. Poll with `opencli browser snapshot` every 2 minutes
- **Rate limiting**: Don't switch between videos faster than a human would (keep at least 30s gap between operations)

## Emergency

If the agent gets stuck or the page changes unexpectedly:
- `opencli browser screenshot` — capture current state for debugging
- `opencli browser back` — go back to previous page
- `opencli browser console` — check for JavaScript errors
