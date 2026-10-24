# Instructions for coding agents

The author reviews and understands every line merged into this repository, so the agent assists; it does not decide.

## How to work

- Work in small steps: one model, one module or one function at a time. Stop and show the change before moving on.
- Explain any non-obvious choice in two or three sentences, and name the alternative you did not pick.
- Never invent a business rule, a KPI definition or a grain. If it is not in `docs/`, ask.
- Never commit, push, run `terraform apply` or `terraform destroy`, or delete data without explicit confirmation.
- Never write secrets, keys or credentials to files. Secrets live in Secret Manager.

## Project conventions

- Stack: GCP, BigQuery, Terraform, dbt Core, Python managed with `uv`.
- dbt layers: `stg_` (rename, cast, deduplicate, no business logic, views), `int_` (joins and pre-aggregations), `fct_` and `dim_` (marts, the only layer exposed to users).
- Every model has a one-sentence grain in its description, and its primary key is tested `unique` and `not_null`.
- Aggregate before joining when a join would fan out (for example payments to order items).
- Ratios are never stored in marts: keep additive components.
- SQL style follows `docs/conventions.md` and is checked by sqlfluff; lowercase keywords, CTEs over subqueries, explicit column lists.
- Terraform: one module per resource family under `infra/modules/`, environments under `infra/envs/dev` and `infra/envs/prod`, least-privilege service accounts.
- An object belongs to one tool only: Terraform owns datasets, IAM, buckets and services; dbt owns tables and views.

## Before proposing a change

- `dbt build --select <model>+` passes.
- `sqlfluff lint` passes.
- `terraform plan` shows only the expected changes.
- Documentation (`schema.yml`, `docs/`) is updated in the same change.
