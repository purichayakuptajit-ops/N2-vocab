# JLPT N2 Vocabulary Mobile Tool

A mobile-friendly, browser-based JLPT N2 vocabulary quiz (1,434 words).

Files:
- `index.html`          — the app; the vocabulary is built into this file
- `vocab.json`          — the source vocabulary database (editable, see below)
- `index.original.html` — the earlier version that loaded vocab.json over the network
- `README.txt`          — this file

## Use on a computer
Double-click `index.html`. Nothing else is needed — no web server, and the file
works on its own even if you move it somewhere else.

## Put it on your phone
Email or upload `index.html` to yourself, open it, and use the browser's
**Add to Home Screen** option. Or upload the folder to a static host such as
GitHub Pages. Only `index.html` is required.

## Editing the vocabulary
`vocab.json` is the master list. After editing it, re-embed it into the app by
running this in the folder:

    python3 - <<'PY'
    import json, io, re
    html = io.open('index.html', encoding='utf-8').read()
    data = json.load(io.open('vocab.json', encoding='utf-8'))
    payload = json.dumps(data, ensure_ascii=False, separators=(',', ':')).replace('</', '<\\/')
    html = re.sub(r'const VOCAB_DATA=.*?;\n', 'const VOCAB_DATA=' + payload + ';\n', html, count=1, flags=re.S)
    io.open('index.html', 'w', encoding='utf-8', newline='\n').write(html)
    PY

Each entry needs a unique `id`, plus `kanji` (may be empty), `reading` and
`meaning`. The `id` is what progress is tracked against, so keep existing ids
stable or past progress for those words is lost.

## Features
- Japanese → English, English → Japanese, or random direction
- 5 / 10 / 20 / 50 / All questions
- Adaptive review (missed words return sooner: 1, 3, 7, 14, 30, 60 days)
- Accuracy, seen, due and mastered counts
- Browse/search all vocabulary
- Export progress backup

## Important
Progress is stored in the browser's localStorage on the device you study on.
It is not synchronized between devices, and clearing site data erases it —
use **Export progress** for a backup.
