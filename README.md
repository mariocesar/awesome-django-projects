# Awesome Django Projects

A collection of real Django projects worth reading. Not tutorials, not toy apps. Code that runs in production and has to keep running tomorrow.

## Why I made this

I started with open source at 17, always Python and the Web, and I never stopped. I've been with Django since 0.96. I maintained sorl-thumbnail for years. Most of what I actually know about Django I didn't learn from documentation, I learned it from opening someone else's project and reading how they solved the thing I was stuck on.

Tutorials show you the happy path. A real codebase shows you the compromises. How they split settings across five deploy targets. Where they gave up on the ORM and wrote SQL. What they did when Celery wasn't enough. How they ran a migration on a table nobody could take offline. That's the part nobody writes a blog post about, but it's all sitting there in the repository.

I'm in love with boring software. Django is boring in the best way: it's been here for twenty years, it doesn't break what works, and the community around it values long term relations over hype. Most projects on this list are boring too. They just quietly do the job, for years, for real users. It's not fun to be boring, but boring is good.

For each project I wrote what it does, what it runs on, and the one thing I think is actually worth your time to go read. I also say when a project isn't really open source, or when the maintenance looks shaky. You should know that before you build on top of something.

## Applications

Complete products you can deploy.

### Paperless-ngx

Scan, index and archive all your documents. Community supported and very actively maintained.

- **Repo:** https://github.com/paperless-ngx/paperless-ngx
- **Stack:** Django 5.2, DRF, Celery + Redis, Angular, PostgreSQL / MariaDB / SQLite
- **Go read:** the search. They use **tantivy**, not Whoosh, not Postgres `SearchVector`. Everybody defaults to one of those two and this project shows the third option working in production. The permissions layer is also worth your time: `django-guardian` for object-level permissions, `django-auditlog` for the trail, `django-cachalot` for ORM caching. If you've ever had to bolt "who can see this row" onto an app that didn't plan for it, read this first.

### NetBox

Source of truth for network infrastructure. IPAM and DCIM in one place, with a real API.

- **Repo:** https://github.com/netbox-community/netbox
- **Stack:** Django, DRF + GraphQL, PostgreSQL 14+, Redis, Python 3.12–3.14, server-rendered UI
- **Go read:** the plugin system. I think it's the best one in the Django world. A plugin registers models, views, tables and API endpoints against documented extension points, and it doesn't feel bolted on. If you're building anything that other people need to extend without forking you, start here. The automatic change logging on every object is the other thing I'd steal.

### TacticalRMM

Remote monitoring and management, built with Django, Vue and Go.

- **Repo:** https://github.com/amidaware/tacticalrmm
- **⚠️ Not open source.** The license is source-available. You can't offer it as a SaaS or a commercial hosted product without written permission from AmidaWare. In-house use and managing your own customers' networks are fine. Read it, learn from it, but check the license before you build a business on it.
- **Stack:** Django + DRF, Vue, Go agent, NATS, MeshCentral
- **Go read:** the `natsapi` app. It's Django talking to thousands of Go agents over **NATS** instead of HTTP polling. I don't know another Django project doing high-fanout messaging like this in the open. Worth reading even if you never touch RMM software.

### LibrePhotos

Self-hosted photos with face recognition, object detection and semantic search.

- **Repo:** https://github.com/LibrePhotos/librephotos
- **Stack:** Django 5 + DRF, PostgreSQL, Django-Q2, React 18 + TypeScript + Vite + Mantine
- **Go read:** how they keep heavy ML off the request cycle. Face recognition, hdbscan clustering, im2txt captioning, places365 scene detection, all pushed into background jobs. If you're putting a model behind a Django view and wondering where it goes, this is the honest answer. Bonus: they merged five repos into one and kept the full commit history, which is a nice piece of work on its own.

### Django Packages

The directory of reusable Django apps. The site a lot of us have used for years without thinking about who runs it.

- **Repo:** https://github.com/djangopackages/djangopackages
- **Stack:** Django, PostgreSQL, Tailwind, `uv`, pytest, Docker Compose
- **Go read:** `apiv3` and `apiv4` living side by side in the tree. This is a public API evolving in the open without breaking the people using it, and you can read the whole history of how they did it. Also the PyPI/GitHub/Bitbucket ingestion, which is a lot of unglamorous integration work done carefully. Good modern tooling for a community project, which is rarer than it should be.

### Docs

Real-time collaborative documents. An open alternative to Notion or Outline, built by the French (DINUM) and German (ZenDiS) governments.

- **Repo:** https://github.com/suitenumerique/docs
- **Stack:** Django + DRF + `django-configurations`, PostgreSQL, Celery + Redis, S3; Next.js frontend with Yjs, ProseMirror, BlockNote, Hocuspocus
- **Go read:** the split between Django and the CRDT layer. Django owns identity, permissions and persistence. Hocuspocus and Yjs own the live document. Drawing that line correctly is the whole problem with collaborative editing and here's a working answer. The rest of the stack is a solid public-sector reference: OIDC via `mozilla-django-oidc`, feature flags with `django-waffle`, CSP headers, `django-treebeard` for sub-documents.

### PostHog

Product analytics, session replay, feature flags, experiments. Open core.

- **Repo:** https://github.com/PostHog/posthog
- **License:** MIT, with a separately licensed proprietary `ee/` directory
- **Stack:** Django, **ClickHouse** for analytics plus PostgreSQL for everything else, React/TypeScript, with Rust and Node services in the monorepo
- **Go read:** Django as the control plane over a columnar database. The ORM handles users, teams and config. Event queries go to ClickHouse. This is the pattern you reach for when your data outgrows Postgres but you don't want to throw away Django, and there aren't many examples this size you can read. I also respect that the open/proprietary split is visible in one repo instead of hidden behind a private fork.

### Sentry

Error tracking and performance monitoring. Fair source.

- **Repo:** https://github.com/getsentry/sentry
- **License:** FSL-1.1-Apache-2.0, not open source
- **Stack:** Django 5.2 + DRF, PostgreSQL, Redis, Kafka, ClickHouse via **Snuba**, Rust services (Relay, taskbroker), React + TypeScript
- **Go read:** `src/sentry/hybridcloud/`. They split one monolith into control and cell databases for data residency, and keep them in sync with a transactional outbox instead of joins. The biggest example of breaking up a Django monolith without a rewrite.

### Zulip

Team chat organized by topic. Apache 2.0.

- **Repo:** https://github.com/zulip/zulip
- **Stack:** Django + PostgreSQL, **Tornado** for real-time, **RabbitMQ** with hand-written workers, Handlebars + TypeScript
- **Go read:** all of it, honestly. Almost every choice here is deliberately not the default, and they wrote down why. Real-time is long-polling through a separate Tornado process, not Channels, not WebSockets. Background jobs run on their own RabbitMQ workers wrapping `pika`, not Celery. The frontend is Handlebars and TypeScript, not React. Add 100% mypy coverage and something like 185K words of contributor documentation and this is the best documented large Django codebase that exists. If you only read one project on this list, read this one.

### Plane

Project management. An alternative to Jira, Linear and ClickUp. AGPL-3.0.

- **Repo:** https://github.com/makeplane/plane
- **Stack:** Django 5.2 + DRF (backend is at `apps/api`), PostgreSQL, Celery with beat and results, Redis, Channels, S3, drf-spectacular, OpenTelemetry
- **Go read:** this one is useful precisely because it's ordinary. A normal DRF backend, done properly, at real scale, inside a pnpm/Turbo monorepo. When you want to see what "just build it the standard way" looks like when it works, this is it. The OpenTelemetry setup and the Celery Beat scheduling are worth copying.

### InvenTree

Inventory management with low-level stock control and part tracking. MIT.

- **Repo:** https://github.com/inventree/inventree
- **Stack:** Django + DRF, Django-Q2, `django-allauth`, PostgreSQL / MySQL / SQLite, React + Mantine with TanStack Query and Lingui
- **Go read:** the plugin system has three surfaces. REST API, installed Python modules, and a mixin-based plugin interface. Compare it with NetBox and you get two serious answers to the same question. The deeply nested domain data, parts inside assemblies inside stock locations, is also a good study in hierarchical models that stay queryable.

### Open edX

The LMS and Studio behind a large part of online education. AGPL-3.0.

- **Repo:** https://github.com/openedx/openedx-platform (renamed from `edx-platform`, this URL is the current one)
- **Stack:** Python 3.12 + Django, MySQL 8.0 **and** MongoDB 7.x, Memcached/Redis, React micro-frontends
- **Go read:** this is the biggest codebase here by a wide margin. Around 68K commits, two deployable services from one tree. Read it for the polyglot persistence, relational data in MySQL and course content in MongoDB, and for how they pulled micro-frontends out of a monolith's UI. Also read it as an honest look at what a twenty-year-old Django system costs to keep alive. That's not a criticism. Someone had to keep it alive, and they did.

### Revel

Event management and ticketing for communities that care about privacy and safety. MIT.

- **Repo:** https://github.com/letsrevel/revel-backend
- **Stack:** Python 3.14, Django 5.2 LTS with **Django Ninja**, PostgreSQL + PostGIS, Celery + Redis
- **Go read:** it's one of the few production **Django Ninja** codebases you can read end to end. I used Ninja on a big app and liked the ergonomics a lot, so it's good to have a real example to point at. Type-annotated schemas instead of serializers, and you can judge the tradeoff yourself. Small enough to read in an afternoon, which makes it the best starting point on this list. The HMAC-signed URLs for protected files and the full LGTM observability setup are nice extras.

### Seedcorn

Self-hosted project planner with a Gantt timeline and reusable plan templates. Apache-2.0.

- **Repo:** https://github.com/vakahnke/seedcorn (formerly `Timeline`)
- **Stack:** Django 6.1 + DRF, PostgreSQL, React 19 + Vite
- **Go read:** `backend/events/status_report.py`. Project status derived from the schedule by explicit rules, in plain Python outside the models and serializers. Young and single-maintainer, so read it before you depend on it.

## Frameworks and CMS toolkits

These aren't products, they're things you build with. They're here because their internals are some of the best Django reading available.

### Wagtail

A Django CMS focused on flexibility and editor experience. BSD-3-Clause.

- **Repo:** https://github.com/wagtail/wagtail
- **Supports:** Python 3.10+, Django 5.2 and 6.0; PostgreSQL, MySQL, MariaDB, SQLite
- **Go read:** **StreamField**. Block-based structured content in a single field, without giving up data integrity. It's the most borrowed idea in modern Django content modelling and it's worth understanding from the source rather than from a blog post. The release process is also worth noting, three month cadence with designated LTS versions, which is exactly the boring predictability I want from something I depend on.

### django CMS

Enterprise content management on Django. BSD-3-Clause, backed by the non-profit django CMS Association.

- **Repo:** https://github.com/django-cms/django-cms
- **Go read:** the placeholder and plugin architecture. It's a different answer to the same problem StreamField solves, and reading both is more useful than reading either one. **Apphooks** are the other idea worth your time, mounting a whole Django app into the CMS page tree. The multi-site i18n is mature and has been fixed by real users hitting real edge cases for a long time.

### Wagtail CRX

Wagtail plus CodeRed Extensions, for building marketing sites fast. BSD-3-Clause.

- **Repo:** https://github.com/coderedcorp/coderedcms
- **Stack:** Django + Wagtail, Bootstrap 5, SASS with no Node.js required
- **Go read:** how to build a reusable layer on top of Wagtail instead of forking it. Pre-built StreamField blocks, page types, form builder, event pages, SEO settings, all shipped as a package you install. If you keep rewriting the same marketing site, this is the pattern.

## Contributing

If you know a Django project worth reading, open an issue or a PR. Tell me what it does and, more importantly, the specific thing in it you think someone should go read.
