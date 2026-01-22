### Mastering Vagrant: 
#### A Practical Guide to Building and Managing Virtual Development Environments

### Preamble

> Without automation, the cloud is less "heavenly" and more "hellish."
Manually configuring servers feels like defusing bombs, but the wires are all the same color.
One missed cron job, and suddenly your entire infrastructure is a digital wasteland.
Scaling manually? Sure, if you enjoy reenacting a tech version of The Hunger Games.
In the end, without automation, the only thing scaling is chaos.

<div align=center>
<img width=500 src=https://github.com/buflallo/Club-cloud/blob/main/Beginner-Project01-Provisionning/images.jpeg/>
</div>

In this project, you will turn the secure server you built in Project 00 into **Infrastructure as Code** using Vagrant, so that a single `vagrant up` command can recreate it from scratch.

---

## What You Will Deliver

By the end of this project, you should have:

- A `Vagrantfile` that fully describes your virtual machine (box, networking, hostname, and any resources you decide to tune).
- One or more provisioning scripts that automate **all** of the configuration from Project 00 (SSH on port 1111, firewall, users, password policy, sudo logging, `monitoring.sh`, and your deployed service).
- A **fully reproducible** server: running `vagrant up` from scratch (or after `vagrant destroy`) results in a working server matching all Project 00 requirements, with **no manual steps** after the VM boots.
- Automation that is **idempotent**: running `vagrant provision` or re-running `vagrant up` does not break the configuration and finishes without errors.
- All of your automation (Vagrantfile + scripts) tracked in Git with several meaningful commits.
- A short README or guide explaining how to use your Vagrant setup and how the provisioning is structured.

---

## How You Will Work (strongly recommended)

To mirror real-world Infrastructure-as-Code practices, it is **strongly recommended** that you:

- Use Git to track your `Vagrantfile` and provisioning scripts with small, descriptive commits instead of one big "final" commit.
- Keep the main provisioning logic in separate shell scripts (or other tools) and reference them from the Vagrantfile, instead of putting everything inline.
- Regularly test your automation by destroying and recreating the VM (`vagrant destroy -f` then `vagrant up`) to ensure it works from a clean state.
- Read the Vagrant documentation, `vagrant --help`, and relevant `man` pages so you understand each option and command before using it.

#### Vagrant, What is that?

> Vagrant is a tool for building and managing virtual machine environments in a single workflow. With an easy-to-use workflow and focus on automation, Vagrant lowers development environment setup time, increases production parity, and makes the “works on my machine” excuse a relic of the past.

<div align=center>
<img width=500 src=https://miro.medium.com/v2/format:webp/0*pV4407g7awNTh1QP.png/>
</div>


#### Use Cases
Vagrant is incredibly versatile, making it useful for software developers, operations engineers, designers, and everyone in between. Here are a few common use cases:
- Development and Testing: Quickly set up isolated development and testing environments that mirror production systems.
- Learning and Experimentation: Learn new software or experiment with different technology stacks without the risk of affecting the main operating system.
- Continuous Integration / Continuous Deployment (CI/CD): Integrate Vagrant boxes into CI/CD pipelines to ensure that applications are tested in an environment that matches production as closely as possible.

By integrating Vagrant into your workflow, you can take advantage of these benefits to make your development process more efficient and error-free.

#### What is a Box?
A box is the predefined images that are used by Vagrant to build the environment according to the instructions provided by the user. A box may be a plain OS installation, or it may be an OS installation plus one or more applications installed. Boxes may support only a single provider or may support multiple providers (for example, a box might only work with VirtualBox, or it might support VirtualBox and VMware Fusion).

#### Essential Vagrant commands (reference)

Vagrant init — to create Vagrantfile

Vagrant box — to manage boxes in your local / host

Vagrant up — to create and provision the VM

Vagrant reload — to restart your VM

Vagrant ssh — to connect with the VM using SSH

Vagrant suspend — to suspend your VM

Vagrant resume — to resume your VM after you suspend it

Vagrant halt — to shut down the VM

Vagrant destroy — to delete the VM

Vagrant status — to know the status of your VM

> For more details on all commands and options, use `vagrant --help` or `man vagrant`.

---

## Key Concept: Idempotency

In real-world automation, **idempotency** is essential. An idempotent script or configuration can be applied multiple times and still leave the system in the same correct state, without causing errors or duplicate changes.

Why this matters:

- In production, automation tools regularly re-apply configuration to keep systems in the desired state.
- If a script fails halfway, you should be able to run it again safely.
- If someone runs `vagrant provision` on an already configured VM, it should not break your server.

Practical tips for idempotent provisioning:

- Check if something already exists before creating it (for example, test if a user or group exists before adding it).
- When editing configuration files, avoid adding the same line multiple times; check before appending or use tools that enforce a single change.
- Use package managers in a way that is safe to run multiple times (such as `apt-get install -y package`).
- Test by running `vagrant provision` at least twice and ensure the second run finishes cleanly and leaves the server working.

#### Setting Up Your First Vagrant Box
A “box” in Vagrant is a package containing a pre-configured operating system and environment settings.

1. Initialize a Project Directory: Create a new directory for your Vagrant project and navigate into it in your terminal:

```bash
mkdir my-vagrant-project
cd my-vagrant-project
```

2. Initialize Vagrant: Run `vagrant init` to create a new `Vagrantfile` in your directory. This file describes the type of machine required for a project, and how to configure and provision these machines. In this example, we’ll use a basic Ubuntu box:

```bash
vagrant init hashicorp/bionic64
```

Provisioning with Vagrant
Provisioning in Vagrant terms involves setting up the virtual machine with all necessary software and configurations automatically. This step is crucial for maintaining consistency across multiple development environments. Vagrant supports several provisioning tools, including shell scripts, Chef, Puppet, and Ansible. This chapter will explore how to use these tools to automate your environment setup.

#### Using Shell Scripts for Provisioning
Shell scripting is the most straightforward method for provisioning your Vagrant environments. It involves writing shell commands that Vagrant executes on the VM as it starts up. Here’s how you can use shell scripts for provisioning:

1. Inline Shell Scripting: Directly embedding shell commands in the `Vagrantfile`.

```bash
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y nginx
  SHELL
end
```

---

#### Intended Functionalities :

---

> This project consists of having you automate the [Server](https://github.com/buflallo/Club-cloud/tree/main/Beginner-00-Virtualization) you made as a first project.

* You are only allowed to use Vagrant as an automation tool, and VirtualBox as Vagrant provider.

* Your server must be provisioned under the **same rules** as the first project (all requirements from Project 00 must be met automatically).

* It should be fully automated; a single `vagrant up` command should be enough to configure and run your server, with no manual configuration after the VM boots.

* Your provisioning scripts must be **idempotent**: running `vagrant provision` or re-running `vagrant up` must not break the existing configuration and should complete without errors.

---

## Testing Your Automation

Before you consider this project complete, you should test your automation as if it were running in a CI/CD pipeline:

1. **Destroy and rebuild:** Run `vagrant destroy -f` to remove the VM, then run `vagrant up` again to rebuild it from scratch.
2. **Verify all requirements:** After the VM boots, check that all Project 00 requirements are satisfied (hostname, SSH on port 1111, firewall rules, users, password policy, sudo logging, `monitoring.sh` running via cron, and your service deployed).
3. **Re-provision safely:** With the VM still running, execute `vagrant provision` and make sure it finishes without errors and the server is still functional.
4. **Document your test:** In your guide or README, briefly describe the tests you ran (for example, "Destroyed and rebuilt the VM twice; all checks from Project 00 passed each time").




#### Ressources 

[Vagrant Official Documentation](https://developer.hashicorp.com/vagrant/docs)

---

## Deliverables :

Use this section as a checklist to ensure your automation behaves like real Infrastructure as Code.

### Vagrantfile & Provisioning Structure :

- [ ] A `Vagrantfile` exists and defines your VM (box, basic networking, hostname, and other relevant settings).
- [ ] Provisioning logic is organized into one or more separate scripts (or tools), not only inline in the Vagrantfile.
- [ ] Scripts are readable and contain comments explaining the main steps.
- [ ] The Vagrantfile clearly references your provisioning scripts so that someone else can understand how the VM is configured.

### Automation Completeness :

- [ ] Running `vagrant up` from scratch (or after `vagrant destroy`) configures **all** requirements from Project 00: hostname, SSH on port 1111, firewall, users, password policy, sudo logging, `monitoring.sh`, and your deployed service.
- [ ] No manual configuration steps are required after the VM boots; the server is ready to use.
- [ ] No errors appear during `vagrant up` or the provisioning phase.

### Idempotency & Testing :

- [ ] Running `vagrant provision` multiple times completes successfully and does not break the server.
- [ ] You have tested `vagrant destroy -f` followed by `vagrant up` at least once and verified that the server still meets all Project 00 requirements.
- [ ] Your README or guide briefly describes the tests you performed and their results.

### Version Control & Documentation :

- [ ] Your `Vagrantfile` and provisioning scripts are tracked in Git with multiple meaningful commits.
- [ ] You provide a short README or guide that explains:
  - How to use the Vagrantfile (for example, "run `vagrant up` to create and configure the server").
  - How the provisioning process is structured and automated (which scripts do what).
  - Any challenges you faced and how you solved them.

### All Project 00 Requirements Verified :

- [ ] SSH service is running only on port 1111, and root login over SSH is disabled.
- [ ] The firewall (UFW or firewalld) allows only port 1111.
- [ ] Users, password policy, and sudo configuration match the rules from Project 00.
- [ ] `monitoring.sh` is deployed and configured to run via cron every 10 minutes, broadcasting the required information.
- [ ] Your final service (for example, a web server) is deployed and functional after `vagrant up`.
