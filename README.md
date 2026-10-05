# Rainbow Readers 2.0

**AppADay 151** | Educational (E)

Live: https://augustineiacopelli.github.io/appaday-151-rainbow-readers-2/
Portfolio: https://augustineiacopelli.github.io/appaday/

## Purpose

Rainbow Readers 2.0 is an offline phonics and sight word practice app for early readers in grades 1 through 3. A short placement probe finds the right starting step, and six mini games then practice that step with spoken prompts, gentle correction, and a rainbow that fills with stars to unlock new friends. It runs from a single `index.html` with no network, no fonts to download, and no accounts, so it works on a tablet, a laptop, a TV, or a RetroPie box with a game controller.

## Curriculum bands

The 23 steps run in order, and each one also mixes in the Dolch sight words for its band.

**Grade 1** covers letter sounds, short a, e, i, o, and u words, mixed short vowels, digraphs (sh, ch, th, wh, ck), starting blends, ending blends, and silent e. Sight words come from the Dolch pre primer and primer lists.

**Grade 2** covers vowel teams, bossy r words, the diphthongs oi, oy, ou, and ow, contractions, compound words, and the endings s, ed, and ing. Sight words come from the Dolch first grade and second grade lists, and short reading passages with comprehension questions unlock here.

**Grade 3** covers two syllable words, prefixes, suffixes, homophones (each spoken inside a short sentence so the meaning is clear), tricky spellings, and a final passages step. Sight words come from the Dolch third grade list.

## The six games

Every game runs a round of eight items and never fails out. A correct first try plays a chime and sends a star flying to the counter. A miss plays a soft bloop, repeats the word, and gently shakes the right answer so the reader can find it.

**Bubble Pop** shows four to six floating word bubbles; pop the one you hear. **Letter Build** fills empty slots by choosing letter tiles in order. **Bingo** uses a 3 by 3 card and ends as soon as a line is complete. **Word Sort** shows a word to read and two or three pattern bins to sort it into. **Sentence Builder** unlocks at the Grade 1 digraph step and asks for word tiles in sentence order. **Passage Reader** appears in Grades 2 and 3, offers a highlighted read along with tap to hear any word, and follows with three comprehension questions (reading earns two stars and each first try answer earns two more).

## Placement and advancement

Placement is an adaptive probe of up to twelve items. It starts in the middle of Grade 1, moves up a step after two correct answers in a row, steps back after a miss, and stops after two misses at the same step or after twelve items. The reader starts at the step where it stops.

A step is mastered after at least 15 attempts with 85 percent or better over the most recent 20. Mastery awards a sticker for the Shelf and moves to the next step. There is never automatic demotion. If accuracy on a step falls below 60 percent, the Play button chooses an easier game for that step until accuracy recovers. Words that are missed more often, or have not been seen for a while, come up more often.

## Rewards

A round earns up to eight stars. Every seven stars fills the rainbow and unlocks the next of twelve original characters. Characters and stickers live on the Shelf.

## Controls

**Touch or mouse:** tap any button.

**Keyboard:** arrow keys move the rainbow focus ring, Enter or Space chooses, and Escape or Backspace goes back.

**Gamepad:** the d pad or left stick moves, A chooses, B goes back, and Start always returns Home. The app reads the standard Gamepad API mapping (buttons 12 to 15 for the d pad, 0 for A, 1 for B, 9 for Start). Press any button once after the page loads so the browser notices the controller.

## Parent area

Hold the gear in the top corner for three seconds, or hold B on a controller (or Escape on a keyboard) for three seconds. Answer the two digit addition problem to enter. Inside, a grown up can jump to any band and step, edit the word list for the current step (one word per line, at least four words, with Restore default to undo), turn speech and sound effects on or off, change the speaking rate and voice, rerun placement, and reset all progress. Reset needs two taps: the first arms the button and the second, within four seconds, erases everything.

## Sound and visual mode

Speech uses the browser's built in voices, prefers a US English voice, and defaults to a rate of 0.8. If no voices are available, or speech is turned off in the Parent area, the app switches to visual mode automatically. Visual mode shows each prompt on screen (letters appear as capitals to match to small letters), shows the model word or sentence in the build games, and still runs the timed highlight in Passage Reader. Sound effects are synthesized with the Web Audio API and start after the first tap or key press.

## RetroPie setup

Copy `index.html` onto the Pi (for example `/home/pi/RetroPie/roms/ports/rainbow-readers/index.html`) and add a launcher script in the ports folder, such as `/home/pi/RetroPie/roms/ports/Rainbow Readers.sh`:

```sh
#!/bin/bash
chromium-browser --kiosk --noerrdialogs --disable-infobars \
  --autoplay-policy=no-user-gesture-required \
  --enable-speech-dispatcher \
  file:///home/pi/RetroPie/roms/ports/rainbow-readers/index.html
```

Make it executable with `chmod +x`, then restart EmulationStation so it appears under Ports. On some images the browser command is `chromium` instead of `chromium-browser`. Press Alt+F4 or use a keyboard to leave kiosk mode.

**Missing voices fix.** Chromium on Linux has no voices of its own, so the app will open in visual mode until speech is set up. Install the speech packages and keep the `--enable-speech-dispatcher` flag in the launcher:

```sh
sudo apt update
sudo apt install speech-dispatcher espeak-ng speech-dispatcher-espeak-ng
```

Reboot, launch from the Ports menu, and open the Parent area. The Voice button should now show an English voice, and Test should speak.

## Saved data and reset

All progress lives in one localStorage key, `rr2.state`, as a versioned object holding the current step, placement flag, recent results per step, per word stats, stars, unlocked friends, stickers, settings, and any custom word lists. Nothing leaves the device.

To start over, use Reset progress in the Parent area (tap twice). To clear it by hand, open the browser console and run `localStorage.removeItem('rr2.state')`, then reload.

## Tech

One ASCII clean `index.html` with inline CSS and JavaScript, a system font stack, inline SVG art, the Web Speech API, the Web Audio API, and the Gamepad API. No libraries, no build step, no network requests.
