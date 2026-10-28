# Repository guidance

## Design priorities

Prefer the smallest coherent implementation that satisfies the task.
Evaluate complexity across configuration, rules, scripts, helpers, tests,
and documentation together.

Before adding an abstraction, configuration option, script, or intermediate
file, check whether an existing component can naturally own the behavior.
Do not introduce infrastructure for hypothetical future requirements.

## Configuration

Expose scientific assumptions and meaningful user choices.
Keep storage formats, compression, tiling, plotting styles, and routine
execution details internal unless users have a demonstrated need to control them.

Read required settings directly after schema validation.
Do not silently supply missing user settings through defaults or chained
mapping lookups.

Do not promote an internal constant into configuration without agreement.
Report undocumented scientific fallback values for assessment.

Keep configuration grouped by responsibility. Source-specific processing
settings belong with that source. Country proxies belong under data_proxies.

Source selection must be explicit. Do not silently switch sources or trigger
expensive rebuilding when a selected source is unavailable or incompatible.

## Snakemake rules and Python scripts

Keep DAG construction, checkpoint resolution, and rule references in .smk files.
Keep data processing and reusable calculations in Python modules.

Declare inputs that the script consumes or that enforce a necessary dependency.
Explain non-obvious ordering dependencies.

Keep downloads separate from processing when this enables source-cache reuse.
Use generic download handling for ordinary transfers. Add source-specific
download code only when acquisition requires distinct behavior.

Do not create a script for every rule automatically. Equally, avoid unrelated
operations collected behind a large mode dispatcher.

Use helpers for substantive shared behavior or non-obvious logic.
Avoid wrappers that only return another helper, rule output, or configuration
value. Prefer direct references when they are clearer.

Do not create dummy files or workflow rules solely to collect report figures.
Request real figure outputs from local tests and write generated reports under
results/.

## Validation and removal

Define source schemas centrally and validate external data at ingestion.
Reuse already loaded data or processing loops where practical.

Do not add repeated validation of generated intermediates solely to prove
that upstream processing worked. Preserve necessary boundary and compatibility
checks, and distinguish output-format definitions from validators.

When removing an option or mode, remove its branches, tags, helpers, schema
entries, tests, and documentation. Do not retain a one-value selector or
constant metadata field that only records the removed distinction.

## Comments and documentation

Explain why code is necessary, scientific assumptions, units, invariants,
source quirks, and non-obvious performance constraints.

Do not narrate obvious operations or describe abandoned implementations.
Avoid migration commentary such as "formerly", "now", or "no longer" in
ordinary documentation and code comments.

Do not repeat example configuration values, release dates, or internal
constants in prose. Link to their authoritative definition.

Keep configuration examples annotated where interpretation matters.
Preserve scientific references, attribution, and explanations of assumptions.

Describe current behavior and verify claims against the implementation.
Semicolons are not inherently a problem; redundant or stale content is.

## Verification

Match verification to the change. Use schema validation, focused checks,
formatting, and Snakemake dry runs where appropriate.

For comment-only changes, verify that executable code and configuration
values remain unchanged. Do not run expensive workflows unnecessarily.

Review the final diff for redundant helpers, unused inputs, stale references,
and documentation that duplicates configuration or code.

## Testing

If you want to develop dedicated tests for the code you write, store these in temp/tests so they are not tracked by git. 