# Topic 5 – Configuration Management Lab

> **DevOps Module | BPP | Level 5**

Ansible manages this machine's configuration. There is no external server — Ansible runs locally, which keeps the setup simple and lets you focus on the concepts.

---

## Getting started

The Codespace installs Ansible automatically. Once it's ready, open a terminal and run:

```bash
cd ansible
ansible-playbook -i inventory.ini playbook.yml
```

You should see Ansible work through the tasks and finish with a PLAY RECAP.

---

## Lab tasks

### Task 1 — Run the playbook and read the output

Run the playbook and look at the PLAY RECAP at the bottom.

- How many tasks reported `changed`?
- How many reported `ok`?

Then look at what Ansible created:

```bash
cat /opt/my-app/app.conf
```

> 💬 **Reflection:** The playbook is a YAML file that says *what* should exist, not *how* to create it. How is this different from writing a shell script that runs `mkdir` and `echo` commands?

---

### Task 2 — Observe idempotency

Run the **exact same command** again without changing anything:

```bash
ansible-playbook -i inventory.ini playbook.yml
```

Compare the PLAY RECAP to your first run.

> 💬 **Reflection:** Why did fewer tasks report `changed` this time? What would happen on a server with 100 machines if every run forced every change regardless of current state?

---

### Task 3 — Change the desired state

Open `playbook.yml` and change the `app_env` variable from `development` to `staging`:

```yaml
app_env: staging
```

Save the file and re-run the playbook. Check the config file again:

```bash
cat /opt/my-app/app.conf
```

> 💬 **Reflection:** Ansible detected that the file contents no longer matched the desired state and updated it. How does keeping configuration in version-controlled files (rather than making manual changes on servers) help a team manage change?

---

### Task 4 — Add a new config value

Open `ansible/templates/app.conf.j2` and add a new line:

```
APP_AUTHOR={{ app_author }}
```

Then add a matching variable to `playbook.yml`:

```yaml
app_author: your-name
```

Re-run the playbook and verify the new value appears in the config file.

> 💬 **Reflection:** You changed the *template* and the *variables* separately. Why is separating the structure of a config file from its values a useful pattern?

---

## Key concepts

| Term | What it means |
|---|---|
| **Desired state** | What you want the system to look like, declared in the playbook |
| **Idempotent** | Safe to run repeatedly — only changes what needs changing |
| **Declarative** | You say *what* you want; Ansible works out *how* |
| **Template** | A config file with `{{ variable }}` placeholders filled in at run time |
| `changed` | Ansible had to make a change to reach desired state |
| `ok` | The system was already in the desired state — nothing to do |

---

## Assessment criteria reminder

| Criterion | What to address in your DevOps Story |
|---|---|
| **Practical Skills (40%)** | Show the playbook running, tasks completing, config file being written |
| **Knowledge & Understanding (20%)** | Explain declarative vs imperative; what idempotency means in practice |
| **Design & Architecture (20%)** | Explain why variables and templates are separated; what the inventory does |
| **Reflection on Practice (20%)** | Use the reflection prompts above; connect to managing real infrastructure at scale |
