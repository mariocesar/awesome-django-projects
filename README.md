# Welcome to Awesome Django Projects!

This is a collection of cool projects made using Django. Each project has been analyzed and presented to highlight what makes it interesting, focusing on unique practices and tools employed.

## Learning Through Examples

Tutorials teach you Django. Real codebases teach you how Django survives contact with production — how a project structures settings across a dozen deploy targets, where it draws the line between fat models and service layers, what it reaches for when the ORM stops being enough, and how it handles migrations on a database nobody can afford to take offline.

Every project here is open source and actively maintained. Each entry notes what the project does, the stack it runs on, and what specifically is worth reading the source for.

## Applications

Complete, deployable products built on Django.

### Paperless-ngx

A community-supported supercharged document management system: scan, index and archive all your documents.

- **Repo:** https://github.com/paperless-ngx/paperless-ngx
- **Stack:** Django 5.2, DRF + drf-spectacular, Celery + Redis, Angular frontend, PostgreSQL / MariaDB / SQLite
- **Worth studying for:** Full-text search on **tantivy** rather than the usual Whoosh or Postgres `SearchVector`. Object-level permissions via `django-guardian`, audit trails via `django-auditlog`, and ORM-level query caching via `django-cachalot` — a good reference for what a mature permissions-and-audit layer looks like. Auth is `django-allauth` with MFA and social accounts; WebSockets via Channels.

### NetBox

The premier source of truth powering network automation — combining IP address management (IPAM) and datacenter infrastructure management (DCIM) with rich APIs and extensions.

- **Repo:** https://github.com/netbox-community/netbox
- **Stack:** Django + DRF + GraphQL, PostgreSQL 14+, Redis 5+, Python 3.12–3.14, server-rendered UI
- **Worth studying for:** One of the most thoroughly designed **plugin architectures** in the Django world — third-party plugins register models, views, tables and API endpoints against documented extension points. Also worth reading: automatic change logging on every object, user-defined custom fields on core models, and Jinja2-based device config rendering.

### TacticalRMM

A remote monitoring & management tool, built with Django, Vue and Go.

- **Repo:** https://github.com/amidaware/tacticalrmm
- **License:** ⚠️ **Source-available, not OSI open source.** The Tactical RMM License forbids offering it as a SaaS or commercial hosted product without written permission from AmidaWare LLC. In-house use and managing your own customers' networks are permitted.
- **Stack:** Django + DRF, Vue.js frontend, Go agent, NATS for agent transport, MeshCentral for remote desktop
- **Worth studying for:** A Django backend coordinating thousands of **Go agents over NATS** instead of HTTP polling — the `natsapi` app is a rare example of Django serving a non-HTTP, high-fanout transport. Read it for the fleet-orchestration patterns, but note the license before building on it.

### LibrePhotos

A self-hosted, open-source photo management service with face recognition, object detection and semantic search, powered by machine learning.

- **Repo:** https://github.com/LibrePhotos/librephotos
- **Stack:** Django 5 + DRF, PostgreSQL, Django-Q2 task queue, React 18 + TypeScript + Vite + Mantine
- **Worth studying for:** Running **heavy ML pipelines** (`face_recognition`, scikit-learn, hdbscan clustering, im2txt captioning, places365 scene detection) as background jobs without blocking request/response. Also a clean case study in **monorepo consolidation** — five separate repos merged while preserving full commit history.

### Django Packages

A directory of reusable apps, sites, tools, and more for your Django projects.

- **Repo:** https://github.com/djangopackages/djangopackages
- **Stack:** Django, PostgreSQL, Tailwind CSS, `uv` for dependency management, pytest, Docker Compose
- **Worth studying for:** **Long-lived API versioning done in the open** — `apiv3` and `apiv4` coexist in the tree, showing how a public API evolves without breaking consumers. Also the comparison-grid data model and the PyPI/GitHub/Bitbucket metadata ingestion pipeline. Notable for being a well-maintained community project on genuinely modern tooling.

### Docs

An open-source text editor: web-native, made for real-time collaboration, with cleanly structured documents and sub-documents and full ownership of your data. An open source alternative to Notion or Outline.

- **Repo:** https://github.com/suitenumerique/docs
- **Maintainers:** A joint initiative of the French government (DINUM) and German government (ZenDiS)
- **Stack:** Django + DRF + `django-configurations`, PostgreSQL (psycopg 3), Celery + Redis, S3 via `django-storages`; Next.js frontend with Yjs, ProseMirror, BlockNote and Hocuspocus for CRDT collaboration
- **Worth studying for:** How a Django REST backend sits **behind a CRDT collaboration layer** — Django owns identity, permissions and persistence while Hocuspocus/Yjs own the live document state. Also a strong public-sector reference stack: OIDC auth via `mozilla-django-oidc`, feature flags via `django-waffle`, CSP headers via `django-csp`, hierarchical sub-documents via `django-treebeard`.

### PostHog

The open source platform for building self-driving products — product analytics, session replay, feature flags, experiments and error tracking.

- **Repo:** https://github.com/PostHog/posthog
- **License:** MIT, with a separately-licensed proprietary `ee/` directory
- **Stack:** Django backend, **ClickHouse** for analytics + PostgreSQL for operational data, React/TypeScript frontend, plus Rust and Node services in the monorepo
- **Worth studying for:** Django as the **control plane over a columnar analytics database** — the ORM handles users, teams and configuration while event queries go to ClickHouse. One of the clearest large-scale examples of Django coexisting with a purpose-built query engine, and of an open-core split kept honest inside a single repo.

### Zulip

Open-source team chat with unique topic-based threading that combines the best of email and chat.

- **Repo:** https://github.com/zulip/zulip
- **License:** Apache 2.0
- **Stack:** Django + PostgreSQL, **Tornado** for real-time push, **RabbitMQ** with custom queue workers, Handlebars + TypeScript frontend
- **Worth studying for:** Almost every choice here is deliberately *not* the default, and documented as such. Real-time delivery is **long-polling through a separate Tornado process**, not Channels or WebSockets. Background work runs on **hand-rolled RabbitMQ workers wrapping `pika`**, not Celery. The frontend is Handlebars and TypeScript, not React. Add **100% mypy coverage** and ~185K words of contributor documentation, and it is arguably the best-documented large Django codebase in existence.

### Plane

Open-source Jira, Linear, Monday and ClickUp alternative — a project management platform for tasks, sprints, docs and triage.

- **Repo:** https://github.com/makeplane/plane
- **License:** AGPL-3.0
- **Stack:** Django 5.2 + DRF 3.17 (backend lives at `apps/api`), PostgreSQL (psycopg 3), Celery + `django-celery-beat`/`-results`, Redis, Channels, S3 via `django-storages`, drf-spectacular, OpenTelemetry
- **Worth studying for:** A **conventional, well-executed Django REST backend inside a pnpm/Turbo monorepo** — useful precisely because it makes normal choices at scale. Good reference for OpenTelemetry instrumentation and Celery Beat scheduling in a real product.

### InvenTree

Open source inventory management system providing powerful low-level stock control and part tracking.

- **Repo:** https://github.com/inventree/inventree
- **License:** MIT
- **Stack:** Django + DRF, Django-Q2 task queue, `django-allauth`, PostgreSQL / MySQL / SQLite, React + Mantine frontend with TanStack Query and Lingui
- **Worth studying for:** A **three-surface plugin system** — plugins can extend via REST API, as installed Python modules, or through a dedicated plugin interface with mixins. Also a good model for deeply hierarchical domain data (parts, assemblies, stock locations) in the Django ORM.

### Open edX

The Open edX LMS & Studio, powering education sites around the world.

- **Repo:** https://github.com/openedx/openedx-platform *(formerly `edx-platform` — the rename is canonical)*
- **License:** AGPL-3.0
- **Stack:** Python 3.12 + Django, MySQL 8.0 **and** MongoDB 7.x, Memcached/Redis, React micro-frontends (MFEs)
- **Worth studying for:** The largest Django codebase on this list by a wide margin — ~68K commits across two deployable services (LMS and Studio) from one tree. Read it for **polyglot persistence** (relational data in MySQL, course content in MongoDB), for the micro-frontend decomposition of a legacy monolith's UI, and as a frank study in the cost of very long-lived Django systems.

### Revel

An event management and ticketing platform for communities that prioritize privacy, safety and autonomy.

- **Repo:** https://github.com/letsrevel/revel-backend
- **License:** MIT
- **Stack:** Python 3.14, Django 5.2 LTS with **Django Ninja** (not DRF), PostgreSQL + PostGIS, Celery + Redis
- **Worth studying for:** One of the few production-grade **Django Ninja** codebases you can read end to end — a real alternative to DRF, with type-annotated schemas instead of serializers. Also notable: HMAC-signed URLs for protected file access, a full LGTM observability stack (Loki/Grafana/Tempo/Mimir), and EU VAT reverse-charge handling. The smallest project on this list, which makes it the most readable.

## Frameworks & CMS Toolkits

Not standalone products — these are libraries you build *with*. Included because their internals are some of the best Django reading available.

### Wagtail

A Django content management system focused on flexibility and user experience.

- **Repo:** https://github.com/wagtail/wagtail
- **License:** BSD-3-Clause
- **Supports:** Python 3.10+, Django 5.2.x and 6.0.x; PostgreSQL, MySQL, MariaDB, SQLite
- **Worth studying for:** **StreamField** — structured, block-based content stored in a single field without giving up data integrity, and probably the most-copied idea in modern Django CMS design. Also worth reading: the page tree, the headless content API, and a release process with a three-month cadence and designated LTS versions.

### django CMS

Lean, open-source enterprise content management powered by Django.

- **Repo:** https://github.com/django-cms/django-cms
- **License:** BSD-3-Clause
- **Governance:** Backed by the non-profit django CMS Association
- **Worth studying for:** The **placeholder-and-plugin architecture** — a different answer to the same problem StreamField solves, worth reading alongside Wagtail to see the tradeoffs. Also **apphooks**, which mount arbitrary Django apps into the CMS page tree, plus mature multi-site i18n and frontend inline editing.

### Wagtail CRX (CodeRed Extensions)

Wagtail + CodeRed Extensions, enabling rapid development of marketing-focused websites.

- **Repo:** https://github.com/coderedcorp/coderedcms
- **License:** BSD-3-Clause (bundled icons are CC-BY-3.0)
- **Stack:** Django + Wagtail, Bootstrap 5, SASS/SCSS with no Node.js requirement
- **Worth studying for:** How to build a **reusable layer on top of Wagtail** rather than forking it — pre-built StreamField blocks, page types, a form builder, event pages and SEO settings, all shipped as an installable package.

---

Suggestions welcome — open an issue or a PR with a project and what makes it worth reading.
