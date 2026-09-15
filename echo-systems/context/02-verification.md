# Claim-by-claim check against ECHO-LIT

| Claim in the TeX | Source of truth |
|---|---|
| API has no ML runtime | `docker-compose.yml` api image `ROLE: api`; `requirements-api.txt` vs `requirements-worker.txt`; `fr7_planning.py` / `fairness_service.py` docstrings |
| Four Redis DBs | `docker-compose.yml` `REDIS_URL` /0, `JOB_REDIS_URL` /1, `CELERY_BROKER_URL` /2, `CELERY_RESULT_BACKEND` /3 |
| Queue routing | `Backend/app/core/celery_app.py` `queue_for` |
| Job operations | `Backend/app/schemas/jobs.py` `JobOperation` |
| Built-in models + capabilities | `Backend/app/core/model_catalog.py` |
| Custom model kinds | `custom_model_validation.py` (`is_encoder_decoder`, `ForCTC`, classification) |
| `trust_remote_code=False` | `custom_model_validation.py` |
| Datasets | `dataset_service.py` `DATASET_PATHS` |
| Layer-probe metrics | `probing_service.py` module docstring |
| Fairness bootstrap = percentile | `fairness_metrics_service.py` module docstring |
| Grounding lift | `grounding_service.py` `grounding_metrics` |
| FR-7 via `/analyses/linguistic-vs-acoustic` | `api/routes/analyses.py` |
| FR-10 via `/analyses/fairness` | same; `JobCreateRequest` rejects those ops on `/jobs` |
| J-lens decoder-only, six layers | `jacobian_lens_service.py`; fitted `metadata.json` `layer_count: 6` |
| Saliency job methods | `SaliencyParameters`: `gradcam \| lime \| shap` |
| IG / LRP in service | `saliency_service.py` imports |
| Perturbation job types | `PerturbationSpec`: noise, time_masking, pitch_shift, time_stretch |
| Eager attention | `model_loader_service.py` `attn_implementation="eager"` |
| HDBSCAN clustering | `clustering_service.py` |
| Session cookie | `session.py` `sid`, HttpOnly, SameSite=lax |
| Object storage 24 h | `ARCHITECTURE.md`; `s3-lifecycle.json` |
| J-lens closed-form test | `tests/test_jacobian_lens.py` relative Frobenius `< 0.1` |
