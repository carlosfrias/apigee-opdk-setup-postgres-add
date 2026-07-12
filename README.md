# apigee-opdk-setup-postgres-add — Add a Postgres Datastore to an Apigee OPDK Analytics Group

> 🔄 **Evolution note:** The automation approach from this OPDK-era role has been consolidated into the `apigee-hybrid-workspace` Ansible collection. See the successor capability in the portfolio hub: [`carlosfrias/apigee-hybrid-workspace`](https://github.com/carlosfrias/apigee-hybrid-workspace) → `bap_coe/private_cloud/` and `bap_coe/apigee_hybrid/`. The collection README explains each role group’s business value and production context.


> **An Ansible role that registers a Postgres master/standby pair as a datastore in an Apigee analytics consumer group** — the datastore step in the analytics topology lifecycle: `axgroup → consumer-group → {consumers (qpid), datastores (postgres master,standby)}`.

> [!NOTE]
> Engineering portfolio note — this role is part of the analytics-topology lifecycle. See the [skills assessment →](SKILLS-ASSESSMENT.md) for the expertise applied.

This role adds a Postgres master/standby pair as a datastore to an existing axgroup and its consumer group. It is composed after `apigee-opdk-setup-analytics-group-add` (axgroup creation) and `apigee-opdk-setup-qpid-add` (consumer addition), and before `apigee-opdk-setup-scopes-add` (scope binding). See the [`apigee-edge-opdk`](https://github.com/carlosfrias/apigee-edge-opdk) framework for composition playbooks.

<!-- BEGIN Google Required Disclaimer -->

## Not Google Product Clause

This is not an officially supported Google product.
<!-- END Google Required Disclaimer -->

---

## What the role actually does

`tasks/main.yml`:

1. **Resolve master/standby UUIDs** — reads `edge_ps_self.uUID` from the `pgmaster` and `pgstandby` inventory groups via `hostvars`.
2. **Construct the master,standby UUID pair** — concatenates master and standby UUIDs for the datastore registration (Apigee expects `uuid1,uuid2` format for the HA pair).
3. **Assert required attributes** — `ax_group`, `consumer_group`, `server_type`, `master_uuid`, `standby_uuid`, credentials.
4. **Register the Postgres server** — `POST /v1/analytics/groups/ax/{ax_group}/servers?uuid={master,standby}&type={server_type}&force=true`.
5. **Add Postgres to the consumer group as a datastore** — `POST /v1/analytics/groups/ax/{ax_group}/consumer-groups/{consumer_group}/datastores?uuid={master,standby}`.

---

## Role variables (selected)

| Variable | Purpose |
|----------|---------|
| `ax_group` | The analytics group name |
| `consumer_group` | The consumer group name |
| `pgmaster_group_name` | Inventory group name for the Postgres master (default: `pgmaster`) |
| `pgstandby_group_name` | Inventory group name for the Postgres standby (default: `pgstandby`) |
| `local_mgmt_ip` | Management Server IP |
| `opdk_user_email` / `opdk_user_pass` | MS API credentials |

---

## Provenance

Authored and maintained by **Carlos Frias** during his tenure on Apigee Edge Private Cloud. One of the analytics-topology roles in the `apigee-opdk-*` corpus.

Contributions welcome — see [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

See [LICENSE](./LICENSE).