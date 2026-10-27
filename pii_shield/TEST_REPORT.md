# Verification report — detection update

Executed in the build environment on 2026-10-27:

- `python -m compileall -q .` — passed.
- `pytest -q` — **45 passed, 1 skipped**. The skipped test is environment-dependent Streamlit AppTest.
- `evaluation.evaluate('rule-only')` — ran over **23 synthetic examples**, including field-label examples from the 11 IndicNER language scripts. This is a smoke-test dataset, not a real-world benchmark.
- `python evaluate.py` — the separate built-in synthetic email evaluation ran successfully over 120 samples.
- A text-processing smoke test confirmed identifier/name detection, a contextual-risk report, detector-stack status, and redaction output.

**Not verified in this environment:** live IndicNER inference because the model is gated and weights were not cached; full Presidio AnalyzerEngine because `presidio-analyzer`/`en_core_web_sm` were not installed in this build runtime; live Ollama agent run; Streamlit AppTest; and Tesseract OCR quality on representative documents. The code includes graceful fallbacks, but these integrations should be tested in the target environment after setup.

No claim of improved real-world precision/recall is made until a labelled held-out dataset is evaluated.

- Added unit tests for space/hyphen/Devanagari mobile detection, privacy-policy recommendations, value-free review prioritization, risk-score components, and mutation-suite coverage.
- `evaluation.evaluate_robustness()` — **10/10 synthetic mutation smoke cases passed** in this runtime after the phone-number normalization update. This is a small synthetic smoke test, not a benchmark; model-dependent NER results can differ when IndicNER weights are installed.
- New risk-policy and review-priority output is advisory and does not change the actual redaction strategy.
