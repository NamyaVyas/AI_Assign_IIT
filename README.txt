README

Files
  reasoning_agent.ipynb   the whole system (parsers, forward chaining, resolution, causal solver, evaluation, submission)
  report.pdf              short report, compiled from report.tex with pdflatex
  submission.csv          columns id,label with True or False
  README.txt              this file

How to run
  1. pip install datasets huggingface_hub pandas
  2. Open the notebook and run all cells. The first run downloads RuleTaker (tasksource/ruletaker),
     FOLIO (tasksource/folio) and CounterBench (read from its raw files if load_dataset fails).
  3. For the hidden test file set TEST_PATH at the top (csv, tsv, json, jsonl or parquet) and run all cells again.
     The file needs a context column and a query column (names like context/premises/given_info and question/conclusion are found automatically).
     An id column is used if present, otherwise the row number is the id.
  4. If TEST_PATH is None the submission.csv is made from the public splits instead.

Settings at the top of the notebook
  TEST_MODE   auto (default), cwa or owa
  MAX_EVAL    cap rows per dataset for a quick run
  FOL_TIMEOUT seconds per resolution proof
  LOCAL_DIR   folder with files named <dataset>_<split>.csv if there is no internet

Method in one paragraph
  English rules and facts are parsed into literals and horn rules by regex and token rules, then solved by forward chaining
  with unification and negation as failure. FOL formulas (FOLIO) are converted to clauses and solved by resolution
  refutation with set of support. CounterBench stories are parsed into a small causal model and the counterfactual
  question is answered by evaluating that model over the possible root assignments. No generative model is used anywhere.

Limits
  English that does not follow simple rule patterns (most of FOLIO) is only partly parsed, so those rows are mostly answered False.
  The way CounterBench questions are read (which reading is used per question form) is chosen on the public labels.
