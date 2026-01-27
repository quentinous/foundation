# Hierarchy Categories Reference

## Category Mapping Table

This table defines how detected components map to hierarchical paths.

| Composant détecté | Keywords | Catégorie | Sous-catégorie | Path généré |
|-------------------|----------|-----------|----------------|-------------|
| iOS app | ios, iphone, apple, swift, swiftui | app | mobile | `app/mobile/ios/` |
| Android app | android, google play, kotlin, jetpack | app | mobile | `app/mobile/android/` |
| React Native app | react native, expo | app | mobile | `app/mobile/react-native/` |
| Flutter app | flutter, dart | app | mobile | `app/mobile/flutter/` |
| Windows app | windows, win32, wpf, uwp, winui | app | desktop | `app/desktop/windows/` |
| Linux app | linux, gtk, qt, gnome, kde | app | desktop | `app/desktop/linux/` |
| macOS app | macos, cocoa, appkit, mac app | app | desktop | `app/desktop/macos/` |
| Electron app | electron, desktop web | app | desktop | `app/desktop/electron/` |
| Web frontend | react, vue, angular, svelte, frontend | app | web | `app/web/frontend/` |
| Web admin | admin panel, backoffice, dashboard | app | web | `app/web/admin/` |
| REST API | rest api, http endpoints, express, fastify | services | api | `services/api/rest/` |
| GraphQL API | graphql, apollo server | services | api | `services/api/graphql/` |
| gRPC service | grpc, protobuf | services | api | `services/api/grpc/` |
| Auth service | authentication, auth, oidc, oauth, keycloak | services | core | `services/core/auth/` |
| User service | user management, profiles | services | core | `services/core/users/` |
| Notification service | notifications, email, push, sms | services | core | `services/core/notifications/` |
| PostgreSQL | postgresql, postgres, pg | infra | data | `infra/data/postgres/` |
| MySQL | mysql, mariadb | infra | data | `infra/data/mysql/` |
| MongoDB | mongodb, mongo | infra | data | `infra/data/mongodb/` |
| Redis | redis, cache | infra | data | `infra/data/redis/` |
| Kubernetes | kubernetes, k8s, helm | infra | deploy | `infra/deploy/k8s/` |
| Docker Compose | docker compose, docker-compose | infra | deploy | `infra/deploy/docker/` |
| Terraform | terraform, iac | infra | deploy | `infra/deploy/terraform/` |
| GitHub Actions | github actions, gh actions | infra | ci | `infra/ci/github-actions/` |
| GitLab CI | gitlab ci, gitlab-ci | infra | ci | `infra/ci/gitlab-ci/` |
| Shared library | shared, common, utils | libs | shared | `libs/shared/` |
| Client SDK | sdk, client library | libs | sdk | `libs/sdk/` |
| Design system | design system, ui components | libs | ui | `libs/ui/design-system/` |

---

## Transmission Inheritance by Category

Each category level receives specific types of transmissions:

| Path | Transmissions reçues |
|------|---------------------|
| `app/` | Recherches UX globales, design system, accessibility guidelines |
| `app/mobile/` | Tendances mobile, concurrents apps, guidelines App Store/Play Store |
| `app/desktop/` | Tendances desktop, patterns desktop, distribution strategies |
| `app/web/` | SEO, web performance, browser compatibility |
| `services/` | Architecture patterns, API standards, security guidelines |
| `services/api/` | API versioning, rate limiting, documentation standards |
| `services/core/` | Business logic patterns, domain events |
| `infra/` | Cloud costs, scaling strategies, disaster recovery |
| `infra/data/` | Database migrations, backup strategies, data governance |
| `infra/deploy/` | Deployment strategies, rollback procedures |
| `infra/ci/` | Pipeline optimization, test strategies |
| `libs/` | Code quality standards, versioning, documentation |

---

## Category Icons

| Category | Icon |
|----------|------|
| app | 📱 |
| services | 🔧 |
| infra | 🏗️ |
| libs | 📚 |

| Subcategory | Icon |
|-------------|------|
| mobile | 📱 |
| desktop | 🖥️ |
| web | 🌐 |
| api | 🔌 |
| core | ⚙️ |
| data | 💾 |
| deploy | 🚀 |
| ci | 🔄 |
| shared | 📦 |
| sdk | 🧰 |
| ui | 🎨 |

---

## Usage in Step 02

When analyzing architecture, use this table to:

1. **Detect keywords** in the architecture document
2. **Map to category** using the Category column
3. **Generate path** using the Path généré column
4. **Create intermediate nodes** for each path level
