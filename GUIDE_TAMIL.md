# AidProof — Intelligent Contract (ஒவ்வொரு step-ஆ)

## STEP 1 — Folder
`AidProof.zip`-ஐ right-click → **Extract All** → Location-ல `D:\` கொடுங்க. கடைசியா `D:\AidProof` ரெடியா இருக்கணும்.

## STEP 2 — GitHub push (Studio test-க்கு முன்னாடியே இதை செய்யணும்)
1. Browser-ல https://github.com/new open பண்ணுங்க. Repository name-ல `aidproof` type பண்ணுங்க. **Public** select பண்ணுங்க. README tick பண்ண வேண்டாம். **Create repository** கொடுங்க.
2. PowerShell-ல இந்த ஒரு வரியை ஓட்டுங்க:
```
cd D:\AidProof; git init; git add .; git commit -m "AidProof v1"; git branch -M main; git remote remove origin 2>$null; git remote add origin https://github.com/gurusankar55/aidproof.git; git push -u origin main
```
3. **Check:** இந்த link open ஆகணும் (push ஆகி 1–2 நிமிஷம் ஆகலாம்):
https://raw.githubusercontent.com/gurusankar55/aidproof/main/demo/report-ok.html

## STEP 3 — Studio-ல 2 deploy
`contracts\aid_proof.py` code முழுசையும் paste பண்ணுங்க. Header-ஐ மாத்த வேண்டாம்.
- `demo_mode` = **true** வச்சு Deploy → address-ஐ "DEMO"ன்னு எழுதி வச்சுக்கோங்க.
- `demo_mode` = **false** வச்சு Deploy → address-ஐ "LIVE"ன்னு எழுதி வச்சுக்கோங்க.

## STEP 4 — Studio test (DEMO contract-ல)
funding_for = `5` வச்சிருக்கேன். அதனால donate பண்ண **5 நிமிஷம்** இருக்கும்.

**முதல்ல 4 campaign-ஐயும் create பண்ணுங்க** (`create_campaign`). எல்லாத்துக்கும்: name = `Flood Relief Kits 2026`, cause = `Food and medicine kits for flood-hit families in Wards 4, 7 and 9`, tranches = `3`, funding_for = `5`, tranche_every = `3`. report_url மட்டும் மாறும்:
- Campaign 1: `https://raw.githubusercontent.com/gurusankar55/aidproof/main/demo/report-ok.html`
- Campaign 2: `https://raw.githubusercontent.com/gurusankar55/aidproof/main/demo/report-none.html`
- Campaign 3: `https://raw.githubusercontent.com/gurusankar55/aidproof/main/demo/report-offcause.html`
- Campaign 4: `https://raw.githubusercontent.com/gurusankar55/aidproof/main/demo/report-missing.html`

**அப்புறம் donate பண்ணுங்க** (5 நிமிஷத்துக்குள்ள): `donate(1)` Value `9` · `donate(2)` Value `6` · `donate(3)` Value `6` · `donate(4)` Value `6`

**create பண்ணி 5 நிமிஷம் கழிச்சு:** `review(1)`, `review(2)`, `review(3)`, `review(4)` → ஒவ்வொண்ணுலயும் `TRANCHE_1_RELEASED` வரணும்

**உடனே (அடுத்த 3 நிமிஷத்துக்குள்ள) Test C challenge:**
`challenge`: campaign_id = `3`, evidence_url = `https://raw.githubusercontent.com/gurusankar55/aidproof/main/demo/report-offcause.html`, note = `Money spent on a resort party, not flood kits`

**3 நிமிஷம் கழிச்சு:**
- `review(1)` → `TRANCHE_2_RELEASED` (Test A)
- `review(2)` → `FAILED_REFUNDS_OPEN` → `claim_refund(2)` → 4 GEN → `claim_refund(2)` மறுபடியும் → `Refund already claimed` error (Test B)
- `review(3)` → `FAILED_REFUNDS_OPEN` → `claim_refund(3)` (Test C)
- `review(4)` பண்ணுங்க. அப்புறம் **ஒவ்வொரு 1 நிமிஷத்துக்கும்** `review(4)` பண்ணுங்க, `FAILED_REFUNDS_OPEN` வர்ற வரைக்கும் (3–4 தடவை). அப்புறம் `get_campaign(4)`-ல `"verdict": "REPORT_PAGE_GONE"` தெரியணும் (Test D)

**இன்னும் 3 நிமிஷம் கழிச்சு:** `review(1)` → `TRANCHE_3_RELEASED`. **இன்னும் 3 நிமிஷம் கழிச்சு:** `review(1)` → `COMPLETED` (Test A முடிஞ்சது)

"Next review is not due yet" அல்லது "Wait" error வந்தா, 1 நிமிஷம் கழிச்சு மறுபடியும் பண்ணுங்க. அது bug இல்லை.
Screenshots → `D:\AidProof\evidence\`: `A_completed.png`, `B_refund.png`, `B_double.png`, `C_challenge_fail.png`, `D_pagegone.png`.

## STEP 5 — Addresses + screenshots push
`DEMO_ADDR`, `LIVE_ADDR` இடத்துல உங்க address-களை போட்டு ஓட்டுங்க:
```
cd D:\AidProof; (Get-Content SUBMIT_TEXT.md) -replace 'Demo contract \(Studio\): 0x_+','Demo contract (Studio): DEMO_ADDR' -replace 'Live contract \(Studio\): 0x_+','Live contract (Studio): LIVE_ADDR' | Set-Content SUBMIT_TEXT.md; git add .; git commit -m "Studio test evidence and addresses"; git push
```

## STEP 6 — Portal submit
**Intelligent Contracts** category-ல submit பண்ணுங்க. `SUBMIT_TEXT.md`-ல இருக்கிற Title, Description, GitHub link, contract address-ஐ copy பண்ணுங்க.
