# Isometric Neck Trainer

An interval timer for isometric neck exercises. One HTML file, no dependencies, no build step, no backend. Works offline, in three languages, and never sends anything anywhere.

**[Live demo →](https://igorswift.github.io/isometric-neck-trainer/)**

<img width="482" height="655" alt="image" src="https://github.com/user-attachments/assets/32d9374f-3e82-49ef-a031-b6038fcd76e5" />


---

## Why I built it

I needed to do a set of isometric neck exercises daily, on a schedule, for several weeks. Every tool I tried was the wrong shape for the job:

- Generic interval timers (HIIT apps) count intervals but don't tell you *what to do* during them, so you end up holding a phone in one hand and a printout in the other.
- Physio apps wanted an account, a subscription, and permission to track me — for a timer.
- Paper instructions don't count reps, and it turns out counting to ten while pressing your head into your palm is a reliable way to lose count.

So the requirement was narrow and clear: show one exercise at a time, run the clock, cue the transitions by sound so I don't have to look at the screen, and stay out of the way. That narrowness is the whole design.

It's a side project — a real tool I use, built small enough to finish.

---

## Features

**Guided sessions.** Five exercises: four directional presses and a chin tuck. Each screen shows one exercise with its instruction, a countdown, and a rep counter. Full session runs all five; short session skips the chin tuck. Individual exercises can be toggled on and off.

**Audio cues.** Distinct tones for hold start, rest start, and exercise complete, generated with the Web Audio API — no audio files to load. The point is being able to run a session without watching the screen, which matters when the exercise involves not moving your head.

**Configurable intervals.** Hold 5–15s, rest 3–10s, 3–12 reps. Optional auto-advance between exercises. Everything persists to `localStorage`.

**Streak tracking.** Current streak, sessions in the last seven days, and a seven-day activity strip. Adherence is the hard part of any exercise protocol, so the app makes the streak the most prominent number on the screen.

**Three languages.** Russian, English, Spanish — including all exercise instructions and safety copy. Detected from the browser on first load, switchable at any time, persisted.

**Screen stays awake.** The Screen Wake Lock API keeps the display on for the length of a session and releases it the moment the session ends, so nothing is held longer than needed. The lock is re-acquired automatically when you switch back to the tab, since browsers drop it whenever a page is hidden. Where the API is unavailable (iOS before 16.4, some Firefox builds) or the request is denied by battery saver, the app degrades silently — a small indicator under the controls tells you whether the lock is actually held rather than assuming it worked.

**Keyboard control.** `Space` pauses, `←`/`→` move between exercises, `R` restarts the current one.

**Accessibility and system preferences.** Automatic light/dark via `prefers-color-scheme`, `prefers-reduced-motion` respected, visible focus rings, ARIA live region on the timer, semantic labels throughout.

**Privacy by construction.** No backend, no analytics, no cookies, no network calls after the fonts load. All state lives in `localStorage` on the device. This isn't a policy — there's nowhere for data to go.

---

## Design decisions worth explaining

**Single file, zero dependencies.** The whole app is ~30 KB of HTML, CSS, and vanilla JS. No framework, no bundler, no `node_modules`. For something this small, a build step would be more moving parts than the app itself — and it means deployment is copying one file to any static host.

**A disclaimer gate, not a footnote.** The app blocks on a modal listing specific contraindications before first use, and the disclaimer stays reachable in the footer. Anything that instructs a person to load their cervical spine should say plainly what it is and isn't, up front. See below.

**Safety language is part of the UI, not decoration.** The instruction copy consistently says *gentle pressure*, *30–50% effort*, *the head stays still* — and never *harder* or *push through*. On a screen that shows a countdown and a rep counter, the interface is quietly encouraging effort; the copy has to pull the other way.

**No personal health data in the product.** Earlier versions of this tool (built for myself) had my diagnosis and appointment dates hard-coded into the UI. Stripping that out was the main change in making it public — the app is now a generic protocol runner, with nothing clinical baked in.

**Localization keys, not scattered strings.** All copy lives in one `I18N` object keyed by language, including per-exercise names and descriptions. Adding a language means adding one object, not hunting through markup.

---

## Medical disclaimer

**This is a learning side project, not a medical device.** It does not diagnose anything, does not adapt to any condition, and does not replace a doctor or physiotherapist.

Isometric neck work is not appropriate for everyone. Consult a clinician before using it if you have a disc herniation, spinal stenosis, a recent neck injury or surgery, uncontrolled high blood pressure, or unexplained dizziness. Stop immediately on sharp pain, numbness, tingling, weakness in the arms, or pain radiating down an arm.

The default intervals are generic starting values, not a prescription.

---

## Tech

| | |
|---|---|
| **Stack** | HTML, CSS, vanilla JavaScript (ES2020) |
| **Dependencies** | none |
| **Audio** | Web Audio API (`OscillatorNode`) |
| **Persistence** | `localStorage` |
| **Screen** | Screen Wake Lock API with graceful degradation |
| **i18n** | hand-rolled key/value dictionaries, 3 locales |
| **Theming** | CSS custom properties + `prefers-color-scheme` |
| **Tests** | jsdom suites — i18n parity, mode switching, session lifecycle, timing transitions, streak logging, keyboard control, wake-lock acquire/release/fallback |
| **Hosting** | GitHub Pages (any static host works) |

---

## Run it

```bash
git clone https://github.com/igorswift/isometric-neck-trainer.git
cd isometric-neck-trainer
open index.html          # that's it — no install, no server
```

Deploy: push to GitHub, then **Settings → Pages → Branch: `main` / `(root)` → Save**.

---

## Roadmap

- [ ] Optional voice cues via the Web Speech API for eyes-free sessions
- [ ] Session history export as CSV
- [ ] Installable PWA with an offline manifest
- [ ] Configurable custom exercises

---

## License

MIT — see [LICENSE](LICENSE). Use it, fork it, adapt it.

Built by [Igor Triandafilov](https://www.linkedin.com/in/igor-triandafilov/).
