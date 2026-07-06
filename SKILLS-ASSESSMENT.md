# Skills Assessment — apigee-opdk-setup-postgres-add

> **Skill domain:** Apigee analytics topology modeling — adding a Postgres master/standby pair as a datastore to an analytics consumer group (OPDK). Part of the broader Apigee platform-operations portfolio; see the [`bap_coe` portfolio hub →](https://github.com/carlosfrias/apigee-hybrid-workspace/blob/main/SKILLS-ASSESSMENT.md) for the cloud-native (Hybrid/K8s) counterpart and the full corpus.

---

## Why this role is notable

- **Postgres HA pair as a datastore.** Registers a `master,standby` UUID pair as a single datastore entry — Apigee analytics expects the HA pair, not individual nodes. The role constructs the pair from inventory group vars and concatenates the UUIDs.
- **Datastore vs. consumer distinction.** Qpid servers are added as *consumers*; Postgres pairs are added as *datastores*. This role uses the `/datastores` endpoint, not `/consumers` — the topology modeling is explicit about the node's role in the analytics pipeline.
- **Composed lifecycle.** This role runs after `analytics-group-add` and `qpid-add`, and before `scopes-add`. The analytics topology is built incrementally, role by role, in dependency order.

---

## Expertise demonstrated

> Ansible is the medium. The engineering evidence lives in the [project README →](README.md). What follows is the skills assessment for the business reader.

- **Apigee analytics topology modeling** — Postgres as a *datastore* (not a consumer) in the `axgroup → consumer-group → {consumers, datastores}` directed object graph. The role models the analytics pipeline's data flow: Qpid collects, Postgres stores.
- **HA pair registration** — constructs the `master,standby` UUID pair from inventory group vars and registers it as a single datastore. The pair is treated as one unit by the analytics group.
- **Composable role architecture** — focused, testable unit designed for composition into larger provisioning runbooks via `requirements.yml`.

---

## How this shows the expertise

The expertise is in the distinction between *consumer* and *datastore* in the analytics topology. Qpid nodes are consumers (they collect analytics events); Postgres nodes are datastores (they persist them). This role uses the `/datastores` endpoint because that is what Postgres is in the Apigee analytics architecture. The `master,standby` UUID pair is registered as a single unit because Apigee expects the HA pair, not individual nodes.

The role is a focused step in the composable lifecycle: `analytics-group-add` → `qpid-add` → **`postgres-add`** → `scopes-add`. Each role owns one API interaction.

---

## Related expertise

| Skill | Repository | Assessment |
|-------|-----------|-----------|
| Analytics (axgroup) lifecycle | [`apigee-opdk-setup-analytics-group-add`](https://github.com/carlosfrias/apigee-opdk-setup-analytics-group-add) | [SKILLS-ASSESSMENT.md →](https://github.com/carlosfrias/apigee-opdk-setup-analytics-group-add/blob/main/SKILLS-ASSESSMENT.md) ✅ |
| Qpid consumer addition | [`apigee-opdk-setup-qpid-add`](https://github.com/carlosfrias/apigee-opdk-setup-qpid-add) | [SKILLS-ASSESSMENT.md →](https://github.com/carlosfrias/apigee-opdk-setup-qpid-add/blob/main/SKILLS-ASSESSMENT.md) ✅ |
| Scope binding (org/env) | [`apigee-opdk-setup-scopes-add`](https://github.com/carlosfrias/apigee-opdk-setup-scopes-add) | [SKILLS-ASSESSMENT.md →](https://github.com/carlosfrias/apigee-opdk-setup-scopes-add/blob/main/SKILLS-ASSESSMENT.md) ✅ |
| Postgres HA / controlled switchover | [`apigee-opdk-setup-postgres-failover`](https://github.com/carlosfrias/apigee-opdk-setup-postgres-failover) | [SKILLS-ASSESSMENT.md →](https://github.com/carlosfrias/apigee-opdk-setup-postgres-failover/blob/main/SKILLS-ASSESSMENT.md) ✅ |
| Cloud-native (Hybrid/K8s) counterpart | [`apigee-hybrid-workspace`](https://github.com/carlosfrias/apigee-hybrid-workspace) | [SKILLS-ASSESSMENT.md →](https://github.com/carlosfrias/apigee-hybrid-workspace/blob/main/SKILLS-ASSESSMENT.md) ✅ portfolio hub |

---

## Provenance

Authored and maintained by **Carlos Frias** during his tenure on Apigee Edge Private Cloud. This skills assessment is the companion to the engineering [README →](README.md).

## License

See [LICENSE](./LICENSE).