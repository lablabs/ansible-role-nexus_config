# Sonatype Nexus Repository Manager configuration

[![Galaxy Quality](https://img.shields.io/ansible/quality/57217?style=flat&logo=ansible)](https://galaxy.ansible.com/lablabs/nexus_config)
[![Role version](https://img.shields.io/github/v/release/lablabs/ansible-role-nexus_config)](https://galaxy.ansible.com/lablabs/nexus_config)
[![Role downloads](https://img.shields.io/ansible/role/d/57217)](https://galaxy.ansible.com/lablabs/nexus_config)
[![molecule lint](https://github.com/lablabs/ansible-role-nexus_config/actions/workflows/lint.yml/badge.svg)](https://github.com/lablabs/ansible-role-nexus_config/actions/workflows/lint.yml)
[![License](https://img.shields.io/github/license/lablabs/ansible-role-nexus_config)](https://github.com/lablabs/ansible-role-nexus_config/blob/main/LICENSE)

[<img src="ll-logo.png">](https://lablabs.io/)

This Ansible role will configure [Sonatype Nexus Repository Manager](https://www.sonatype.com/products/repository-oss?topnav=true) using the Rest API.
Currently, this role is covers:

- [x] Initial admin password setup
- [x] Users creation
- [x] Users update
- [x] Users deletion
- [x] Blob storage (file) creation
- [x] Blob storage (file) update
- [x] Blob storage (file) deletion
- [x] Blob storage (AWS S3) creation
- [x] Blob storage (AWS S3) update
- [x] Blob storage (AWS S3) deletion
- [ ] Blob storage (Azure) creation
- [ ] Blob storage (Azure) update
- [ ] Blob storage (Azure) deletion
- [x] Repositories (Maven, Docker, PyPi, Conda, R, npm) creation
- [x] Repositories (Maven, Docker, PyPi, Conda, R, npm) update
- [x] Repositories (Maven, Docker, PyPi, Conda, R, npm) deletion
- [x] Roles creation
- [x] Roles update
- [x] Roles detetion
- TBD ...

## Requirements

Ansible >= 2.10

## Role Variables

This is a copy of `defaults/main.yml`

```yaml
---

# Administrator user name
admin_username: admin

# Initial Nexus admin password
initial_admin_password: admin123

# Admin password which will be set during the initial setup.
admin_password: "{{ lookup('env', 'ADMIN_PASSWORD') }}"

# Nexus API host
api_host: localhost

# Nexus API port
api_port: 8081

# Nexus endpoint protocol
api_protocol: http

# Path to root ca .pem file, if https with custom ssl is used
root_ca:

# Hide sensitive Ansible error logs (may contain passwords)
hide_sensitive_logs: true

# Anonymous access
anonymous_access: true

users: []
  # - id: joan                    # User ID
  #   first_name: Joan            # User's first name
  #   last_name: Doe              # User's last name
  #   email: joan@example.org     # Email
  #   password: nbusr123          # Password ( do not push it to git :) )
  #   status: active              # Status of the user. You can set active/disabled or deleted to delete the user.
  #   source: default             # Source
  #   roles:                      # List of the assigned roles
  #     - nx-admin
  # - id: joe
  #   first_name: Joe
  #   last_name: Doe
  #   email: joe@example.org
  #   password: "{{ lookup('env', 'JOE_PASSWORD') }}"
  #   status: disabled
  #   source: default
  #   roles:
  #     - nx-anonymous

roles: []
  #  - id: nx-role                         # Role ID
  #    name: nx-role                       # Role name
  #    description: Some new role          # Role description
  #    status: created                     # Status of the role. You can set created or deleted to delete the role.
  #    privileges:                         # Privileges to include
  #      - nx-repository-view-*-*-add
  #      - nx-repository-view-*-*-browse
  #      - nx-repository-view-*-*-read
  #    roles: [ ]                          # Roles to include

stores: []
  # - name: file_blob             # Blob Store name
  #   type: file                  # Blob Store type (file, s3)
  #   soft_quota: 0               # Blob Store quota
  #   path: /tmp/blobs
  #   status: active              # Blob Store status (active, deleted)
  # - name: s3_blog
  #   type: s3
  #   soft_quota: 0
  #   prefix: ""
  #   region: default
  #   expiration_days: -1
  #   status: active

repositories: []
  # - name: maven_repo_hosted
  #   online: true                                  # Repository state (true, false, deleted)
  #   type: maven                                   # Repository type (Currently supported: maven)
  #   kind: hosted                                  # Repository kind (hosted, proxy)
  #   blob_store: default                           # Blob storeage
  #   strict_content_type_validation: false         # Strict Content Type Validation
  #   write_policy: allow                           # Controls if deployments of and updates to assets are allowed (allow, allow_once, deny)
  #   cleanup_names: []                             # Cleanup policies names. Omit if no cleanup is needed.
  #   version_policy: MIXED                         # Version Policy (MIXED, RELEASE, SNAPSHOT)
  #   layout_policy: STRICT                         # Layout Policy (STRICT, PERMISSIVE)
  #   content: INLINE                               # Content Disposition (INLINE)

  # - name: maven_repo_proxy
  #   online: true
  #   type: maven
  #   kind: proxy
  #   blob_store: default
  #   strict_content_type_validation: false
  #   remote_url: https://maven.example.org/repo    # Remote repository url
  #   maximum_artifacts_age: -1                     # Maximum component age
  #   maximum_metadata_age: 1440                    # Maximum metadata age
  #   negative_cache: true                          # Not found cache
  #   not_found_cache_ttl: 1440                     # Not found cache TTL
  #   http_client:
  #     blocked: false
  #     auto_block: true
  #     connection:
  #       retries: 0
  #       user_agent_suffix: ""
  #       timeout: 60
  #       enable_circular_redirects: false
  #       enable_cookies: false
  #       user_trust_store: false
  #     authentication:                             # Remote repo authentication
  #       type: username                            # Authentication type (username, ntlm)
  #       username: joe
  #       password: nbusr123
  #       ntlm_host:
  #       ntlm_domain:
  #       preemptive: false
  #   routing_rule: null
  #   version_policy: MIXED
  #   layout_policy: STRICT
  #   content: INLINE

  # - name: maven_repo_group
  #   online: true
  #   type: maven
  #   kind: group
  #   blob_store: default
  #   strict_content_type_validation: false
  #   group:
  #     - maven-releases
  #     - maven-snapshots

  #  - name: docker-hub-proxy
  #    online: true
  #    type: docker
  #    kind: proxy
  #    blob_store: default
  #    strict_content_type_validation: false
  #    remote_url: "https://index.docker.io/"       # Location of the remote repository being proxied (Optional)
  #    maximum_artifacts_age: -1
  #    maximum_metadata_age: 1440
  #    negative_cache: true
  #    not_found_cache_ttl: 1440
  #    routing_rule: null
  #    v1_enabled: true                             # Whether to allow clients to use the V1 API to interact with this repository
  #    force_basic_auth: true                       # Whether to force authentication (Docker Bearer Token Realm required if false)
  #    index_type: "REGISTRY"                       # Type of Docker Index (HUB, REGISTRY, CUSTOM)
  #    index_url: "https://index.docker.io/"        # Url of Docker Index to use (Optional)

  #  - name: conda-forge-proxy
  #    online: true
  #    type: conda
  #    kind: proxy
  #    blob_store: default
  #    strict_content_type_validation: false
  #    remote_url: "https://conda.anaconda.org/conda-forge/"
  #    maximum_artifacts_age: -1
  #    maximum_metadata_age: 1440
  #    negative_cache: true
  #    not_found_cache_ttl: 1440
  #    routing_rule: null

  #  - name: cran-proxy
  #    online: true
  #    type: r
  #    kind: proxy
  #    blob_store: default
  #    strict_content_type_validation: false
  #    remote_url: "https://cloud.r-project.org"
  #    maximum_artifacts_age: -1
  #    maximum_metadata_age: 1440
  #    negative_cache: true
  #    not_found_cache_ttl: 1440
  #    routing_rule: null

  #  - name: npm-proxy
  #    online: true
  #    type: npm
  #    kind: proxy
  #    blob_store: default
  #    strict_content_type_validation: false
  #    remote_url: "https://registry.npmjs.org/"
  #    maximum_artifacts_age: -1
  #    maximum_metadata_age: 1440
  #    negative_cache: true
  #    not_found_cache_ttl: 1440
  #    routing_rule: null

```

## Example Playbook

In this example the playbook will create two additional Nexus users and one additional Blob Storage.

```yaml
---
- name: Configure Nexus
  hosts: all
  user: ansible
  become: yes
  vars:
    config:
      users:
        - id: joan
          first_name: Joan
          last_name: Doe
          email: joan@example.org
          password: "{{ lookup('env', 'JOAN_PASSWORD') }}"
          status: active
          source: default
          roles:
            - nx-admin
        - id: joe
          first_name: Joe
          last_name: Doe
          email: joe@example.org
          password: nbusr123
          status: disabled
          source: default
          roles:
            - nx-anonymous
      stores:
        - name: file_blob
          type: file
          soft_quota: 0
          path: /mydata/blobs
          status: active
  roles:
    - role: lablabs.nexus_config
```

## License

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

See [LICENSE](LICENSE) for full details.

    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

      https://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.

## Author Information

Created in 2021 by [Labyrinth Labs](https://www.lablabs.io/)
