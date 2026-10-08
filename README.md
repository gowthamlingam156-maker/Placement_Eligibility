# PlacementPulse

A small Flask web app for students preparing for campus placements. Fill one form and get:

1. **Eligibility check** against company rules (10th / 12th / UG %, standing arrears, history of arrears), with the exact reason for each "not eligible".
2. **Skill-gap analysis**: paste your resume and a job description, see a weighted match score, the skills you have, and the ones you are missing.
3. **Day-by-day study plan** built from the missing skills and the number of days you have left.

No database, no API keys, no ML model to download. Rules and skills are plain JSON, so anyone can edit them.

## Why this exists

Most placement tools are either job boards or generic resume scorers. Students in India usually get stuck on two practical questions first: *"Am I even eligible given my arrears?"* and *"What exactly should I study in the days I have?"* PlacementPulse answers both in one place.

## Quick start

Requires Python 3.9 or higher.

**Windows (CMD or VS Code terminal)**

```bash
git clone https://github.com/<your-username>/placementpulse.git
cd placementpulse
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
python app.py
```

**Mac / Linux**

```bash
git clone https://github.com/<your-username>/placementpulse.git
cd placementpulse
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python app.py
```

Open http://127.0.0.1:5000 and click **Load demo data**, then **Analyze**. Press `Ctrl + C` in the terminal to stop.

### Troubleshooting

- `python` not recognized: try `py app.py`, or reinstall Python with "Add to PATH" ticked.
- `No module named 'flask'`: activate the virtual environment, then run `pip install -r requirements.txt`.
- `No module named 'engine'`: run the command from the folder that contains `app.py`.
- Port 5000 busy: change the last line of `app.py` to `app.run(debug=True, port=5001)`.

## Run the tests

```bash
python -m unittest discover tests -v
```

## JSON API

```bash
curl -X POST http://127.0.0.1:5000/api/analyze -H "Content-Type: application/json" \
  -d '{"tenth":85,"twelfth":80,"ug":72,"standing_arrears":0,"history_arrears":2,
       "resume":"Python SQL Flask","jd":"Java SQL Docker AWS","days":10}'
```

## Project structure

```
app.py               Flask routes
engine/eligibility.py   rule checker
engine/skillgap.py      resume vs job description matcher
engine/planner.py       study-plan generator
data/companies.json     company rules (SAMPLE values, please verify and edit)
data/skills.json        skills, aliases, topics, practice tasks
templates/ static/      UI
tests/                  unit tests
```

## Customize

- **Add a company:** add an object to `data/companies.json`. Use `null` for `max_history_arrears` if history is not restricted.
- **Add a skill:** add an entry to `data/skills.json` with aliases, days, topics and a practice task.

## Important note

The company criteria shipped here are placeholders. Always confirm real eligibility on the official careers page before relying on the result.

## Roadmap ideas

- Upload resume as PDF
- Save progress and tick off plan days
- Export plan as PDF or calendar file
- Add aptitude and HR question sets per skill

## License

MIT
