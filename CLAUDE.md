# Project Brief: Ansible Role for install packages on linux systems

## WHY

This is ansible role is designed to install packages on linux systems. It provides a flexible and efficient way to install packages across multiple hosts in a idempotent manner.

## What

* **TECHNOLOGY STACK:** Ansible, YAML, Python, Linux
* **Supported Package Managers:** apt, dnf, pacman, zypper
* **KEY DIRECTORIES:**
  * `files` : Contains static files for deployment
  * `templates` : Contains Jinja2 templates for dynamic content
  * `tasks` : Contains tasks for deployment and update
  * `vars` : Contains variables for configuration
  * `meta` : Contains metadata for the role
  * `handlers` : Contains handlers for notifications
  * `defaults` : Contains default variables for the role
* **Naming Conventions:**
  * YAML files should use `.yml` or `.yaml` extension
  * Variable names should be snake_case

## HOW (Rules and Guidelines)

### Mandatory Rules

* **Idempotency:** All tasks MUST be idempotent. Re-running the playbook should not cause changes unless necessary.
* **Error Handling:** Implement robust error handling using `block`, `rescue`, and `always` patterns where appropriate.
* **Best Practices:** Follow established Ansible best practices, including Fully Qualified Collection Names (FQCN) for modules (e.g., `ansible.builtin.ping`).
* **Testing:** After generating any code, run the format and check scripts locally (`ansible-lint` and `yamllint`) to verify syntax and quality.

### Guidelines

The role should accept the following variables <ansible_pkg_mgr>_base_packages, package_list, package_list_group and package_list_host. It should combine all the variables with unique only entries and iterate through the results to install required packages.
